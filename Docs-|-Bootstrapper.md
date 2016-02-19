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

1. Override **CreateRootFrame** - the return type of this override is a XAML Frame. If default content, properties, or registration in your app is custom, you can implement it here.
1. Override **CreateNavigationService** - the return type of this override is a Template 10 INavigationService. If it is necessary for your to create the service in s a special way, including mocking, do it here.

##Activation paths

The Bootstrapper seals away many of the overrides that ship with the out-of-the-box Application class. Where a standard UWP app overrides OnActivated(), OnLaunched(), and several other activation variations, Template 10 simplifies the startup pipeline to the following:

1. Application constructor
1. OnInitializeAsync()
1. OnPrelaunchAsync()
1. OnStartAsync()
1