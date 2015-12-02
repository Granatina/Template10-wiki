##Services

A service is a functionality wrapper, sometimes called a View Service.

###Table of contents

1. [NavigationService](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Services#navigationservice)
1. [KeyboardService](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Services#keyboardservice)
1. [SettingsService](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Services#settingsservice)

###NavigationService
The intent of the `NavigationService` is to centralize `Frame` interaction and to ensure basic behaviors occur, such as calling `OnNavigatedTo` in your page's view-model before navigating to the page. The `NavigationService` is testable, because it is interface-based, and it corrects a few nuisance flaws in the native XAML `Frame`.

````csharp
// from inside the app.xaml.cs
this.NavigationService.Navigate(typeof(Views.MainPage));
            
// from inside a view-model
this.NavigationService.Navigate(typeof(Views.DetailPage), this.Value);

// from inside any window
var nav = WindowWrapper.Current().NavigationService;
nav.Navigate(typeof(Views.DetailPage), this.Value);

// from/with a reference to a Frame
var nav = WindowWrapper.Current(MyFrame).NavigationService;
nav.Navigate(typeof(Views.DetailPage), this.Value);
````

> Using the Template 10 NavigationService ensures your app stays in sync with the BootStrapper and if you use view-models, implement INavigable, and set it as the value of your Page.DataContext, its OnNavigatedTo override will be called and passed any parameter used with Navigate(). 

###KeyboardService
The intent of the `KeyboardService` is to provide an abstracted way of reliably handling keyboard input. It was initially created to handle the back and forward gestures in the `NavigationService` but has been abstracted in this service for general use.

`documentation needed`

###SettingsService
The intent of the `SettingService` is to provide an abstracted way to interacting with service that properly supports serialization and an interface implementation for unit testing purposes.

````csharp
// from inside a view-model
 Services.SettingsServices.SettingsService _settings;
 ...
 public bool UseLightThemeButton
        {
            get { return _settings.AppTheme.Equals(ApplicationTheme.Light); }
            set { _settings.AppTheme = value ? ApplicationTheme.Light : ApplicationTheme.Dark; base.RaisePropertyChanged(); }
        }
````

