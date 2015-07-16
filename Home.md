##Frequently Asked Questions about Template 10

* **Is Template 10 ready to use?** No. Not yet. Wait until Windows 10 RTM.

* **What is Template 10?** Template 10 is a better project template for Windows UWP apps. For 90% of app developers, Template 10 is the ideal project template. It was inspired by the Microsoft Patterns & Practices team.  

* **Why would I use Template 10?** There no good reason for you to use Template 10 until you understand it. If you aren't great at reverse-engineering our code in GitHub, then you better wait for documentation (soon).

* **Who is the author of Template 10?** Template 10 was born in Microsoft Developer Evangelism, but is ultimately the work of the Microsoft developer community. Any developer is welcome to join the team. 

* **Is Template 10 another control library?** No. WinRT XAML Toolkit, for example, is an awesome toolkit for XAML developers. There are others. Template 10 doesn't replace them; we wouldn't hesitate to use them. 

* **Is Template 10 another MVVM framework?** No. MVVM Light, Caliburn, Cross and others do a great job being MVVM frameworks. We leverage them. That being said, the [Template 10 Library] does ship with BindableBase.

* **Does Template 10 require MMVM?** No, but we certainly love MVVM and can hardly imagine why a developer would not use it. But if your project doesn't, that's cool. Template 10 doesn't require it in any way.

* **Does Template 10 require Dependency Injection?** No. But this is another one of those things that's difficult to see writing a Windows XAML app without. But, no, there's no dependency on DI in Template 10.

* **Is Template 10 free?** I guess, yes. Template 10 is an open source project for use in any project for any purpose. There is no copyright, no attribution or anything else. Take the code and use it as you need it.

* **What's the motivation behind Template 10?** Honestly? The OOB VS templates just make us angry. In Windows 8 templates had all kinds of insanity and in Windows 10 there's only Blank. Template 10 tries to strike a balance. 

* **Is Template 10 related to Microsoft Patterns & Practices' Prism?** No. But, yes - at least in spirit. Some of the code in Template 10 is highly inspired by the code from Prism for StoreApps (which was for Windows 8 apps). In addition, the APIs in Template 10 are intentionally reflective of Prism for WPF so our WPF-XAML brothers and sisters can find some familiarity in Windows UWP apps, as they begin to transition. But, Template 10 is written from scratch.

* **Can I contribute to Template 10?** Yes. Just submit a pull request. 

* **Can Template 10 work with existing apps?** Yes. Template 10 is enabled by the external [Template 10 Library], which can be referenced in and used by existing Windows app projects. 

* **Does Template 10 support Visual Basic?** Not as a project template, no. But, Template 10 is enabled by the external [Template 10 Library], it can be referenced in and used by Visual Basic apps.

* **How do I use the Template 10 project template?** [Updated 7/16/2015] The plan is to put the project template into the Visual Studio gallery within a week or so of Windows 10 RTM (7/29). In the meantime, you have two options. 1) you can clone the project and just use the blank project as your base. Or, 2) .. nope, just one option right now. Sorry. 

* **Is the Template 10 Library in NuGet yet?** No, not yet. We'll put it up there around Windows 10 RTM.

* **Will Template 10 use NuGet 2.0 or NuGet 3.0?** We're upgrading it now to NuGet 3.0. This means the project will require VS RTM which isn't released yet. So, we won't commit NuGet 3.0 changes for a while.

* **Where is the Template 10 wizard?** The Template 10 project template will eventually include a wizard to guide developers to tweak their Windows app project. We haven't built it yet, though.

* **What are Template 10 Services?** Because the Template 10 project template will install from the Visual Studio Gallery with a VSIX, we intend to extend Visual Studio with it, so Template 10 projects can add view services in their services folder with a special dialog (similar to how MCV projects add controllers). We haven't built it yet, though.

* **What's the future of Template 10?** Since we only just about to release, that's a crazy question. The truth is, we'll keep it up-to-date for Windows UWP apps, but it's services that will continue to grow.

* **Will Template 10 ever be OOB VS?** No. But, during the many distractions of the forthcoming zombie apocalypse we plan to seize the moment and sneak Template 10 into VS's main branch. But, until then, no. #patience