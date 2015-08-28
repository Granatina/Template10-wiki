## HANDLING FORWARD & BACK

When a user navigates from one page to another in a Windows app, the target view and associated view-model is instantiated every time. This is even true when the user goes to a view, goes to a second view, and returns to the first view. 

This is good because the resulting view is always fresh. But, this is bad because sometimes the data displayed by a view hasn’t changed since the user was viewing it last. It would be great if a developer could instruct a view to cache itself in these cases.

### CACHE MODE

For this reason, the XAML page class has NavigationCacheMode. By default it is disabled. This means every time the user navigates to the page, the page is instantiated. But when it’s set to enabled (which can only be set inside the page constructor) the page instantiates only on the first visit.

> MSDN Docs: https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page.navigationcachemode.aspx

Caching is difficult on machines that have limited memory. Because every cached page equals some amount of memory consumption, it increases the opportunity for your app to be automatically terminated by the operating system. But, an app’s memory threshold is high and such a case is seldom. 

> To help developers more, the XAML frame has a CacheSize property. This is the limit to the number of pages that can be cached. When cache is enabled, this is obeyed. However, the third possible value to NavagationCacheMode is required, which behaves just like enabled except it ignores CacheSize.

### CONSIDER THIS

1. If some view causes the data displayed in another view to change, the data should keep itself up-to-date if you are sharing models in memory. If, however, you are using copies of models in memory, the first view-model will need to signal the second view-model through some type of messaging solution. 
> Messaging is a pattern for view-models to communicate without requiring a reference to one another. It’s powerful, common, and gets the job done. Template 10 does not ship with a messaging solution, but we like the PubSubEvents created by Microsoft in NuGet, and the Messenger class in MVVM Light. Again, there are many out there and they are all effective.

2. If some view causes the data displayed in another view to be deleted, the developer cannot allow the user to navigate forward (or back) and view the data that is dead. In this case, the developer would need to clear the BackStack  or ForwardStack of the container XAML frame. In Template 10, this is done with NavigationService.Frame.BackStack.Clear();. Some of the visual controls on your view may need to be manually updated to reflect the change in navigation.

3. You should assume your view is new every time. What I mean is, you can’t rely on caching. Either the system or some freak of nature is going to cause your view or view-model to be re-instantiated on navigation. No matter how you use cache, you must write your code defensively so that if caching should not work or be overloaded in some way, your app continues to deliver an excellent user experience.

## IMPLEMENTATION

Let’s take a minute and talk about how you would introduce the basic caching functionality of caching in a Template 10 Windows app. Once you have enabled caching in the constructor of your page(s) it’s time to update your view-models to handle this special navigation.

### TURN IT ON

Remember you only need to mess with this in view-models associated to views that actually enable the NavigationCache. Here's how you enable it.

```c#
public DetailPage()
{
    InitializeComponent();
    NavigationCacheMode = NavigationCacheMode.Enabled;
}
```

### NAVTO

Perhaps the biggest benefit of Template 10 is INavigable. If your view-model implements that interface, such as inheriting from Template10.Mvvm.ViewModelBase, there is an OnNavigatedTo method you can override. This method is called whenever the user navigates to a view that sets the view-model to its DataContext property. It’s awesome.

One of the parameters of OnNavigatedTo is NavigationMode. When the user is navigating forward or back along the navigation stack this argument has the corresponding forward or back ENUM value. Handling this value allows you to property handle data loading – in most cases you will not re-load your data if the user is only navigating forward or back.

```C#
// (optional) When NavigationCacheMode = NavigationCacheMode.Enabled;
if (mode == NavigationMode.Forward | mode == NavigationMode.Back)
{
    // rely on cache
}
else
{
    // normal execution
}
```

Make sense?
