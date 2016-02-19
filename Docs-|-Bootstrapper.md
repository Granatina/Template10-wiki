Bootstapper (Library/Common/Bootstrapper.cs) is responsible for the core capabilities of Template 10. It derives from Application, and is implemented in your app in the App.xaml/App.xaml.cs files. 

It's responsibilities include:

1. Handling an extended splash screen
1. Creating the root frame
1. Creating the navigation service
1. Aggregating activation paths

In addition, it also handles:

1. Mapping keystrokes to navigation
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
1. Override `CreateNavigationService` - the return type of this override is a Template 10 INavigationService. If it is necessary for your to create the service in s a special way, including mocking, do it here.
 
> Note: the root frame and the subsequent navivgation service are created after the OnInitializeAsync override is called by the Bootstrapper. See below.

##Activation paths

The Bootstrapper seals away many of the overrides that ship with the out-of-the-box Application class. Where a standard UWP app overrides `OnActivated()`, `OnLaunched()`, and several other activation variations, Template 10 simplifies the startup pipeline to the following:

> `constructor` -> `OnInitializeAsync` -> `OnPrelaunchAsync()` -> `OnStartAsync()`

1. Application `constructor` - several Application properties, including `RequestedTheme` can only be set in the constructor of Application. Typically, applications setup global settings here.
1. `OnInitializeAsync` executes first. And it executes every time, even if the application is coming out of suspension. The type of code appropriate here would be authentication and cache checks. If the developer is creating a custom frame, spoiling the automatic implementation of the Bootstrapper, that code should also be here - since the Bootstrapper automatically creates the frame+navigation service after this override is called. 
1. `OnPrelaunchAsync` optionally executes before `OnStartAsync` and only during prelaunch. Prelaunch is a feature of the platform that launches an app for 16 seconds without UI, then suspends it. Prelaunch is not guaranteed, as it is influenced by available resources. A developer would handle Prelaunch if they need to prevent UI (for example: marking a user as 'available') that might occur on start. When handled, the developer can prevent `OnStartAsync` from continuing - if prevented, Template 10 will automatically call `OnStartAsync` when the app resumes from the prelaunch suspend. 
1. `OnStartAsync` if is the one and only entry point to an application that is not resuming from suspension. `OnStartAsync` combines every launch and activation scenario into a single, simple override. A developer can test the launch arguments of the method to determine the startup kind. A developer can also use the custom StartKind argument to the method to determine if the original startup method was intended to be launch or activate (if that is important to the startup logic). 

> Note: The helper method `DetermineStartCause` in Bootstrapper can help a developer determine the subtle startup causes that are otherwise unclear. For example, determining if the primary or secondary tile, or toast caused of the startup. 