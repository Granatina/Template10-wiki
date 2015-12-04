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

1. There are [controls](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Controls)
1. There are [behaviors](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Behaviors).
1. There are [services](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Services)
1. There are [converters](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-Converters)
1. There are MVVM classes (BindableBase, DelegateCommand, and ViewModelBase)
1. There are utility classes (Template.Utils)
1. There are project templates (Blank, Minimal)
1. There is a NuGet (library)[http://nuget.org/packages/template10]
1. There are samples, many samples!

###Where have I heard about Template 10?

1. On Microsoft Virtual Academy's Developer's Guide to Windows 10 
1. On Microsoft official Windows 10 Hands-on-Labs 
1. At the Microsoft Windows 10 Tour events
1. In Microsoft Press Windows 10 exam-prep books 

###Who's contributing to Template 10?

1. Jerry Nixon, co-author of Developer's Guide to Windows 10.
2. Daren May, co-author of official Windows 10 hands on labs.
3. Robert Evans, lead field engineer and consultant for Windows apps.
4. Microsoft (PFE) premier field engineers, worldwide.
5. Internal Microsoft product teams who contribute and advise.
5. Internal Microsoft platform teams who contribute and advise.
6. Community developers, like you, who submit pull requests all the time.

###Does Template 10 require MVVM?

No. Though it's difficult to imagine any XAML app not using model-view-view-model, there is nothing in Template 10 that requires you to use it. Template 10 is compatible with any MVVM framework.

> To leverage other MVVM frameworks, simply inherit from Template10.Mvvm.ViewModelBase or implement Template10.Services.NavigationService.INavagable. This enables OnNavigatedTo/From in your view-models.

##Is Template 10 right for me?

Probably. Template 10 packs in as many time-saving lessons-learned as possible. It's perfect for new apps, or apps wanting to leverage the library. That being said, it's not for everyone (probably 90% of Windows XAML apps). 

##Known Issues with Template10
1. Template10 is not currently compatible with UWP1.1 (SDK version 10586). It is necessary to target the older SDK version to build production apps using Template 10, at least for the moment.
