##Controls

Lorem Ipsum

### Table of contents

1. Template10.Controls.PageHeader 
1. Template10.Controls.HamburgerMenu 
1. Template10.Controls.CustomTitleBar
1. Template10.Controls.ModalDialog 
1. Template10.Controls.Resizer 
1. Template10.Controls.PiePiece 

###PageHeader
The control to create a common page element including header text, primary buttons, and secondary buttons. 

![](http://i.imgur.com/BFG3pSB.png)

#### Inspiration

Every page needs some kind of header/title. It's the boilerplate code developers write over and over. The Template 10 PageHeader does this. It is inspired by the design of Microsoft's MSN News app. The Template 10 PageHeader control is 90% representative of the that implementation, but not identical. You'll like it.

#### CommandBar

In Universal Windows Platform the CommandBar control can be placed anywhere - not just top and bottom, like in Windows 8.x. PageHeader is a UI control that extends the CommandBar. The PageHeader allows developers to create a uniform page UI with primary and secondary buttons presented in a consistent, easy way.

#### Key features

- Support for Primary buttons/commands (always visible)
- Support for Secondary buttons/commands (hidden until the ellipse is tapped)
- Standard on-canvas back button, wired to navigate
- Standard on-canvas forward button, wired to navigate
- Easy look & feel style customization

#### Properties

- **BackButtonVisiblity** (Visible|Collapsed) default=Collapsed

> This property controls the visibility of the on-canvas back button. The on-canvas back button requires the PageHeader.Frame property to be set in order to function properly.

- **Content** (object)

> This property allows ****for advanced scenarios for developers. With the Text property, the developer can set a simple string header. Content allows for any XAML control for a custom experience.

- **Frame** (XAML Frame)

> This property sets the context for the on-canvas back button. This property not only enables the back button but allows it to operate against any off-canvas frame, if desired.

- **HeaderBackgroundBrush** (Brush)

> This property overrides the default background of the PageHeader. This property can be applied through a style to allow developers the ability to theme their application.

- **HeaderForegroundBrush** (Brush)

> This property overrides the default foreground (text) of the PageHeader. This property can be applied through a style to allow developers the ability to theme their application.

- **PrimaryButtons** (IEnumerable<IAppBarItem>)

> This property allows developers to add any IAppBarItem to the always-visible collection of buttons in the header. This, in effect, is the PrimaryButtons property of the CommandBar.

- **SecondaryButtons** (IEnumerable<IAppBarItem>)

> This property allows developers to add any IAppBarItem to the collection of buttons only visible when the user clicks the ellipses button. This, in effect, is the SecondaryButtons property of the CommandBar.

- **Text** (string)

> This simple property sets the header text for the control. The text is displayed on the left side of the header and does not wrap. The color of the font is controlled by the HeaderForegroundBrush property.

- **VisualStateNarrowMinWidth** (integer)

> This property indicates the width when the Narrow Visual State will be applied. This Visual State does only one thing, it increases the left margin of the Text by 48 pixels to accommodate a hamburger button.

- **VisualStateNormalMinWidth** (integer)

> This property indicates the width when the Normal Visual State will be applied. This Visual State has no visual impact on the PageHeader other than removing the effects of the VisualStateNarrowMinWidth property. 

#### Syntax

Before you can add the control, you must add the Template10.Controls namespace:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

</Page>
```

With the namespace in place, add the PageHeader control to your page:

> Remember that setting the Frame property on the PageHeader is important if you intend to use the Back navigation button functionality built-in to the PageHeader control.

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Frame="{x:Bind Frame}">

</Page>
```

Your initial UI will look like this:

![](http://i.imgur.com/BFG3pSB.png)

####Customization

The easiest customization properties of the PageHeader control are:

- **Text** to define the text displayed in the header (typically, the title of the page)
- **HeaderForeground** to define the text color of the header
- **HeaderBackground** to define the background color of the header

For example:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Frame="{x:Bind Frame}" 
        Text="Main Page" 
        HeaderBackground="Orange" 
        HeaderForeground="Red" />

</Page>
```
Your custom UI will look like this:

![](http://i.imgur.com/xvwCFXf.png)

####Navigation

Built-in navigation is a handy PageHeader behavior. The BackButtonVisibility property lets the developer turn this functionality on. Then, the back button has its own logic to hide itself (discussed below). 

Specifically:

1. The developer is responsible to connect the PageHeader control to the XAML Frame using the PageHeader.Frame property. Without it, the PageHeader does not know the context of the navigation stack. 

1. The Back button will only display if there are pages in the BackStack. For example, the user will never see the back button on the main page of the application, since the back stack is empty.

1. The developer may choose to let the operating system draw a back button in the chrome of the application. This can be set using the Bootstrapper.ShowShellBackButton property. When the shell-drawn back button is visible, the PageHeader's on-canvas back button will not be visible. 

- When DeviceFamily=Desktop + Mode=Mouse, the shell-drawn back button in the application's title bar. 
- When DeviceFamily=Desktop + Mode=Touch, the shell-drawn back button is in the task bar.
- When DeviceFamily=Mobile, the shell-drawn back button is in the navigation bar.

Note: When (DevideFamily=Desktop + Mode=Touch) or (DeviceFamily=Mobile) setting the PageHeader BackButtonVisibility property has no effect. The shell-drawn back button is always visible.

####Built-in behavior

Clicking the on-canvas or the shell-drawn back button is automatically handled by Template 10. It will Frame.GoBack automatically. If there is no where to go back, the on-canvas button will no longer be visible. In this state, the shell-drawn back button will be visible; it does not auto-hide. Clicking the shell-drawn back button in this state will have variable behavior.

- When DeviceFamily=Desktop + Mode=Mouse, clicking the back button will do nothing. 
- When DeviceFamily=Desktop + Mode=Touch, clicking the back button will show the start menu.
- When DeviceFamily=Mobile, clicking the back button will show the next open app.

####Overriding built-in behavior

The developer might want to handle BackRequested behavior manually. To do override the native behavior, handle the Bootstrapper.BackRequested event and (optionally) set args.Handled to true. 

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Frame="{x:Bind Frame}" 
        BackButtonVisibility="Visible"
        Text="Detail" />

</Page>
```

The on-canvas back button would look like this:

![](http://i.imgur.com/rKLWSCm.png)

####Commands

The PageHeader control extends the standard XAML CommandBar control.

PageHeader offers two categories of commands:

1. PrimaryCommands are always visible.
2. SecondaryCommands are hidden by default.
 
The most common control is **AppBarButton**. This represents a button with a label and an icon. You manage interaction by either handling the button's Click event or bind to the Command property for apps using the MVVM design pattern. 

The following sample code shows how to define a set of commands:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Text="Main Page" Frame="{x:Bind Frame}">
        <controls:PageHeader.PrimaryCommands>
            <AppBarButton Icon="Forward" Label="Next" Click="Next_Clicked" />
        </controls:PageHeader.PrimaryCommands>
        <controls:PageHeader.SecondaryCommands>
            <AppBarButton Label="Option 1" Click="Op1_Clicked" />
            <AppBarButton Label="Option 2" Click="Op2_Clicked" />
        </controls:PageHeader.SecondaryCommands>
    </controls:PageHeader>

</Page>
```

The resulting PageHeader with buttons looks like this:

![](http://i.imgur.com/NYQTfCg.png)

####Phone versus Desktop

From a UI point of view, the size of the PageHeader control can be very different when DeviceFamily=Desktop versus DeviceFamily=Mobile. As such, on mobile you should privilege the secondary commands and add as primary commands no more than four buttons; otherwise, when space is limited, primary buttons are not visible in your UI. In addition, when you are designing your UI, you must remember the space taken by the text/title of your page. In such cases it might make sense to have even fewer primary buttons. 

> The developer can manipulate primary and secondary buttons either through view-model binding or via code-behind at any time during runtime to account for size or device family.

Here's a comparison to consider:

![](http://i.imgur.com/3KUiKFs.png)

####PageHeader and the HamburgerMenu

The PageHeader control is the perfect companion for the HamburgerMenu control (also part of Template 10). 

> In this case, a developer would typically choose to set the PageHeader.HeaderBackground property to match the HamburgerMenu.HamburgerBackground property. 

####Visual States for the Hamburger Button

The PageHeader control has two built-in visual states - VisualStateNarrow and VisualStateNormal. These have been specifically created to support the HamburgerMenu control which has the identical visual states. 

1. The PageHeader's VisualStateNarrow effects the UI in only one way, it shifts the Text of the control 48 pixels to the right. This provides the on-screen real estate required to display the stand alone hamburger button. 

2. The PageHeader's VisualStateNormal effects the UI in no way, other than removing the effects applied by the PageHeader's VisualStateNarrow - effectively shifting the Text left 48 pixels. 

####Controlling the Visual States

You can control the PageHeader's visual states by defining the minimum widths that triggers them. The **VisualStateNarrowMinWidth** and **VisualStateNormalMinWidth** properties accomplish this.

You can apply these values like this:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Text="Main page" 
        Frame="{x:Bind Frame}"
        VisualStateNarrowMinWidth="0"
        VisualStateNormalMinWidth="800" />

</Page>
```

In the case above, when the width of the window is greater than 0 effective pixels but less than 800, the VisualStateNarrow visual state will be triggered. Concurrently, when the width of the window is equal to or greater than 800 effective pixels, the VisualStateNormal visual state will be triggered - shifted the PageHeader.Text 48 pixels to the right. 

> Remember: to achieve the best result, the VisualStateNarrowMinWidth property of the HamburgerMenu control should be set to the same value.

Here's how your UI will behave:

![](http://i.imgur.com/07deVoH.gif)

## Disable the Visual States

Some developers may not want the Narrow View State to be applied to the PageHeader. This will certainly be true if the developer is not implementing the hamburger button. In this case, setting the ViewStateNarrowMinWidth to the value of -1 will cause it to never qualify and never be applied.

You would handle this scenario like this:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls">

    <controls:PageHeader Text="Main page" 
        Frame="{x:Bind Frame}"
        VisualStateNarrowMinWidth="-1" />

</Page>
```
