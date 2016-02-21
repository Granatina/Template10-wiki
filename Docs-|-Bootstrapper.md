Bootstrapper (Library/Common/Bootstrapper.cs) is responsible for the core capabilities of Template 10. It derives from Application, and is implemented in your app in the App.xaml/App.xaml.cs files. 

Its responsibilities include:

1. Handling an extended splash screen
1. Creating the root frame
1. Creating the navigation service
1. Aggregating activation paths
1. Automate suspension management

In addition, it also handles:

1. Creating the initial window wrapper
1. Creating the initial dispatcher wrapper
1. Wrapping the root frame with a modal dialog
1. Support for dependency injection for view-models

Simplest implementation looks like this:

````csharp
namespace Sample
{
    sealed partial class App : Template10.Common.BootStrapper
    {
        public App() { InitializeComponent(); }

        public override Task OnStartAsync(StartKind startKind, IActivatedEventArgs args)
        {
            NavigationService.Navigate(typeof(Views.MainPage));
            return Task.CompletedTask;
        }
    }
}
````

> This implementation comes from the blank template. You can install all templates through the Visual Studio extension gallery. Simply search for "Template 10".

##Properties, methods and overrides

````csharp
// show shell button when necessary
bool ShowShellBackButton { get; set; }

// show shell button regardless of necessity
bool ForceShowShellBackButton { get; set; }

// refreshes shell button visiblity
void UpdateShellBackButton();

// automatically clear suspensionState
TimeSpan CacheMaxDuration { get; set; }

// expose automatic root wrapper 
ModalDialog ModalDialog { get; }

// default service for first frame
INavigationService NavigationService { get; }

// in-memory property bag
StateItems SessionState { get; set; }

// method to create splash ui
Func<SplashScreen, UserControl> SplashFactory { get; }

// event that exposes window created operations
event EventHandler<WindowCreatedEventArgs> WindowCreated;

// creates new navigation with new/existing frame
INavigationService NavigationServiceFactory();

// pipeline override, occurs even when resuming from terminated
Task OnInitializeAsync(IActivatedEventArgs args);

// pipeline override, occurs before OnStartAsync
Task OnPrelaunchAsync(IActivatedEventArgs args, out bool runOnStartAsync);

// pipeline override, occurs when resuming
void OnResuming(object s, object e, BootStrapper.AppExecutionState previousExecutionState);

// pipeline override, does not occur when resuming from terminated
Task OnStartAsync(BootStrapper.StartKind startKind, IActivatedEventArgs args);

// pipeline override, occurs when suspending
Task OnSuspendingAsync(object s, SuspendingEventArgs e, bool prelaunchActivated);

// dictionary of page keys for optional page key navigation
Dictionary<T, Type> PageKeys<T>() where T : struct, IConvertible;

// optional dependency injection endpoint for creating view-models
INavigable ResolveForPage(Page page, NavigationService navigationService);
````

##Splash page

Your app has a limited number of seconds (currently 10s) to activate before the platform automatically, punitively terminates your app. This means, costly operations cannot be front-loaded at app startup. A common work-around is the extended splash screen. This view mimics the default splash screen while executing long-running startup tasks. 

In the Bootstrapper, identify your extended splash factory, like this:

````csharp
sealed partial class App : Template10.Common.BootStrapper
{
    public App()
    {
        InitializeComponent();
        SplashFactory = (e) => new Views.Splash(e);
    }

    // runs only when not restored from state
    public override Task OnStartAsync(StartKind startKind, IActivatedEventArgs args)
    {
        NavigationService.Navigate(typeof(Views.MainPage));
        return Task.CompletedTask;
    }
}
````

> This implementation comes from the minimal template. You can install all templates through the Visual Studio extension gallery. Simply search for "Template 10".

##Root frame

The Bootstrapper automatically creates your app's root frame. However, the developer can intercept this by creating their own root frame in the OnInitializeAsync() override. Why? Sometimes, like when using the Hamburger Menu, you want a custom root to your application. If you want to do this, you can do something like this:

````csharp
public override Task OnInitializeAsync(IActivatedEventArgs args)
{
    // content may already be shell when resuming
    if ((Window.Current.Content as ModalDialog) == null)
    {
        // setup hamburger shell
        var nav = NavigationServiceFactory(BackButton.Attach, ExistingContent.Include);
        Window.Current.Content = new ModalDialog
        {
            DisableBackButtonWhenModal = true,
            Content = new Views.Shell(nav),
            ModalContent = new Views.Busy(),
        };
    }
    return Task.CompletedTask;
}
````

> This implementation comes from the hamburger template. You can install all templates through the Visual Studio extension gallery. Simply search for "Template 10".

##Navigation service

