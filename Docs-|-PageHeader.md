# The PageHeader control #
One of the new features added in the Universal Windows Platform is the ability to place the CommandBar control in multiple positions, while in Windows / Windows Phone 8.1 it was possible to place it only at the bottom of the screen.
PageHeader is a control that extends the CommandBar so that it can act as a header for your pages, to mimic the behavior of some of the new Windows 10 apps (like News or Money).

Some of the features provided by the PageHeader control are:

- Support for primary and secondary commands
- Embedded navigation management: the header can automatically contain navigation buttons to go back and forth through the application’s pages.
- Easy look & feel customization

## IMPLEMENTATION ##
Adding the control to your page is quite easy. Make sure, first, to declare in the XAML page the namespace *Template10.Controls*, like in the following sample:

```XAML
<Page x:Class="Controls.MainPage"
      xmlns:controls="using:Template10.Controls"
      mc:Ignorable="d">

  		<!-- your page content here -->

</Page>
```

Then, place the PageHeader control in your page:

```XAML
<controls:PageHeader Frame="{x:Bind Frame}">
```

The key property is **Frame**, which has to be connected to the Frame that contains the current page. Without this configuration, the PageHeader control won’t be able to properly handle the navigation.

![](http://i.imgur.com/BFG3pSB.png)

## CUSTOMIZATION ##
The PageHeader control offers some properties to customize its look & feel. The most important ones are:
- **Text** to define the text displayed in the header (typically, the title of the page)
- **HeaderForeground** to define the text color of the header
- **HeaderBackground** to define the background color of the header

Here is a sample customization:

```XAML
<controls:PageHeader Text="Main Page" Frame="{x:Bind Frame}" HeaderBackground="Orange" HeaderForeground="Red" />
```
![](http://i.imgur.com/xvwCFXf.png)

## NAVIGATION ##
The PageHeader control offers a property to define the visibility of the navigation buttons, called **BackButtonVisibility**. When it's set to **Visible**, the control will display an arrow to perform the navigation. Setting the Frame property makes sure that the arrow is visible only if there are pages in the stack. For example, if you’re in the first page of your application the arrow will always be hidden since the back stack is empty, even if the BackButtonVisibility property is set to Visible.
By default, the visibility of the navigation buttons on the desktop is connected to the **ShowShellBackButton** property of the bootstrapper. If this property is set to true (which means that, on desktop, you’re going to take advantage of the virtual back button that is embedded in the chrome of the app), the Back button in the PageHeader control will always be hidden, to avoid generating confusion for the user.

You can control the visibility of the navigation buttons using the BackButtonVisibility property, but you can't override the standard behavior of the operating system. For example, no matter if you set the BackButtonVisibility property to Visible, if you're on a mobile device the button will always be hidden.

```XAML
<controls:PageHeader Text="Detail" Frame="{x:Bind Frame}" BackButtonVisibility="Visible" />
```
![](http://i.imgur.com/rKLWSCm.png)

## COMMANDS ##
The PageHeader control offers a CommandBar control, which works in the same way of the standard one that developers can leverage to manage the application bar. Commands are, by default, displayed on the right of the screen, while the header’s text is displayed on the left.

PageHeader offers two categories of commands:

1. **PrimaryCommands**: they are always visible and they are displayed as an icon with a description.
2. **SecondaryCommands**: they are hidden by default and they are displayed when the user taps on the three dots at the end of the bar. Only the description is displayed.
 
The most common control you’re going to include as a command is **AppBarButton**, which represents a button with an icon (defined in the **Icon** property) and a label (define in the **Label** property). If you want to manage the interaction with the user, you can subscribe to the **Click** event or, if you’re using the MVVM pattern, use the **Command** property.

The following sample code shows how to define a set of commands:

```XAML
<controls:PageHeader Text="Main Page" Frame="{x:Bind Frame}">
    <controls:PageHeader.PrimaryCommands>
        <AppBarButton Icon="Forward" Label="Next" Click="OnNextClicked" />
    </controls:PageHeader.PrimaryCommands>
    <controls:PageHeader.SecondaryCommands>
        <AppBarButton Label="Option 1" />
        <AppBarButton Label="Option 2" />
    </controls:PageHeader.SecondaryCommands>
</controls:PageHeader>
```

![](http://i.imgur.com/NYQTfCg.png)

## PHONE VS DESKTOP ##
It’s very important to remember a couple of differences in using the PageHeader control when you're targeting a mobile application rather than a desktop one.

- From a UI point of view, the size of the PageHeader control can be very different when it’s used on a desktop / tablet rather than on a phone. As such, on the phone you should privilege the secondary commands and add as primary commands no more than one or two options; otherwise, the space wouldn’t be enough to display both the buttons and the text of the header.

![](http://i.imgur.com/3KUiKFs.png)

- The navigation system between the two device families has some minor differences. On the desktop, you can choose to use the virtual Back button embedded in the shell, by enabling the ShowShellBackButton property of the bootstrapper, or you can choose leverage the Back button in the PageHeader control. On the phone, instead, the back button in the PageHeader is always hidden: the backward navigation on a mobile device should always rely on the hardware back button.
