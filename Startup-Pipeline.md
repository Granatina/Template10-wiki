This page shows the order of operation for standard T10 launch.

##Launch
1. BootStrapper.Constructor 
1. BootStrapper.OnWindowCreated 
1. BootStrapper.InternalLaunchAsync Previous:ClosedByUser
1. BootStrapper.InitializeFrameAsync IActivatedEventArgs:Windows.ApplicationModel.Activation.LaunchActivatedEventArgs
1. BootStrapper.NavigationServiceFactory BackButton:Attach ExistingContent:Include
1. BootStrapper.NavigationServiceFactory BackButton:Attach ExistingContent:Include Frame:Windows.UI.Xaml.Controls.Frame
1. BootStrapper.CreateNavigationService Frame:Windows.UI.Xaml.Controls.Frame
1. BootStrapper.OnStartAsync Calling
1. BootStrapper.UpdateShellBackButton 
1. BootStrapper.SubscribeBackButton 

##Suspend
1. BootStrapper.Suspending 
1. BootStrapper.Nav.SuspendingAsync Nav:Template10.Services.NavigationService.NavigationService
1. BootStrapper.OnSuspendingAsync Calling
1. BootStrapper.OnSuspendingAsync Virtual

##Resume (after suspend)
1. BootStrapper.Resuming 
1. BootStrapper.OnResuming Virtual, PreviousExecutionState:Suspended

##Suspend + Terminate
1. BootStrapper.Suspending 
1. BootStrapper.Nav.SuspendingAsync Nav:Template10.Services.NavigationService.NavigationService
1. BootStrapper.OnSuspendingAsync Calling
1. BootStrapper.OnSuspendingAsync Virtual

##Resume (after terminate)
1. BootStrapper.Constructor 
1. BootStrapper.OnWindowCreated 
1. BootStrapper.InternalLaunchAsync Previous:Terminated
1. BootStrapper.InitializeFrameAsync IActivatedEventArgs:Windows.ApplicationModel.Activation.LaunchActivatedEventArgs
1. BootStrapper.NavigationServiceFactory BackButton:Attach ExistingContent:Include
1. BootStrapper.NavigationServiceFactory BackButton:Attach ExistingContent:Include Frame:Windows.UI.Xaml.Controls.Frame
1. BootStrapper.CreateNavigationService Frame:Windows.UI.Xaml.Controls.Frame
1. BootStrapper.OnResuming Virtual, PreviousExecutionState:Terminated
1. BootStrapper.DetermineStartCause IActivatedEventArgs:Windows.ApplicationModel.Activation.LaunchActivatedEventArgs
1. BootStrapper.Nav.Restored Restored:True
1. BootStrapper.SubscribeBackButton