The navigation service is documented in the services wiki, however, the important thing to know here is that Template 10 relies on every XAML frame control to have a companion Template 10 navigation service. The Bootstrapper creates the root frame using several methods in concert: `InitializeFrameAsync` -> `CreateRootFrame` & `NavigationServiceFactory` -> `CreateNavigationService`. Developers can intercept this process along several steps:

1. Override `CreateRootFrame` - the return type of this override is a XAML Frame. If default content, properties, or registration in your app is custom, you can implement it here.
1. Override `CreateNavigationService` - the return type of this override is a Template 10 INavigationService. If it is necessary for you to create the service in a special way, including mocking, do it here.

````csharp
// runs only when not restored from state
public override Task OnStartAsync(StartKind startKind, IActivatedEventArgs args)
{
    NavigationService.Navigate(typeof(Views.MainPage));
    return Task.CompletedTask;
}
````
 
> Note: the root frame and the subsequent navigation service are created after the OnInitializeAsync override is called by the Bootstrapper. See below.

##Activation paths

The Bootstrapper seals away many of the overrides that ship with the out-of-the-box Application class. Where a standard UWP app overrides `OnActivated()`, `OnLaunched()`, and several other activation variations, Template 10 simplifies the startup pipeline to the following:

> `constructor` -> `OnInitializeAsync` -> `OnPrelaunchAsync()` -> `OnStartAsync()`

1. Application `constructor` - several Application properties, including `RequestedTheme` can only be set in the constructor of Application. Typically, applications setup global settings here.
1. `OnInitializeAsync` executes first. And it executes every time, even if the application is coming out of suspension. The type of code appropriate here would be authentication and cache checks. If the developer is creating a custom frame, spoiling the automatic implementation of the Bootstrapper, that code should also be here - since the Bootstrapper automatically creates the frame+navigation service after this override is called. 
1. `OnPrelaunchAsync` optionally executes before `OnStartAsync` and only during prelaunch. Prelaunch is a feature of the platform that launches an app for 16 seconds without UI, then suspends it. Prelaunch is not guaranteed, as it is influenced by available resources. A developer would handle Prelaunch if they need to prevent UI (for example: marking a user as 'available') that might occur on start. When handled, the developer can prevent `OnStartAsync` from continuing - if prevented, Template 10 will automatically call `OnStartAsync` when the app resumes from the prelaunch suspend. 
1. `OnStartAsync` if is the one and only entry point to an application that is not resuming from suspension. `OnStartAsync` combines every launch and activation scenario into a single, simple override. A developer can test the launch arguments of the method to determine the startup kind. A developer can also use the custom StartKind argument to the method to determine if the original startup method was intended to be launch or activate (if that is important to the startup logic). 

````csharp
public override Task OnStartAsync(StartKind startKind, IActivatedEventArgs args)
{
    var shareArgs = args as ShareTargetActivatedEventArgs;
    if (shareArgs != null)
    {
        var key = nameof(ShareOperation);
        SessionState.Add(key, shareArgs.ShareOperation);
        NavigationService.Navigate(typeof(Views.MainPage), key);
    }
    else
    {
        NavigationService.Navigate(typeof(Views.MainPage));
    }
    return Task.CompletedTask;
}
````

> The code above is from the Share Target sample project.

**Note**: The helper method `DetermineStartCause` in Bootstrapper can help a developer determine the subtle startup causes that are otherwise unclear. For example, determining if the primary or secondary tile, or toast caused of the startup. 

````csharp
public override Task OnStartAsync(StartKind startKind, IActivatedEventArgs args)
{
    switch (DetermineStartCause(args))
    {
        case AdditionalKinds.SecondaryTile:
            var tileargs = args as LaunchActivatedEventArgs;
            NavigationService.Navigate(typeof(Views.DetailPage), tileargs.Arguments);
            break;
        case AdditionalKinds.Toast:
            var toastargs = args as ToastNotificationActivatedEventArgs;
            NavigationService.Navigate(typeof(Views.DetailPage), toastargs.Argument);
            break;
        case AdditionalKinds.Primary:
        case AdditionalKinds.Other:
            NavigationService.Navigate(typeof(Views.MainPage));
            break;
    }
	return Task.CompletedTask;
}
````

> The code above is from the Toast and Tiles sample project.

##Suspension management

With UWP, an app may be suspended into memory at any time. Suspended apps may be terminated, due to resource constraints, at any time. Handling these states, their transitions, and providing a seamless user experience is the responsibility of the developer. Template 10 helps automate this.

Automatically, the Bootstrapper calls `OnNavigatedFrom` on every active view-model. In most apps, there will be only one active view-model; but, should there be more, Bootstrapper will call them all. Each call will be wrapped in a single Deferral, and enable await in the calls. Template 10 cannot extend the 10 second limitation imposed by the platform, so the developer remains responsible to limit the suspend-time operation.

