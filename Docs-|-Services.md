##Services

Lorem ipsum

###Table of contents

1. NavigationService
1. KeyboardService
1. SettingsService

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

Lorem ipsum

###SettingsService

Lorem ipsum
