####What is a XAML behavior?
XAML behaviors are a mechanism to encapsulate code with logic and configuration parameters such that it can then be declared in XAML without requiring code-behind. Most XAML Behaviors have design-time support. They can easily be written in C# and have simple interfaces (IBehavior).

> For now, XAML behaviors require the XAML behaviors extension SDK that ships natively with Visual Studio. To add behavior support in your application, developers must manually reference this extension.

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

**XAML Action**

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

##Template 10 XAML behaviors and actions
Naturally, we built these for our own use, but they are nice, utilitarian behaviors.
###ConditionalAction 
This action is intended to be both the child of a XAML behavior and the parent of one or more XAML actions. This  action has a configurable condition. If that condition is satisfied, this action will invoke all of its child XAML actions. If the condition is not satisfied, this action will not invoke its child XAML actions.

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
in the example above, a XAML behavior monitoring the click event of a button will fire when the user clicks the button. The developer does not want any XAML actions invoked if the user is not logged in; he can put this XAML action in between the XAML behavior and the actions. The "logged in" condition can be configured to this XAML action to determine if the child XAML actions are invoked or the behavior is ignored.

###NavButtonBehavior
###TextBoxEnterKeyBehavior
###TimeoutAction

