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
<controls:PageHeader Text="Main Page" 
                     Frame="{x:Bind Frame}" 
                     HeaderBackground="Orange" 
                     HeaderForeground="Red" />
```
![](http://i.imgur.com/xvwCFXf.png)

## NAVIGATION ##
The PageHeader control offers a property to define the visibility of the navigation buttons, called **BackButtonVisibility**. When it's set to **Visible**, the control will display an arrow to perform the navigation. However, the visibility of the button isn't controlloed just by this property, but also from other factors that are controlled by the operating system.

Specifically:

1. The PageHeader control is connected to the Frame of the application, so it knows the state of the navigation stack. As such, the Back button will be displayed only if there are pages in the stack. For example, the user will never see the back button on the main page of the application, since the back stack is empty.
2. Even if the navigation system is unique across the different device families where Windows 10 runs, there are some minor differences. On desktop, you can choose to display a virtual back button in the chrome of the app or to include it into the layout of your page. On mobile, instead, backward navigation is always controlled by the hardware back button and you shouldn't have any visual element to handle it. Consequently, even if you set the BackButtonVisibility property to Visible, on a mobile device the button will always be hidden.
3. As already mentioned, on desktop a Windows 10 application can take advantage of a virtual back button embedded in the chrome of the app to handle the backward navigation. This feature, in Template10, is controlled by the **ShowShellBackButton** property of the BootStrapper class (which is true by default). If the shell button is enabled, the back button of the PageHeader control will always be hidden, even if the BackButtonVisibility property is set to Visible, to avoid generating confusion for the user. 


```XAML
<controls:PageHeader Text="Detail" Frame="{x:Bind Frame}" 
                     BackButtonVisibility="Visible" />
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

From a UI point of view, the size of the PageHeader control can be very different when it’s used on a desktop / tablet rather than on a phone. As such, on the phone you should privilege the secondary commands and add as primary commands no more than one or two options; otherwise, the space wouldn’t be enough to display both the buttons and the text of the header.

![](http://i.imgur.com/3KUiKFs.png)