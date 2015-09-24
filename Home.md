##Is Template 10 ready to use?

It sure is. No part of Template 10 is in preview.

###What is Template 10?

Template 10 is three things. 

1. The Template 10 Windows app project templates in the Visual Studio gallery.
2. The Template 10 Windows app helper library in [NuGet](https://www.nuget.org/packages/Template10/).
3. The Template 10 Windows app technique samples in [GitHub](https://github.com/Windows-XAML/Template10).

> Template 10 is the brainchild of Microsoft Developer Evangelism and was started there. Lots of learnings from Windows 8, including lots from the Pattern's and Practices Prism.StoreApps framework are in the code base.

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

Probably. Template 10 is intended to remove the boilerplate stuff and to pack in as many time-saving lessons-learned as possible for app developers. It's perfect for brand new apps, or apps just wanting to leverage the library. That being said, Template 10 is not for everyone (probably around 90% of apps). 