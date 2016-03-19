# Table of Contents
1. [T10_Command](#T10_Command)
2. [T10_CommandTyped](#T10_CommandTyped)
3. [T10_Dispatch](#T10_Dispatch)
3. [T10_DispatchAwait](#T10_DispatchAwait)
3. [T10_DispatchAwaitResult](#T10_DispatchAwaitResult)

## T10_Command
Expands to create a command definition:
```csharp
DelegateCommand _executeCommand;
    public DelegateCommand ExecuteCommand
        => _executeCommand ?? (_executeCommand = new DelegateCommand(() =>
        {
           throw new NotImplementedException();
        }, () => true));
```
## T10_CommandTyped
Expands to create a strongly type command:
```csharp
DelegateCommand<object> _MyCommand;
public DelegateCommand<object> MyCommand
    => _MyCommand ?? (_MyCommand = new DelegateCommand<object>(MyCommandExecute, MyCommandCanExecute));
bool MyCommandCanExecute(object param) => true;
void MyCommandExecute(object param)
{
    throw new NotImplementedException();
}
```
## T10_Dispatch
Expands to code snippet that allows an action to be executed on the current UI thread:
```csharp
WindowWrapper.Current().Dispatcher.Dispatch(() => { /* TODO */ });
```
## T10_DispatchAwait
Expands to code snippet that allows an async action to be executed on the current UI thread and the task to be awaited:
```csharp
await WindowWrapper.Current().Dispatcher.DispatchAsync(() => { /* TODO */ });
```
## T10_DispatchAwaitResult
Expands to code snippet that allows an async action to be executed on the current UI thread, the task to be awaited and the result to be assigned:
```csharp
var MyVariable = await WindowWrapper.Current().Dispatcher
                 .DispatchAsync<string>(() => { return default(string); });
```