````csharp
public override Task OnNavigatedFromAsync(IDictionary<string, object> suspensionState, bool suspending)
{
    if (suspending)
    {
        suspensionState[nameof(Value)] = Value;
    }
    return Task.CompletedTask;
}
````

Automatically, the Bootstrapper will save and restore the navigation state of every active navigation service. This means, when the app is restored from termination, the navigation stack (including the back and forward stacks) will be restored. The current page will be re-created, the `OnNavigatedTo` overrides will be called on the page and the view-model, and the suspensionState passed to those methods will be populated.

````csharp
public override Task OnNavigatedToAsync(object parameter, NavigationMode mode, IDictionary<string, object> suspensionState)
{
    if (suspensionState.Any())
    {
        Value = suspensionState[nameof(Value)]?.ToString();
    }
    return Task.CompletedTask;
}
````

In addition to those automatic operations, a developer may also use:

1. `OnSuspendingAsync()` in Bootstrapper is an override that is called after the OnNavigatedFrom() methods in every view-model are called. With whatever time remains, the developer can implement global logic to handle suspension.
1. `OnResume()` is in Bootstrapper is an override that is called when the application is resuming. A developer can check cache for staleness or anything else that makes sense, and is important to the specific application. The developer does NOT need to restore navigation state, unless they have introduced a custom navigation pattern. In addition to standard suspension, resume from prelaunch may also be handled here.
 
#Window wrapper

The window wrapper class is documented elsewhere, however, it is important to understand what each window wrapper maps to a  window. The platform refers to this as a view, but since we call XAML pages views, this is confusing. We do not mean XAML view, in this case, we mean something that has a title bar. Something we typically call a window. 

Since an app can have more than one window, there can also be more than one window wrapper. The Bootstrapper keeps track of the creation of windows and adds them to the static `ActiveWindows` collection in the window wrapper class. This occurs in the WindowCreated overload which is not available to developers using Bootstrapper. 

###Window created

If this is an important part of the lifecycle to your app, your code can handle the Bootstrapper's `WindowCreated` event, which will include the `WindowCreatedEventArgs` from the original override. This is an edge case most developers will not need.  

Here's the internal implementation:

````csharp
public event EventHandler<WindowCreatedEventArgs> WindowCreated;
protected sealed override void OnWindowCreated(WindowCreatedEventArgs args)
{
    DebugWrite();

    if (!WindowWrapper.ActiveWrappers.Any())
        Loaded();

    // handle window
    var window = new WindowWrapper(args.Window);
    WindowCreated?.Invoke(this, args);
    base.OnWindowCreated(args);
}
````

##Dispatcher wrapper

The dispatcher wrapper is documented elsewhere, however, here's the summary: the XAML CoreDispatcher is how an operation that is off the UI thread can execute an operation on the UI thread. The dispatcher is generally not available to non-UI threads, but can be accessed in Template 10 through the current window wrapper. The dispatcher wrapper, however, is really just a series of helper methods intended to make dispatching code simpler.

##Modal dialog

The modal dialog control is documented elsewhere, however, it's important to understand that it is used to display one of two content. The main content or the overlaying content, meant/intended to be an overlay of some kind, or a modal dialog. An example of this might be a login form or a busy indicator.  

The Bootstrapper automatically wraps the root frame in a modal dialog. It exposes this through the `Bootstrapper.ModalDialog` property. Here, a developer can set their own ModalContent and the ModalDialog's IsModal value. 

> Note: developers who intercept the standard Frame creation pipeline - for example, using the HamburgerMenu Shell approach - will find the Bootstrapper.ModalDialog property to be null. But this can be custom-implemented by the developer.

````csharp
public override Task OnInitializeAsync(IActivatedEventArgs args)
{
    // content may already be shell when resuming
    if ((Window.Current.Content as ModalDialog) == null)
    {
        // setup hamburger shell
        var nav = NavigationServiceFactory(BackButton.Attach, ExistingContent.Include);
        Window.Current.Content = new ModalDialog
        {
            DisableBackButtonWhenModal = true,
            Content = new Views.Shell(nav),
            ModalContent = new Views.Busy(),
        };
    }
    return Task.CompletedTask;
}
````

##Dependency injection

Dependency injection is a common design pattern that Template 10 supports, but does not natively implement. That being said, Template 10 enables dependency injection specifically with Bootstrapper.`ResolveForPage()`. Overriding this method, allows a developer to inject (or return) any `INavigable` view-model into a page immediately after initial navigation, while still maintaining the standard navigation pipeline.

// ENDOFFILE