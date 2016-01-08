##Behaviors
XAML behaviors are a mechanism to encapsulate code with logic and configuration parameters such that it can then be declared in XAML without requiring code-behind. Most XAML Behaviors have design-time support. They can easily be written in C# and have a simple interface `IBehavior` and are typically contain XAML Actions `IAction`.

###Table of Contents

1. What is a XAML Behavior?
1. `EllipseBehavior`
1. `NavButtonBehavior`
1. `TextBoxEnterKeyBehavior`
1. What is a XAML Action?
1. `CloseFlyoutAction`
1. `ConditionalAction`
1. `FocusAction`
1. `OpenFlyoutAction`
1. `TimeoutAction`

**XAML Behavior**

A XAML behavior (implementing [IBehavior](https://msdn.microsoft.com/en-us/library/microsoft.xaml.interactivity.ibehavior(v=vs.120).aspx)) is typically, though not necessarily, responsible to listen for an preconfigured or manually configured event. An example of a XAML behavior is the PropertyChangedBehavior (which is native to the framework) which waits for the value of a property to change (using INotifypropertyChanged). XAML behaviors, once triggered, invoke one or more XAML actions - also part of the behavior framework. 

````csharp
[ContentProperty(Name = nameof(Actions))]
public class MyBehavior : DependencyObject, IBehavior
{
    public DependencyObject AssociatedObject { get; set; }

    public void Attach(DependencyObject associatedObject)
    {
        AssociatedObject = associatedObject;
    }

    public void Detach()
    {
        // TODO
    }

    public ActionCollection Actions
    {
        get
        {
            var actions = (ActionCollection)base.GetValue(ActionsProperty);
            if (actions == null)
                base.SetValue(ActionsProperty, actions = new ActionCollection());
            return actions;
        }
    }
    public static readonly DependencyProperty ActionsProperty = DependencyProperty.Register(nameof(Actions),
        typeof(ActionCollection), typeof(MyBehavior), new PropertyMetadata(null));
}
````

###EllipseBehavior
The intent of the `EllipseBehavior` is to fix a flaw in the native XAML `CommandBar` control. The ellipse to show either labels and secondary commands is always visible, even when not necessary (even in Windows 10.1). 
> This is valuable when your design requires you to remove the ellipse.

`syntax`

###NavButtonBehavior
The intent of the `NavButtonBehavior` is to add behavior to any native XAML `Button` or `AppBarButton`. Once set to either [ Forward | Back ] then clicking the `Button` will navigate the referenced `Frame` accordingly.
> This is valuable when wanting to create navigation buttons.

````XAML
<AppBarButton Icon="Forward" Label="Forward">
    <Interactivity:Interaction.Behaviors>
        <Behaviors:NavButtonBehavior Direction="Forward" Frame="{x:Bind Frame, Mode=OneWay}" />
    </Interactivity:Interaction.Behaviors>
</AppBarButton>
````

###TextBoxEnterKeyBehavior
The intent of the `TextBoxEnterKeyBehavior` is to add behavior to any native XAML `TextBox` to invoke any child action when the user hits the `Enter` key. 
> This is valuable in a form with a Submit button.

````XAML
<TextBox>
    <Interactivity:Interaction.Behaviors>
        <Behaviors:TextBoxEnterKeyBehavior>
            <Core:CallMethodAction MethodName="GotoDetailsPage" TargetObject="{Binding}" />
        </Behaviors:TextBoxEnterKeyBehavior>
    </Interactivity:Interaction.Behaviors>
</TextBox>
````

##XAML Action

A XAML action (implementing [IAction](https://msdn.microsoft.com/en-us/library/microsoft.xaml.interactivity.iaction(v=vs.120).aspx)) does not typically listen to events, but instead takes some action - which is limited only by the developer's imagination. An example of a XAML action is the ChangePropertyAction (which is native to the framework) which changes the value of some property to a new value, typically leveraging data-binding. 

````csharp
public sealed class MyAction : DependencyObject, IAction
{
    public object Execute(object sender, object parameter)
    {
        // TODO
    }
}
````

###CloseFlyoutAction
The intent of the `CloseFlyoutAction` is to close the first `FlyOut` parent in the Visual Tree. When invoked by a behavior, it will hunt up the XAML tree for the first `FlyOut` and set its `IsOpen` property to false. 
> This is valuable for small forms inside a `FlyOut` and is commonly used on Submit buttons in those forms.

`syntax`

###ConditionalAction
The intent of the `ConditionalAction` is to prevent subsequent actions unless a condition is met. When invoked by a behavior, it will evaluate the condition and invoke child actions if the condition is met.
> This is valuable if a child action cannot be executed until some condition is satisfied.

````XAML
<Button>
    <Core:EventTriggerBehavior EventName="Clicked"> 
        <b:ConditionalAction xmlns:b="using:Template10.Behaviors" 
            LeftValue="{Binding IsLoggedIn}" Operator="IsTrue"> 
            <Core:GoToStateAction StateName="LoggedInViewState" /> 
        </b:ConditionalAction> 
    </Core:EventTriggerBehavior> 
</Button>
````

###FocusAction
The intent of the `FocusAction` is to call `Control.Focus()` on some referenced control. When invoked by a behavior, it will call Focus() and ignore if it succeed or not.
> This is valuable in situations like Page.Load, focusing on the first element. It can also change the focus when used in conjunction with `TextBoxEnterBehavior`.

`syntax`

###OpenFlyoutAction
The intent of the `OpenFlyoutAction` is to open the FlyoutBase on the specified XAML element. When invoked by a behavior, it will look for the `FlyOut` and call `Show()`.
> This is valuable because it can be coupled with actions like `ConditionalAction` that can prevent the `FlyOut` until a condition. Or on controls that otherwise don't support a `FlyOut`.

````XAML
         <AppBarButton Icon="Find" Label="Search">
             <FlyoutBase.AttachedFlyout>
                  <Flyout>
                      <StackPanel>
                          <TextBlock Text="Awesome Flyout!" />
                      </StackPanel>
                  </Flyout>
              </FlyoutBase.AttachedFlyout>
              <interactivity:Interaction.Behaviors>
                  <core:EventTriggerBehavior EventName="Tapped">
                      <behaviors:OpenFlyoutAction />
                  </core:EventTriggerBehavior>
              </interactivity:Interaction.Behaviors>
          </AppBarButton>
````

###TimeoutAction
The intent of the `TimeoutAction` is to invoke child actions only after a specified number of seconds passes. When invoked by a behavior, a timer starts and child Actions are called once the time has passed. 
> This is valuable when you want to delay a response or invoke a secondary action after some time.
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


