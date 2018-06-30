This wiki page aims to explain the basic environment setup of every Spectrum plugin. This page was last updated for the Spectrum Gamma release.

### Creating the Project
Using Visual Studio 2015 or later, create a new class library project **for .NET Framework, not .NET Standard**, targeting the **.NET Framework 3.5**.   
<p align="center"><img src="https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/PluginCreation/TheBasics/ProjectSetup.png"></img></p>

### Environment setup
Rename the default `Class1.cs` file to a name of your liking. In this example we are using `Entry.cs`:
```CSharp
using Spectrum.API;
using Spectrum.API.Interfaces.Plugins;
using Spectrum.API.Interfaces.Systems;

namespace ExamplePlugin
{
    public class Entry : IPlugin
    {
        public void Initialize(IManager manager, string ipcIdentifier)
        {

        }
    }
}
```
The plugin has only one initialization method `Initialize(IManager manager)` and is executed only once, when your plugin is loaded. You can set up settings, register events, hook to engine, and so on in here.   

### Adding some code
In the `using` section of your code, add the following line:
```CSharp
using System;
```  
In the `Initialize(IManager manager)` method, add the following line:
```CSharp
Console.WriteLine("Hello, world.");
```   
This will result in `Hello, world.` being displayed in the console window Spectrum creates.

### Setting up Distance
#### Enabling development console
After installation, Spectrum adds a nice command-line argument to Distance, that allows you to have a development console enabled. It helps track down bugs using exception print-outs, and makes your life easier as a developer in general.

To enable console window to spawn at Distance startup, add `-console` argument to the startup arguments, does not matter if it's a shortcut or Steam launch argument.

#### Packaging the plugin
Plugin packaging is described on [this wiki page](Packaging-a-plugin).

### The results
Your console should look like the following image (this image was taken using an outdated Spectrum version):   
<p align="center"><img src="https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/PluginCreation/TheBasics/ConsoleResult.png"></img></p>