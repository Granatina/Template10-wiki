##Is Template 10 ready to use?

Yes. It sure is. _No part of Template 10 is in preview starting September 2015._

###What is Template 10?

Template 10 is three things. 

1. The Template 10 Windows app project templates in the Visual Studio gallery.
2. The Template 10 Windows app helper library in [NuGet](https://www.nuget.org/packages/Template10/).
3. The Template 10 Windows app technique samples in [GitHub](https://github.com/Windows-XAML/Template10).

> Template 10 is the brainchild of Microsoft Developer Evangelism and was started there. Lots of learnings from Windows 8, including lots from the Pattern's and Practices Prism.StoreApps framework are in the code base.

###Why are you saying T10 is convention-based?

Look. You know the Asp.NET MVP Visual Studio project? It has several empty folders intended to guide developers where to put their views, controllers, etc. Template 10 does this, too. Like MVC, we don't require you use our conventions, but you can. We believe that developers should be given guidance, not just code. Template 10 does that. Our conventions are simple and consistent. But, most importantly, optional. Have your own conventions? No problem. Otherwise, there are ours.

Our conventions are simple. Like this:

1. We out put views (XAML files) in a /Views folder (and ns)
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

1. Template10.Controls.PageHeader ([docs](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-PageHeader))
````XAML
<controls:PageHeader Text="Main Page" xmlns:controls="Template10.Controls"
    BackButtonVisibility="Collapsed" Frame="{x:Bind Frame, Mode=OneWay}" />
````
2. Template10.Controls.HamburgerMenu ([docs](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu))
````XAML
<Controls:HamburgerMenu x:Name="MyHamburgerMenu">
    <Controls:HamburgerMenu.PrimaryButtons>
        <Controls:HamburgerButtonInfo ClearHistory="True" PageType="views:MainPage">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Width="48" Height="48" Symbol="Home" />
                <TextBlock Margin="12,0,0,0" VerticalAlignment="Center" Text="Home" />
            </StackPanel>
        </Controls:HamburgerButtonInfo>
    </Controls:HamburgerMenu.PrimaryButtons>
</Controls:HamburgerMenu>
````
3. Template10.Controls.CustomTitleBar (docs)

**There are XAML behaviors and actions in Template 10.**

1. Template10.Behaviors.NavButtonBehavior (docs)
````XAML
<AppBarButton Icon="Forward" Label="Forward">
    <Interactivity:Interaction.Behaviors>
        <Behaviors:NavButtonBehavior Direction="Forward" Frame="{x:Bind Frame, Mode=OneWay}" />
    </Interactivity:Interaction.Behaviors>
</AppBarButton>
````
2. Template10.Behaviors.TextBoxEnterBehavior (docs)
````XAML
<TextBox>
    <Interactivity:Interaction.Behaviors>
        <Behaviors:TextBoxEnterKeyBehavior>
            <Core:CallMethodAction MethodName="GotoDetailsPage" TargetObject="{Binding}" />
        </Behaviors:TextBoxEnterKeyBehavior>
    </Interactivity:Interaction.Behaviors>
</TextBox>
````
3. Template10.Behaviors.TimeoutAction (docs)
````XAML
<Button>
    <Interactivity:Interaction.Behaviors>
        <Core:EventTriggerBehavior EventName="Click">
            <Core:CallMethodAction MethodName="ShowBusy" TargetObject="{Binding Mode=OneWay}" />
            <Behaviors:TimeoutAction Milliseconds="5000">
                <Core:CallMethodAction MethodName="HideBusy" TargetObject="{Binding Mode=OneWay}" />
            </Behaviors:TimeoutAction>
        </Core:EventTriggerBehavior>
    </Interactivity:Interaction.Behaviors>
</Button>
````

**There are XAML services in Template 10.**

1. Template10.Services.NavigationService (docs)
````csharp
// from inside the app.xaml.cs
this.NavigationService.Navigate(typeof(Views.MainPage));
            
// from inside a view-model
this.NavigationService.Navigate(typeof(Views.DetailPage), this.Value);

// from inside the primary window
var nav = Template10.Common.BootStrapper.Current.NavigationService;
nav.Navigate(typeof(Views.DetailPage), this.Value);
````
2. Template10.Services.KeyboardService (docs)
3. Template10.Services.SettingsService (docs)
4. Template10.Common.WindowWrapper (docs)
5. Template10.Common.DispatcherWrapper (docs)

> We plan to add more services to Template 10.

**There are XAML converters in Template 10.**

1. Template10.Converters.DateTimeFormatConverter (docs)
2. Template10.Converters.ValueWhenConverter (docs)

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
4. Microsoft (PFE) professional field engineers, worldwide.
5. Internal product and platform teams who contribute and advise.
6. Community developers, like you, who submit pull requests all the time.

> If you wonder if we accept community pull requests, we do. Without the help of the developer community, many enhancements and tweaks that make Template 10 great would not be present.

###Does Template 10 require MVVM?

No. Though it's difficult to imagine any XAML app not using model-view-view-model, there is nothing in Template 10 that requires you to use it. Template 10 is compatible with any MVVM framework, and ships with a mini framework that's probably enough for many of the apps out there.

> To leverage other MVVM frameworks, simply inherit from Template10.Mvvm.ViewModelBase or implement Template10.Services.NavigationService.INavagable. This enables OnNavigatedTo/From in your view-models.

##Is Template 10 right for me?

Probably. Template 10 is intended to remove the boilerplate garbage we all have to write over and over; it packs in as many time-saving lessons-learned as possible for app developers. It's perfect for brand new apps, or apps just wanting to leverage the library. That being said, Template 10 is not for everyone (probably around 90% of apps). 