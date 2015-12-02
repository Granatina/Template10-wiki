#Welcome! 

![T10](https://raw.githubusercontent.com/Windows-XAML/Template10/master/Assets/T10%2056x56.png) 

This is the home page for the Template 10 Minimal template. This template, alongside the Blank template, make up the Template 10 project template collection. They provide a production-ready example of the Template10Library Nuget API. 

##First, are you having trouble building your new Template 10 project? We can fix that.

Template 10 templates (both Blank and Minimal) use NuGet 3. Visual Studio tooling for NuGet 3 is quite new; you must restore your packages from NuGet before they will build. Here are the one-time steps to do so. 

1. Press Ctrl+Q, type `pac man` 
1. Ensure `Allow NuGet to download missing packages` is checked.
1. Ensure `Automatically check for missing packages during build in Visual Studio` is checked.  
1. Right-click your Solution, and select `Clean`.
1. Right-click your Solution, and select `Rebuild`.
1. Select your project, and click the `Refresh` button at the top of Solution Explorer.

Congratulations, you can now use your project without "missing assemblies" errors. If you want, return the Package Manager Settings back to their original values. This will make your builds significantly faster.

![](https://raw.githubusercontent.com/Windows-XAML/Template10/master/Assets/GetStarted.gif)
