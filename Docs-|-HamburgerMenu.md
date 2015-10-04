# The HamburgerMenu control 

The control to create a hamburger menu based navigation pattern in your application.

![](http://i.imgur.com/YXAtzYy.png)

## Contents overview
- [Properties](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#properties)
- [Implementation](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#implementation)
- [Customization](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#customization)
- [Buttons](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#buttons)
- [Implementing a shell](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#implementing-a-shell)
- [VisualStates for the HamburgerMenu](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-HamburgerMenu#visual-states-for-the-hamburgermenu)

## Inspiration

The Hamburger Menu approach is one of the most widely used navigation pattern nowadays in mobile applications and websites. It's made by panel, which is usually hidden, that contains the links to browse through the different sections of the applications. By tapping a button, the user is able to show or hide the panel, so that he can quickly jump from one section to another. The name of this navigation pattern comes from the icon that is used to show / hide the panel: three lines, one on top of the other, which resemble a hamburger placed in the middle of two pieces of bread.

Hamburger Menu is one of the many navigation patterns available in Windows 10 and it's effective when your application has multiple and separate sections.

## SplitView

The Universal Windows Platform has added a new control called **SplitView** to implement hamburger menu navigation's patterns. The goal of the SplitView control is to leave the maximum freedom to the developer, since it simply takes care of splitting the page in two sections:

- A panel, which can be placed in any margin of the page and that can behave in multiple ways (always visible, manually activated by the user, etc.)
- The main content of the page: typically, when the user selects one of the items in the panel, the main section is reloaded to display the selected page.

The downside of this freedom is that, if you want to implement a "standard hamburger menu experience" (like the one offered by the MSN News app), you'll have a lot of work to do.

The HamburgerMenu control helps you to quickly implement this kind of experience.

## Key features

- Support for Primary buttons / commands (displayed at the top of the panel; they provide access to the most frequently used sections of the application)
- Support for Secondary buttons / commands (displayed at the bottom of the panel with, optionally, a separator; they provide access to the least frequently used sections of th application).
- Built-in styles to quickly create the navigation buttons for the different sections of the application
- Easy look & feel style customization
- Works best with the PageHeader control

## Properties

- **HamburgerBackground** (SolidColorBrush)

> This property controls the background color of the button used to show / hide the panel of the hamburger menu.

- **HamburgerForeground** (SolidColorBrush)

>This property controls the foreground color of the button used to show / hide the panel of the hamburger menu.

- **NavAreaBackground** (SolidColorBrush)

>This property controls the background color of the panel.

- **NavButtonBackground** (SolidColorBrush)

>This property controls the background color of the button connected to the currently selected section.

- **NavButtonForeground** (SolidColorBrush)

>This property controls the foreground color of the button connected to the currently selected section.

- **SecondarySeparator** (SolidColorBrush)

>This property controls the color of the separator line which is added before the secondary commands.

- **NavigationService** (NavigationService)

>This property holds a reference to the NavigationService instance provided by Template10. It's required to handle the navigation between the different sections of the application.

- **PrimaryButtons** (IEnumerable)

> This property allows developers to add in the panel the main sections of the application, which are displayed at the top. Each section is represented with a NavigationButtonInfo control, which offers some built in features like predefined styles, automaticatic navigation, etc.

- **SecondaryButtons** (IEnumerable)

> This property works in the same way of the PrimaryButtons one, but it's used to add to the panel the secondary sections of the applications, which are displayed on the bottom.

- **IsOpen** (boolean)

> This property controls the visibility of the panel.

- **PaneWidth** (double)

> This property controls the size of the panel.

- **Selected** (NavigationButtonInfo)

> This property contains a reference to the selected section in the panel.

- **VisualStateNarrowMinWidth** (integer)

> This property indicates the width when the Narrow Visual State will be applied. This Visual State does only one thing, it completely hides the panel when the window is too small (like on a smarpthone).

- **VisualStateNormalMinWidth** (integer)

> This property indicates the width when the Normal Visual State will be applied. In this state, the SplitView control is displayed in Minimal mode.

## IMPLEMENTATION 

Before you can add the control, you must add the Template10.Controls namespace:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

</Page>
```

With the namespace in place, add the HamburgerMenu control to your page:

```XAML
<controls:HamburgerMenu x:Name="Menu" />
```

To properly support navigation, you need to assign a value to the NavigationService property of the control, like in the following sample:

```C#
public sealed partial class Shell : Page
{
    public Shell(NavigationService navigationService)
    {
        this.InitializeComponent();
        Menu.NavigationService = navigationService;
    }
}
```

You'll understand better how to pass a reference to the NavigationService object to a page in the next sections, when we'll discuss how to embed the HamburgerMenu into a shell.

## CUSTOMIZATION ##

The easiest customization properties of the HamburgerMenu control are:

- **HamburgerBackground** to define the background color of the button used to show / hide the panel.
- **HamburgerForeground** to define the foreground color of the button used to show / hide the panel.
- **NavAreaBackground** to define the background color of the panel.
- **NavButtonBackground** to define the background color of the highlighted section in the panel.
- **NavButtonForeground** to define the foreground color of the highlighted section.
- **SecondarySeparator** to define the color of the separator which is added before the secondary buttons.

For example, the following code:

```XAML
<controls:HamburgerMenu x:Name="Menu"
                    HamburgerBackground="#FFD13438"
                    HamburgerForeground="White"
                    NavAreaBackground="#FF2B2B2B"
                    NavButtonBackground="#FFD13438"
                    SecondarySeparator="White"
                    NavButtonForeground="White" />
```

will create the following result:

![](http://i.imgur.com/tFrrdA7.png)

## BUTTONS

The HamburgerMenu control offers an easy way to define the sections of your applications, by providing two categories of buttons:

- PrimaryButtons are displayed at the top of the panel and they are used to provide to the user quick access to the most used sections of the application.
- SecondaryButtons are displayed at the bottom of the panel and, as such, they are used to provide to the user access to the less frequently used sections of the applications (like Settings).

The original SplitView control allows developers to add, into the panel, arbitrary XAML, without any constraint. However, it doesn't provide built-in controls to quickly recrate the buttons you can find, for example, in the MSN News app.

Template10 provides a specific control for this purpose, called **NavigationButtonInfo**, which derives from the **RadioButton** control. RadioButton is the best choice for this scenario since:

- It already handles the fact that the current selected item should be highlighted.
- It already supports mutual selection: if one of the buttons in the group is selected, all the other are automatically unselected.

Here is how a sample HamburgerMenu implementation looks like:

```XAML
<controls:HamburgerMenu x:Name="Menu"
                HamburgerBackground="#FFD13438"
                HamburgerForeground="White"
                NavAreaBackground="#FF2B2B2B"
                NavButtonBackground="#FFD13438"
                SecondarySeparator="White"
                NavButtonForeground="White" >

    <controls:HamburgerMenu.PrimaryButtons>
        <controls:NavigationButtonInfo PageType="views:MainPage" ClearHistory="True">
            <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                <SymbolIcon Symbol="Home" Width="48" Height="48" />
                <TextBlock Text="Home" Margin="12, 0, 0, 0" VerticalAlignment="Center" />
            </StackPanel>
        </controls:NavigationButtonInfo>

        <controls:NavigationButtonInfo PageType="views:DetailPage" >
            <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                <SymbolIcon Symbol="Calendar" Width="48" Height="48" />
                <TextBlock Text="Calendar" Margin="12, 0, 0, 0" VerticalAlignment="Center" />
            </StackPanel>
        </controls:NavigationButtonInfo>
    </controls:HamburgerMenu.PrimaryButtons>

    <controls:HamburgerMenu.SecondaryButtons>
        <controls:NavigationButtonInfo PageType="views:DetailPage">
            <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                <SymbolIcon Symbol="Setting"  Width="48" Height="48"  />
                <TextBlock Text="Settings" Margin="12, 0, 0, 0" VerticalAlignment="Center"/>
            </StackPanel>
        </controls:NavigationButtonInfo>
    </controls:HamburgerMenu.SecondaryButtons>

</controls:HamburgerMenu>
```

The NavigationButtonInfo control has the following properties:

- **PageType** is a reference to the page type connected to the section. When the user taps on this button, he will be automatically redirected to that page.
- **PageParameter** is an optional parameter which can be passed to the destination page and retrieved using the **OnNavigatedTo()** event handler.
- **ClearHistory** is a boolean. When it's set to true, the navigation to the selected page will also clear the back stack. This is tipically applied to the button connected to the main page of the application, to avoid circular navigations.

The look & feel of the button is up to the developer, who can use an arbitrary XAML to define its layout. The sample shows a standard implementation using a symbol (with the **SymbolIcon** control) and a label (with a **TextBlock** control). This implementation makes easier to recreate the look & feel of many native application: when the panel is closed, only the symbol will be displayed; when the panel is opened, both the symbol and the label will be displayed.
 
![](http://i.imgur.com/xYkujJ3.png)

## IMPLEMENTING A SHELL

The HamburgerMenu is a XAML control and, as such, can be placed in any page of the application. However, since it's used to provide a global navigation pattern, it's unlikely that it will be placed in just one page of the application. To provide a seamless experience, you would need to place the same control in every page of your application. However, this approach would have many downsides: redundancy, harder to mantain, etc.

A better approach is to place the HamburgerMenu control into a **shell**, which is a page that will be used as a container in replacement of the standard Frame of the application. The standard behavior in a Universal Windows app is to set, as content of the main Window, an empty frame, which will host all the pages of the application and provide navigation capabilities between them.
With this new approach, we're going to use a new page, which will contain the HamburgerMenu control, as a content of the main window. All the other pages of the application will be hosted by this new container.

The first step is to add a new XAML page in your project, which will contain just the definition of the HamburgerMenu control, like in the following sample:

```XAML
<Page
    x:Class="HamburgerSample.Views.Shell"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:views="using:HamburgerSample.Views"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    xmlns:controls="using:Template10.Controls"
    mc:Ignorable="d">

    <controls:HamburgerMenu x:Name="Menu">

        <controls:HamburgerMenu.PrimaryButtons>
            <controls:NavigationButtonInfo PageType="views:MainPage" ClearHistory="True">
                <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                    <SymbolIcon Symbol="Home" Width="48" Height="48" />
                    <TextBlock Text="Home" Margin="12, 0, 0, 0" VerticalAlignment="Center" />
                </StackPanel>
            </controls:NavigationButtonInfo>

            <controls:NavigationButtonInfo PageType="views:DetailPage" >
                <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                    <SymbolIcon Symbol="Calendar" Width="48" Height="48" />
                    <TextBlock Text="Calendar" Margin="12, 0, 0, 0" VerticalAlignment="Center" />
                </StackPanel>
            </controls:NavigationButtonInfo>
        </controls:HamburgerMenu.PrimaryButtons>

        <controls:HamburgerMenu.SecondaryButtons>
            <controls:NavigationButtonInfo PageType="views:DetailPage">
                <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
                    <SymbolIcon Symbol="Setting"  Width="48" Height="48"  />
                    <TextBlock Text="Settings" Margin="12, 0, 0, 0" VerticalAlignment="Center"/>
                </StackPanel>
            </controls:NavigationButtonInfo>
        </controls:HamburgerMenu.SecondaryButtons>

    </controls:HamburgerMenu>
</Page>
```

The only operation to do in code behind is, as we've previously seen, to assign the NavigationService property of the HamburgerMenu control:

```C#
public sealed partial class Shell : Page
{
    public Shell(NavigationService navigationService)
    {
        this.InitializeComponent();
        Menu.NavigationService = navigationService;
    }
}
```
Now we need to:

1. Define this new page as main content of the application's window, in replacement of the standard Frame
2. Pass a reference to the NavigationService to the page, so that it can be assigned to the HamburgerMenu control

Both objectives can be achieved in the bootstrapper class, which provides a method called **OnInitializeAsync()** that is invoked every time the app is initialized, right before triggering the navigation to main page. Here is how we can override this method for our purpose:

```C#
sealed partial class App : BootStrapper
{
    public App()
    {
        this.InitializeComponent();
    }

    public override Task OnInitializeAsync(IActivatedEventArgs args)
    {
        var nav = NavigationServiceFactory(BackButton.Attach, ExistingContent.Include);
        Window.Current.Content = new Views.Shell(nav);
        return Task.FromResult<object>(null);
    }

    public override Task OnStartAsync(BootStrapper.StartKind startKind, IActivatedEventArgs args)
    {
        NavigationService.Navigate(typeof(Views.MainPage));
        return Task.FromResult<object>(null);
    }
}
```

**Window.Current.Content** is the property that holds a reference to the main frame of the application. By default, it simply contains a new instance of the Frame class. For our scenario, we need to override this behavior and to assign to the property a new instance of the page we've created to act as a shell.

To get a reference to the NavigationService we need for the HamburgerMenu control we use a method, provided by the BootStrapper class, called **NavigationServiceFactory**: we pass the returned object to the constructor of the shell page.

After these changes, we'll notice that the application, at startup, will be indeed redirected to the main page (as per the navigation triggered in the **OnStartAsync()** method), but the HamburgerMenu control defined in the shell page will be visible too.

## PAGEHEADER AND THE HAMBURGERMENU
The HamburgerMenu control is a great companion of the PageHeader one, also provided by Template10. You can learn more about how to use them together in the [PageHeader documentation](https://github.com/Windows-XAML/Template10/wiki/Docs-%7C-PageHeader#pageheader-and-the-hamburgermenu)

## VISUAL STATES FOR THE HAMBURGERMENU

The HamburgerMenu control has two built-in visual states - VisualStateNormal and VisualStateNarrow. 

1. In VisualStateNormal, the HamburgerMenu applies the Minimal layout to the SplitView control. It means that the full panel is closed, but it's always visible in minimal mode, which means that only the icons of the buttons are visible.
2. In VisualStateNarrow, the HamburgerMenu is completely hidden, except for the Hamburger button that shows / hides the panel.

## Controlling the Visual States ##

You can control the HamburgerMenu's visual states by defining the minimum widths that triggers them. The VisualStateNarrowMinWidth and VisualStateNormalMinWidth properties accomplish this.

You can apply these values like this:

```XAML
<Page
    x:Class="HamburgerSample.Views.Shell"
    xmlns:controls="using:Template10.Controls">

    <controls:HamburgerMenu x:Name="Menu"
               VisualStateNarrowMinWidth="0"
               VisualStateNormalMinWidth="800" />

</Page>
```

In the case above, when the width of the window is greater than 0 effective pixels but less than 800, the VisualStateNarrow visual state will be triggered. In this scenario, the panel will be hidden and only the hamburger button will be visible. Concurrently, when the width of the window is equal to or greater than 800 effective pixels, the VisualStateNormal visual state will be triggered. In this case, the panel will be visible in minimal mode.

![](http://i.imgur.com/uRfQur0.gif)