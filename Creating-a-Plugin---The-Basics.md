This wiki page aims to explain the basic environment setup of every Spectrum plugin. This page was last updated for the Spectrum Ultraviolet release.

## Creating the Project
Using Visual Studio 2015 or later, create a new class library project **for .NET Framework, not .NET Standard**, targeting the **.NET Framework 3.5**.   
<p align="center"><img src="https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/PluginCreation/TheBasics/ProjectSetup.png"></img></p>

## Environment setup
Rename the default Class1.cs file to Entry.cs. Replace the default file code with the following template:
```CSharp
using Spectrum.API;
using Spectrum.API.Interfaces.Plugins;
using Spectrum.API.Interfaces.Systems;

namespace ExamplePlugin
{
    public class Entry : IPlugin
    {
        public string FriendlyName => "Example Plugin";
        public string Author => "someone";
        public string Contact => "contact@example.com";
        public APILevel CompatibleAPILevel => APILevel.UltraViolet;

        public void Initialize(IManager manager)
        {

        }

        public void Shutdown()
        {

        }
    }
}
```

### Default code breakdown
#### Plugin properties
`FriendlyName` is a field that Spectrum uses to identify your plugin in the registry, basically your plugin's name.  
`Author` and `Contact` fields are self-explanatory.   
`CompatibleAPILevel` is a field that allows you to provide a targeted Spectrum API level. Using the highest available is recommended.   

#### Plugin methods
`Initialize(IManager manager)` is the equivalent of `main()` in a normal program. Your plugin can set up settings, register events, hook the engine and so on here.   
`Shutdown()` gets called when the game is closed **normally** (e.g. user quits with X on window).

## Adding some code
In the `using` section of your code, add the following line:
```CSharp
using System;
```  
In the `Initialize(IManager manager)` method, add the following line:
```CSharp
Console.WriteLine("Hello, world.");
```   
This will result in `Hello, world.` being displayed in the console window Spectrum creates.

## Setting up Distance
### Enabling development console
After installation, Spectrum adds a nice command-line argument to Distance, that allows you to have a development console enabled. It helps track down bugs using exception print-outs, and makes your life easier as a developer in general.

To enable console window to spawn at Distance startup, add `-console` argument to the startup arguments, does not matter if it's a shortcut or Steam launch argument.

### Copying the plugin
Copy the plugin DLL from `bin\Debug` folder of your plugin project to `%DISTANCE_PATH%\Distance_Data\Spectrum\Plugins`, **%DISTANCE_PATH%** is where Distance is installed on your computer. Make sure the plugin file in the target location follows the name scheme `YourPluginName.plugin.dll`. Otherwise the plugin won't be loaded.

## The results
Your console should look like the following image:   
<p align="center"><img src="https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/PluginCreation/TheBasics/ConsoleResult.png"></img></p>

You are now ready for the advanced parts of this plugin creation guide.