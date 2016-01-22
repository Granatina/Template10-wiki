Congratulations, you are about to upgrade to the best build of Template 10. 1.1.0+ includes legacy 10240 support and 10586 support. There are a few steps you need to follow in the upgrade. 

1. Update Windows 10 to 1151, UWP 10586 (your project can still target 10240).
1. Install the Windows 10586 SDK to Visual Studio 2015 {any edition} Update 1. 
1. Use NuGet to upgrade Template 10 v1.0.* to v1.1.* in your project(s).
1. In Project/Resources delete your "Behaviors SDK (XAML)" reference.
1. Clean and rebuild.

Congratulations, you have upgraded to the best build of Template 10! 

