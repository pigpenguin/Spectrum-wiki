There are times you need to debug something with data being saved after the game quits, as development console closes as soon as the game shuts down. Spectrum provides you with a facility to do just that. To keep things simple, this guide will use default plugin set-up described in [this part](https://github.com/Ciastex/Spectrum/wiki/Creating-a-Plugin:-The-Basics).

## Introduction
Spectrum saves its log files in the `%DISTANCE_PATH%\Distance_Data\Spectrum\Logs` folder. Plugins can use the `Logger(string fileName)` object to log their diagnostic data in there. The `Logger` class is located inside the `Spectrum.API.Logging` namespace.

The `Logger` constructor prototype is:
```CSharp
public Logger(string fileName)
```
### Standard behavior
Upon correct object creation, a file with the name you provided is created by Spectrum. By default, a `Logger` instance has output colorization enabled, and console write-out disabled.

## Code structure
### Creating a log file
To create a log file, simply create a class-wide variable of `Logger` type. Put it below the plugin property block and above `Initialize(IManager manager)` method:
```CSharp
private Logger _logger;
```
This will allow you to initialize and access your logger.

In the `Initialize(IManager manager)` method, add the following line to the top of the method:
```CSharp
_logger = new Logger("ExamplePlugin.log");
```
### Enabling console output
To enable console output for your logger, you need to use `WriteToConsole` property of the object:
```CSharp
_logger.WriteToConsole = true;
```
### Customizing output colors
Spectrum allows you to customize output colors for your logger with a few public properties:  
`ErrorColor` is the color of error messages. Defaults to `ConsoleColor.Red`.   
`WarningColor` is the color of warning messages. Defaults to `ConsoleColor.Yellow`.   
`InfoColor` is the color of information messages. Defaults to `ConsoleColor.White`.   
`ExceptionColor` is the color of exception messages. Defaults to `ConsoleColor.Magenta`.

### Logging data
Spectrum provides you with a few methods for logging the data depending on its severity:   
`Error(string message)` writes a message using standard error appearance.   
`Warning(string message)` writes a message using standard warning appearance.   
`Info(string message)` writes a message using standard information appearance.   
`Exception(Exception e)` pretty-prints an exception you provide it.
`ExceptionSilent(Exception e)` does not care about `WriteToConsole` property. Useful for hiding exceptions you don't care about that much to see them in real time.

## Conclusion
After finishing this guide you should have a fair understanding of Spectrum's diagnostic logging capabilities.
### Final plugin code
```CSharp
using Spectrum.API;
using Spectrum.API.Interfaces.Plugins;
using Spectrum.API.Interfaces.Systems;
using Spectrum.API.Logging;
using System;

namespace ExamplePlugin
{
    public class Entry : IPlugin
    {
        public string FriendlyName => "Example Plugin";
        public string Author => "someone";
        public string Contact => "contact@example.com";
        public APILevel CompatibleAPILevel => APILevel.UltraViolet;

        private Logger _logger;

        public void Initialize(IManager manager)
        {
            _logger = new Logger("ExamplePlugin.log");
            _logger.WriteToConsole = true;

            _logger.Error("A catastrophic error has occured.");
            _logger.Warning("A catastrophic error could occur but it did not.");
            _logger.Info("It works.");
            _logger.Exception(new Exception("A message of exception."));
        }

        public void Shutdown()
        {

        }
    }
}
```
```