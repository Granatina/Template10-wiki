###What is Template 10?

![T10](https://raw.githubusercontent.com/Windows-XAML/Template10/master/Assets/T10%2056x56.png) 

Template 10 is a set of Visual Studio project templates. They sling-shot developer productivity by getting ~80% of the boilerplate stuff delivered in the template - things like navigation, suspension, and even a Hamburger control. 

1. `Template 10 (Blank)` project template ([link](https://visualstudiogallery.msdn.microsoft.com/60bb885a-44e9-4cbf-a380-270803b3f6e5))
1. `Template 10 (Minimal)` project template ([link](https://visualstudiogallery.msdn.microsoft.com/60bb885a-44e9-4cbf-a380-270803b3f6e5))

Template 10 is intended for Window XAML apps written in C#. You can install Template 10 by searching for "Template 10" in the Visual Studio 2015 Extension Manager. Once installed, Template 10 templates will show up in the New Project dialog.

Template 10 templates share the Template 10 library in NuGet. This hosts the serviceable code and keeps the templates simple. It's a library that our templates or existing projects can include in their apps. 

1. `Template 10 (library)` in NuGet ([link](http://www.nuget.org/packages/Template10/))

###Credit where credit is due

Template 10 is the brainchild of Microsoft Developer Evangelism and was started there. Lots of learnings from Windows 8, including lots from the Pattern's and Practices Prism.StoreApps framework are in the code base.

###Why are you saying T10 is convention-based?

The philosophy is this: we want you do it our way unless you want to do it your way. You are the developer and only you know your app well enough to make big decisions. But, without a good reason to do it another way, do it our way. It's not about telling you to do it our way, it's about telling you to do it our way unless you don't want to. It's about guidance. It's about conventions. And, it's about flexibility.

**Here are some of our conventions:**

1. We put views (XAML files) in a /Views folder (and ns)
1. We only have one view-model for one view
1. We put our view-models in a /ViewModels folder (and ns)
1. We use OnNavigatedTo in view-models, not pages
1. We put our models in a /Models folder (and ns)
1. We often use the façade pattern with our models
1. We navigate using a NavigationService
1. We communicate with a Messenger
1. We like Dependency Injection
1. We use Template 10 ;-)

###What's in Template 10?

There is a lot to Template 10, but it's actually very basic.

> Note: many of the (docs) are still being written.

**There are XAML controls in Template 10.**

See [documentation](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Controls)

**There are XAML behaviors and actions in Template 10.**

See [documentation](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Behaviors).

**There are XAML services in Template 10.**

See [documentation](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Services)

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

4. Template10.Common.WindowWrapper (docs)
5. Template10.Common.DispatcherWrapper (docs)

> We plan to add more services to Template 10.

**There are XAML converters in Template 10.**

1. Template10.Converters.DateTimeFormatConverter ([docs](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Converters#datetimeformatconverter))
2. Template10.Converters.ValueWhenConverter ([docs](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Converters#valuewhenconverter))

**There are basic MVVM classes in Template 10.**

1. Template10.Mvvm.BindableBase (docs)
2. Template10.Mvvm.DelegateCommand (docs)
3. Template10.Mvvm.ViewModelBase (docs)

> Template 10 is not intended to be a new MVVM Framework.

**There are developer utilities in Template 10.**

1. Template10.Utils.MonitorUtil (docs)
2. Template10.Utils.ColorUtil (docs)
3. Template10.Utils.TypeUtil (docs)
4. Template10.Utils.XamlUtil (docs)

**There are Visual Studio project templates in Template 10.**

1. Windows/Universal/Template 10/Template 10 (Blank) (docs)
2. Windows/Universal/Template 10/Template 10 (Minimal) (docs)

> Projects are meant for greenfield projects, and learning

**There is a library in NuGet with Template 10.**

1. http://nuget.org/packages/template10

> The library can be added to existing Windows apps

**There are samples in Template 10.**

1. Samples/MvvmLight (docs)
2. Samples/Search (and Login) (docs)
3. Samples/Tiles (and Toast) (docs)
4. Samples/TitleBar (docs)

###Where have I heard about Template 10?

Template 10 shows up a lot of places.

1. On Microsoft Virtual Academy's Developer's Guide to Windows 10 (link)
2. On Microsoft official Windows 10 Hands on Labs (link)
3. In Microsoft Press Windows 10 exam-prep books (link)

###Who's contributing to Template 10?

Template 10 has a lot of authors.

1. Jerry Nixon, co-author of Developer's Guide to Windows 10.
2. Daren May, co-author of official Windows 10 hands on labs.
3. Robert Evans, lead field engineer and consultant for Windows apps.
4. Microsoft (PFE) premier field engineers, worldwide.
5. Internal product and platform teams who contribute and advise.
6. Community developers, like you, who submit pull requests all the time.

> If you wonder if we accept community pull requests, we do. Without the help of the developer community, many enhancements and tweaks that make Template 10 great would not be present.

###Does Template 10 require MVVM?

No. Though it's difficult to imagine any XAML app not using model-view-view-model, there is nothing in Template 10 that requires you to use it. Template 10 is compatible with any MVVM framework, and ships with a mini framework that's probably enough for many of the apps out there.

> To leverage other MVVM frameworks, simply inherit from Template10.Mvvm.ViewModelBase or implement Template10.Services.NavigationService.INavagable. This enables OnNavigatedTo/From in your view-models.

##Is Template 10 right for me?

Probably. Template 10 is intended to remove the boilerplate garbage we all have to write over and over; it packs in as many time-saving lessons-learned as possible for app developers. It's perfect for brand new apps, or apps just wanting to leverage the library. That being said, Template 10 is not for everyone (probably around 90% of apps). 