## WELCOME

There was a recurring pattern to the Windows UWP app questions I received and the answers I was giving. The questions about simple start-up stuff were endlessly recurring, and my answers were tirelessly preceded with painful comments like, “First you need to figure out navigation” or “First you need to figure out suspension” or fill-in-the-blank with boilerplate code. 

> Jump to [known issues](https://github.com/Windows-XAML/Template10/wiki/Welcome-%7C-Nuget#known-issues).

### GENESIS

This was the genesis of Template 10. Originally, it was only a project template, but as we wrote boilerplate solutions that every project would need, it became quickly clear a nuGet library was developing. The library is intended to enable the Template 10 project templates as well as be perfectly fine for existing projects who want the many benefits it brings.

![](https://raw.githubusercontent.com/Windows-XAML/Template10/master/Assets/T10%201366x768.png)

> https://www.nuget.org/packages/Template10

### WE LOVE CONVENTIONS

Template 10 was truly inspired by the Asp.NET MVC project template in Visual Studio. What’s terrific about that project template is that it includes empty folders. Those give direction on conventions for where files and classes belong – if a developer doesn’t already have an established pattern. Template 10 does the same thing, shipping with empty folders like Converts and Models to make simple things no-brainers. And, like MVC, Template 10 doesn't require you follow our folder conventions.

### WE'RE WARMING-UP TO OPEN SOURCE

Though some Microsoft people are contributors to this project, it’s important you realize that this is open source and a product of community contribution. This is awesome, by the way, because we can tweak the holy crap out of this code base from the trial and errors of scores of developers using Template 10. And, who benefits? Every single one of us. 

> As an aside, we're also warming-up to GitHub. Not bad.

### WE LOVE MVVM

Template 10 is not an MVVM framework and it does not require MVVM. That being said, Template 10 loves MVVM and enables developers who use the pattern. The most significant way this is true is with Template 10’s INavigable interface intended for view-models. Once implemented, view-models have OnNavigatedTo, OnNavigatedFrom, and OnNavigatingFrom methods, just like a XAML page. 

> If you use an MVVM framework like Light, Caliburn, Cross, or Prism, you will be happy to learn that these can work in peaceful coexistence with Template 10. This was by design. There is no up-front requirement for MVVM in Template 10 and once you start to use the pattern, there is no requirement on a specific framework or set of frameworks.

As a final note, just because there is no requirement for MVVM doesn’t mean we don’t recommend it. Since MVVM is so fundamentally compatible with XAML applications, it boggles our minds as to why even the simplest app wouldn’t just throw together a view-model to handle data interactions. If MVVM sounds complex to you, I think you should look again, it’s as simple as it comes. Somebody's been showing you all wrong.

> You might be saying, "Okay, you show me, then". No. This is not for that. If you want to do some real learning you can head over to http://microsoftvirtualacademy.com and get all the free training you can bear. Don't forget there are tons of public blogs and you could even pay for plural sight, if you need even more.

### WE DON'T LOVE APP.XAML.CS : Application

The first thing developers will see in Template 10 is that XAML’s Application object is sub-classed.  Template 10 apps inherit from BootStrapper in their App.xaml.cs, not Application. Why? Because the implementation required by Application is boilerplate and brittle. With Template 10, the startup pipeline is simplified from a dozen Launch and Activated overrides to a single, unified OnStartAsync() override. 

Our OnStartAsync() doesn’t hide anything from you. Every argument and parameter is still available to the developer, just the web of startup vectors is refactored to one. This lets you build applications and focus on the logic that matters instead: yours. This isn’t a commentary on the Application class. Building a platform is complex, and supporting every scenario is hard.

### WE'RE NOT FOR EVERYONE

We believe that Template 10 is ideal for about 80% of the Windows UWP apps out there. The work that we have done is not only good, it’s also common. Most apps would benefit from incorporating the library at once. That being said, we also recognize that 20 percent of the developers out there probably have their own, custom frameworks and we’re not a fit for them. That’s cool.

### THE Template 10 MISSION

The reason Template 10 exists is NOT to change your framework if you already have one. It’s to provide one for those developers who don’t have one or are unhappy with the one they are using. We want to be simple. We want to be prescriptive, and we want to be as flexible as we can without violating the patterns we want developers to actually adopt.

Template 10 is a labor of love by the developers writing it. 

Have a great time using Template 10.

## Known issues

For the time being, there are a few known issues in this release of the library.

1. There is no support for x64. It's coming, just not in this version. This has to do with the internal structure change to NuGet packs. There's only a little documentation for this. We'll figure it out soon.
2. Styles for our custom controls don't properly appear in Mobile. We believe this is a big in Mobile and not a big in the library. But a new build of mobile is coming and we will test it there.
