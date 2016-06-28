* [Passing simple Parameters to a Page] (#PassingParameters1) 
* [Passing complex Parameters to a Page] (#PassingParameters2) 

###[Passing Simple Parameters to a Page](PassingParameters1)

A simple parameter is a C# string, integer, boolean, or some other data type - sometimes called simple types, reference types, or integral type. The easiest way to understand it is: it's a base type that is not a class. Passing these in Template 10 is simple. 

####Using app.xaml.cs or a view-model

**Send**

App.xaml.cs inheriting from `Bootstrapper` (which is standard in a Template 10 project) and every view-model inheriting from `ViewModelBase` (which is standard in a Template 10 project) has `NavigationService` as a standard property. The NavigationService's `NavigateAsync()` method is the only supported method to navigate in Template 10.  

> Ideally, all of your parameters passed to pages would be simple types.

````csharp
var value = "Hello world";
await NavigationService.NavigateAsync(typeof(Views.MainPage), value);
````

**Receive**

The only time the passed parameter is surfaced in a Template 10 view-model is the `parameter` argument of the `OnNavigatedToAsync` method. In this case, the parameter's type is object and must be cast by the developer to the corresponding, expected type. Remember to code defensively and verify your cast result before using the object.  

````csharp
public override async Task OnNavigatedToAsync(object parameter, 
    NavigationMode mode, IDictionary<string, object> suspensionState)
{
    var value = parameter as string;
    // TODO: use the parameter somehow
    await Task.CompletedTask;
}
````

####Using a standard XAML Page

**Send**

All XAML pages have a default `Frame` property which can be used to discover the corresponding Template 10 `NavigationService` by using the `GetNavigationService()` extension method available in the `Template10.Utils` namespace. This is only necessary because the Template 10 NavigationService is the only supported method to navigate in Template 10.  

````csharp
using Template10.Utils;

var value = "Hello world";
var service = this.Frame.GetNavigationService();
await service.NavigateAsync(typeof(Views.MainPage), value);
````

**Receive**

Like the `OnNavigatedTo` in the view-model, the parameter argument receives the incoming parameter value. In Template 10, however, this value cannot be cast from object to the expected type. This is because the incoming value is always a string. Why? Because the `NavigationService` always serializes the parameter before Navigation. To use the parameter, you must both deserialize it and cast it. This can be done using Template 10's `SerializationService`. 

````csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var param = e.Parameter?.ToString();
    var service = Template10.Services.SerializationService.SerializationService.Json;
    var value = service.Deserialize<int>(param);
    // TODO: use parameter
}
````

###[Passing Complex Parameters to a Page](PassingParameters2)

> If your type serializes easily, and you do not need a specific instance, you don't need to obey this section. In this case, you can use the `Simple Type` approach indicated above.

####Using app.xaml.cs or a view-model

**Send**

Because the Template 10 `NavigationService` serializes all parameters, not all types can be successfully passed to a page. The solution to this is simple, save your parameter value into `SessionState` and retrieve if when your page loads. `SessionState` is a Template 10-specific, in-memory dictionary accessible everywhere. You can use it all you want, for anything you want, but it will not persist if your application is terminated. For this reason, write your code defensively.

The strategy here is to save the value into `SessionState` pass only the key

````csharp
var value = new MyType();
SessionState.Add("MyKey", value);
await NavigationService.NavigateAsync(typeof(Views.MainPage), "MyKey");
````

**Receive**

`SessionState` is a standard property in Template 10 view-models that inherit ViewModelBase. This is the same, identical SessionState available in every view-model and app.xaml.cs. 

**Here's how to receive in a view-model**

````csharp
public override async Task OnNavigatedToAsync(object parameter, NavigationMode mode, 
    IDictionary<string, object> suspensionState)
{
    var key = parameter as string;
    if (SessionState.ContainsKey(key))
    {
        var value = SessionState[key] as MyType;
        // TODO: use the parameter
    }
    else
    {
        // TODO: handle missing parameter
    }
    await Task.CompletedTask;
}
````

####Using a standard XAML Page

Since `SessionState` is a Template 10 artifact and not part of XAML, the standard `Page` does not have the `SessionState` property. Since there can be only one instance of `BootStrapper`, fetching it using the `Current` property allows you to then retrieve its `SessionState` which you can then use to persist or retrieve values.

````csharp
var state = Template10.Common.BootStrapper.Current.SessionState;

````

**Send**

All XAML pages have a default `Frame` property which can be used to discover the corresponding Template 10 `NavigationService` by using the `GetNavigationService()` extension method available in the `Template10.Utils` namespace. This is only necessary because the Template 10 NavigationService is the only supported method to navigate in Template 10.  

````csharp
using Template10.Utils;

var value = new MyType();
var state = Template10.Common.BootStrapper.Current.SessionState;
state.Add("MyKey", value);
var service = this.Frame.GetNavigationService();
await service.NavigateAsync(typeof(Views.MainPage), "MyKey");
````

**Receive**

Like the `OnNavigatedTo` in the view-model, the parameter argument receives the incoming parameter value. In Template 10, however, this value cannot be cast from object to the expected type. This is because the incoming value is always a string. Why? Because the `NavigationService` always serializes the parameter before Navigation. To use the parameter, you must both deserialize it and cast it. This can be done using Template 10's `SerializationService`. 

````csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    var param = e.Parameter?.ToString();
    var service = Template10.Services.SerializationService.SerializationService.Json;
    var key = service.Deserialize<string>(param);
    var state = Template10.Common.BootStrapper.Current.SessionState;
    if (state.ContainsKey(key))
    {
        var value = state[key] as MyType;
        // TODO: use parameter
    }
    else
    {
        // TODO: handle missing parameter
    }
}
````

// end
