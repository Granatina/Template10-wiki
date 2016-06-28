##Helpful Tips

* [Passing simple Parameters to a Page] (#PassingParameters1) 
* [Passing complex Parameters to a Page] (#PassingParameters2) 

###[Passing Parameters to a Page](PassingParameters1)

A simple parameter is a C# string, integer, boolean, or some other data type - sometimes called reference types or integral type. The easiest way to understand it is: it's a base type that is not a class. Passing these in Template 10 is simple. 

**Here's how to pass from app.xaml.cs or a view-model**

App.xaml.cs inheriting from `Bootstrapper` (which is standard in a Template 10 project) and every view-model inheriting from `ViewModelBase` (which is standard in a Template 10 project) has `NavigationService` as a standard property. The NavigationService's `NavigateAsync()` method is the only supported method to navigate in Template 10.  

> Ideally, all of your parameters passed to pages would be simple types.

````csharp
var value = "Hello world";
await NavigationService.NavigateAsync(typeof(Views.MainPage), value);
````

**Here's how to pass from a page**

All XAML pages have a default `Frame` property which can be used to discover the corresponding Template 10 `NavigationService` by using the `GetNavigationService()` extension method available in the `Template10.Utils` namespace. This is only necessary because the Template 10 NavigationService is the only supported method to navigate in Template 10.  

````csharp
using Template10.Utils;

var value = "Hello world";
await this.Frame.GetNavigationService().NavigateAsync(typeof(Views.MainPage), value);
````

**Here's how to receive in a view-model**

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

**Here's how to receive in a page**

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

###[Passing Parameters to a Page](PassingParameters2)

Thee are 
MainPage.XAML

````XAML
 <Sample />
````

MainPage.XAML.cs

````csharp
var sample = "one";
````


