Spectrum is not just about plugins and scripts. This modding toolkit lets you create any automated game patch you want. Make sure you [have the latest source](https://github.com/Ciastex/Spectrum/wiki/Compiling-Spectrum#downloading-the-source-code) before continuing. This tutorial assumes you have a basic knowledge about .NET's IL code.

#### Preface
Prism uses Mono.Cecil to perform offline patches on Distance's assembly. Every patch that is run needs to inherit the `BasePatch` class or `IPatch` interface directly. Inheriting `BasePatch` is recommended to avoid code redundancy. The patch class instances are then added to the Prism's patch list, then run one after another. I like practical things for learning the new stuff, so we're going to write a patch that enables development mode permanently (until the next game's update).

#### Writing the patch
Launch Visual Studio, open the project, let it restore any packages. When it's done, you are ready to begin.

Look at the list below. All patches you write should be created in the highlighted folder; they should also have the `Patch` suffix.  
![The list](http://img02.imgland.net/nNUDnOy.png)  
Having the folder highlighted, press `Alt + Shift + C` hotkey. This will open the class creation menu. Name the class `DevModeEnablePatch.cs`. Look at the image below for reference. Click 'Add' to create the class.  
![The class creator](http://img04.imgland.net/3Ngy11T.png)  
Now modify the generated code, so it looks like this:  
```C#
using System;
using Mono.Cecil;
using Spectrum.Prism.Runtime;
using Spectrum.Prism.Runtime.EventArgs;

namespace Spectrum.Prism.Patches
{
    public class DevModeEnablePatch : BasePatch
    {
        public override string Name => "DevModeEnable";
        public override bool NeedsSource => false;

        public override void Run(ModuleDefinition moduleDefinition)
        {
            
        }

        public override void Run(ModuleDefinition sourceModule, ModuleDefinition targetModule)
        {
            OnPatchFailed(this, new PatchFailedEventArgs(Name, new Exception("This patch does not require any source modules.")));
        }
    }
}
```  
Let's explain some things first. We are not going to use any external DLLs for our patch, so we override the base `NeedsSource` field and set its value to `false`. Then we are defining overrides for the two `Run(...)` methods. Again, we are not going to use any external DLLs for our patch, so we make it fail when we provide a source module definition to it. Therefore, the method of interest is the first one.  
Now... What code are we going to patch? Where is it? This is when our trusty decompiler comes in. This tutorial will use [dnSpy](https://github.com/0xd4d/dnSpy) for decompilation.  
The `IsDevBuild_` property is defined in `GameManager` singleton class. It's a read-only property, so it hides a compiler-generated method for **getting** the value:  
![The decompiled code](http://img03.imgland.net/zFJ1IzW.png)  
No obvious things, huh? So let's switch to the **IL language**. Aha! There is our hidden method:  
![The hidden method](http://img03.imgland.net/d36cqt.png)  
Now that we know the method's name, let's write the patch. Modify your Run method to become this:  
```C#
try
{
    var targetType = moduleDefinition.GetType("GameManager");
    var methodDefinition = targetType.Methods.Single(m => m.Name == "get_IsDevBuild_");

    methodDefinition.Body.Instructions.Clear();
    var ilProcessor = methodDefinition.Body.GetILProcessor();

    var ldcInstruction = ilProcessor.Create(OpCodes.Ldc_I4_1);
    var retInstruction = ilProcessor.Create(OpCodes.Ret);

    ilProcessor.Append(ldcInstruction);
    ilProcessor.Append(retInstruction);

    OnPatchSucceeded(this, new PatchSucceededEventArgs(Name));
}
catch (Exception ex)
{
    OnPatchFailed(this, new PatchFailedEventArgs(Name, ex));
}
```  
There's a lot going on here now, let's analyze it step-by-step.  
First, we need to know the type our method is stored in. We know it's **GameManager** so we pass it on to Mono.Cecil. Second - we need to get the method definition for our **getter**. We know its name, so we find it inside GameManager's methods. We then proceed to create the **ILProcessor** for this method and clear all the existing instructions inside the requested method. Then there's the core of our solution. We create two IL instructions and append them to the method body. After this, we notify the patcher that we're done (either failed or succeeded). This is it, that's the whole patch.

#### Making it work
Alright, so we've created the patch. Now it's time to introduce it to Prism. Open up the `Program.cs` file and find the `PreparePatches()` method. Add the following line to the end of method: `_patcher.Add(new DevModeEnablePatch());` and you're ready to build Prism. Now that you've built Prism, copy the target EXE and dependencies into `Distance_Data\Managed` folder. Run the patch with the following command:  
`Spectrum.Prism.exe -t Assembly-CSharp.dll -p DevModeEnable`. 

You're done.