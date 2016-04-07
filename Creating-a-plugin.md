Plugin development for Spectrum is really easy once you know the basics. Follow this guide to get an idea on how to make a Spectrum plugin properly. At least basic knowledge of C# 6.0 and Visual Studio 2015 workflow is assumed, so you'll want to brush that up before diving into plugin development.

#### Creating the project
Start by launching Visual Studio and creating a project of type "Class Library".  
**Important:** You have to set the Target Framework to ".NET Framework 3.5". Otherwise the plugin won't get loaded by Spectrum. 
![](http://img.imgland.net/NNkwHyB.png)  
We'll name the plugin **TutorialPlugin** here, but you can choose any name you want. Click OK and wait until the project gets created.

#### Creating the plugin
The starting point for you should be this code:
```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace TutorialPlugin
{
    public class Class1
    {
    }
}
```  
If this is not the code created by Visual Studio after clicking OK, then re-read the previous section carefully. Otherwise continue.

Every plugin loaded by Spectrum has to meet three basic rules:
  1. There must exist a class named **Entry** and the name **Entry** should exist only once in the whole project.
  2. The class named **Entry** must implement the **IPlugin** interface.
  3. The file from which the plugin is loaded shall have the extension `.plugin.dll`.

To make the **IPlugin** interface accessible, you have to include a reference to **Spectrum.API.dll**. It is located in the Spectrum's default installation path. You should already know this from the [installing Spectrum guide](https://github.com/Ciastex/Spectrum/wiki/Installing-Spectrum).  
**Do not continue** until your References list includes ![](http://img02.imgland.net/NxPfIMy.png). I'm sure you'll figure this out.  

Keeping in mind the two first base rules, we change the code above to become this:
```C#
using Spectrum.API;
using Spectrum.API.Interfaces.Plugins;
using Spectrum.API.Interfaces.Systems;

namespace TutorialPlugin
{
    public class Entry : IPlugin
    {
        public string FriendlyName => "Tutorial Plugin";
        public string Author => "AnAwesomeDeveloper";
        public string Contact => "anawesomedev@example.com";
        public APILevel CompatibleAPILevel => APILevel.RadioWave;

        public void Initialize(IManager manager)
        {
            
        }

        public void Shutdown()
        {
            
        }
    }
}
```  
As you can see, it's pretty straightforward. Let's have a closer look at the plugin's property and method structure.

| Property name  | Type | Description  |
| :---: | :---: | :--- |
| ```FriendlyName```  | `System.String`  | Holds information on how Spectrum will display your plugin's name. It can be any length and can contain any characters.  |
| `Author` | `System.String` | A place where you can put your name/nickname/whatever points to you as a creator. |
| `Contact`  | `System.String`  | You can put anything that can be used to contact you. JIDs, webpages, e-mails. Anything that allows communication with you in case somebody wants to submit a bug, etc. |
| `CompatibleAPILevel` | `Spectrum.API.APILevel` | Holds information for which version of the API the plugin has been made for. |


| Method name | Return type | Passed parameters | Description |
| :---: | :---: | :--- | :--- | 
| `Initialize` | `void` | The IManager object for the current game session | This is the first method called when your plugin passes the validation and loading phase. You can set up - for example - hotkey/event callbacks, settings initialization here. This method gets called only once for each loaded plugin. |
| `Shutdown` | `void` | None | This is the method when your plugin gets a last chance of cleaning up before the game exits. Do any clean-up activities like deleting temporary resources or saving your settings here. |

Being armed with that information, let's write an obligatory "Hello, World!" plug-in, shall we? The plugin will show "Hello, world!" text to the right of the player's car when a race starts.  
To do that: 
  * Add the following to the top (where the `using` directives are):  
```C#
using Spectrum.API.Game;
using Spectrum.API.Game.Vehicle;
```
  * Then simply add the following to the `Initialize(IManager manager)` method:  
```C#
Race.Started += (sender, args) => { LocalVehicle.HUD.SetHUDText("Hello, world!"); };
```
This will make the HUD text appear when the race starts by binding the code on the right to the event on the left.

Compile it, then copy it from your project's active configuration output directory, to Spectrum's plugin directory.  
**NOTE:** You should only copy your plugin DLL, no matter what other files are there, unless you know what you're doing! Name the file to comply with the base rule #3 and run the game.  
This is what you should see when running with verbose mode (the `-console` parameter in launch options):  
![Console result](http://img.imgland.net/v4BVJ2V.png)  
And the following is what you should see when any race starts now:  
![Game result](http://img.imgland.net/vwbhbyI.png)

#### Going further
Here is the list of tasks you can do to get familiar with the API structure faster:
 1. Getting the text shown on HUD can be boring after a while. Make it so the text gets written on the car's screen rather than on the HUD.
 2. Showing the text only once for the whole race isn't fun. Make it so the text gets shown after a specific **hotkey** is pressed.
 3. Having a hard-coded hotkey is not flexible. Try to make the hotkey configurable by using the **settings API**.

Hopefully this got you started on plugin development for Spectrum. We're eager to hear what amazing things you've done with the Spectrum Extension System. Drop us a line so that we know about your development efforts.