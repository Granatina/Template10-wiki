##Is Template 10 ready to use?

It sure is. No part of Template 10 is in preview.

###What is Template 10?

Template 10 is three things. 

1. The Template 10 Windows app project templates in the Visual Studio gallery.
2. The Template 10 Windows app helper library in [NuGet](https://www.nuget.org/packages/Template10/).
3. The Template 10 Windows app technique samples in [GitHub](https://github.com/Windows-XAML/Template10).

> Template 10 is the brainchild of Microsoft Developer Evangelism and was started there. Lots of learnings from Windows 8, including lots from the Pattern's and Practices Prism.StoreApps framework are in the code base.

###What's in Template 10?

There is a lot to Template 10, but it's actually very basic.

**There are XAML controls in Template 10.**

1. [Template10.Controls.PageHeader](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-PageHeader)
2. [Template10.Controls.HamburgerMenu](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu)
3. Template10.Controls.CustomTitleBar

**There are XAML behaviors and actions in Template 10.**

1. Template10.Behaviors.NavButtonBehavior
2. Template10.Behaviors.TextBoxEnterBehavior
3. Template10.Behaviors.TimeoutAction

**There are XAML services in Template 10.**

1. Template10.Services.NavigationService
2. Template10.Services.KeyboardService
3. Template10.Services.SettingsService

> We plan to add more services to Template 10.

**There are XAML converters in Template 10.**

1. Template10.Converters.DateTimeFormatConverter
2. Template10.Converters.ValueWhenConverter

**There are basic MVVM classes in Template 10.**

1. Template10.Mvvm.BindableBase
2. Template10.Mvvm.DelegateCommand
3. Template10.Mvvm.ViewModelBase

> Template 10 is not intended to be a new MVVM Framework.

**There are developer utilities in Template 10.**

1. Template10.Utils.MonitorUtil
2. Template10.Utils.ColorUtil
3. Template10.Utils.TypeUtil
4. Template10.Utils.XamlUtil

**There are Visual Studio project templates in Template 10.**

1. Windows/Universal/Template 10/Template 10 (Blank)
2. Windows/Universal/Template 10/Template 10 (Minimal)

> Projects are meant for greenfield projects, and learning

**There is a library in NuGet with Template 10.**

1. http://nuget.org/packages/template10

> The library can be added to existing Windows apps

**There are samples in Template 10.**

1. Samples/MvvmLight
2. Samples/Search (and Login)
3. Samples/Tiles (and Toast)
4. Samples/TitleBar

###Where have I heard about Template 10?

Template 10 shows up a lot of places.

1. On Microsoft Virtual Academy's Developer's Guide to Windows 10
2. On Microsoft official Windows 10 Hands on Labs
3. In Microsoft Press Windows 10 exam-prep books

###Who's contributing to Template 10?

Template 10 has a lot of authors.

1. Jerry Nixon, co-author of Developer's Guide to Windows 10.
2. Daren May, co-author of official Windows 10 hands on labs.
3. Microsoft (PFE) professional field engineers, worldwide.
4. Internal product and platform teams who contribute and advise.
5. Community developers, like you, who submit pull requests.

> If you wonder if we accept community pull requests, we do. Without the help of the developer community, many enhancements and tweaks that make Template 10 great would not be present.

###Does Template 10 require MVVM?

No. Though it's difficult to imagine any XAML app not using model-view-view-model, there is nothing in Template 10 that requires you to use it. Template 10 is compatible with any MVVM framework, and ships with a mini framework that's probably enough for many of the apps out there.

> To leverage other MVVM frameworks, simply inherit from Template10.Mvvm.ViewModelBase or implement Template10.Services.NavigationService.INavagable. This enables OnNavigatedTo/From in your view-models.

##Is Template 10 right for me?

Probably. Template 10 is intended to remove the boilerplate garbage we all have to write over and over; it packs in as many time-saving lessons-learned as possible for app developers. It's perfect for brand new apps, or apps just wanting to leverage the library. That being said, Template 10 is not for everyone (probably around 90% of apps). 