## How to use Spectrum
#### Requirements
In order to utilize Spectrum's capabilities, you have to modify the main Distance DLL. To do that you need:
* [.NET Reflector](https://www.red-gate.com/products/dotnet-development/reflector/) or [ILSpy](http://ilspy.net/) (recommended)
* [Reflexil](http://reflexil.net/) plug-in for the chosen decompiler

Spectrum expects the following minimum file tree available at runtime:
* Distance_Data/
* -- Spectrum/
* ---- Configuration/
* ------ Spectrum.Manager.xml
* -- Plugins/
* -- Scripts/
* -- Spectrum.Manager.dll
* -- Spectrum.API.dll
* -- KopiLua.dll
* -- NLua.dll

#### Mechanism of action
Modified ```Assembly-CSharp.dll``` has a new reference to ```Spectrum.Bootstrap.dll```. The bootstrap DLL has a method which searches for ```Distance_Data/Spectrum/Spectrum.Manager.dll``` and passes control to it. The bootstrap DLL is also responsible for update calls to the manager DLL which in turn calls Update() on all loaded plug-ins and scripts.

#### Installing Spectrum
Install ILSpy and the Reflexil plugin. Open ILSpy and load ```Assembly-CSharp.dll``` located in ```Distance_Data/Managed/``` folder and the ```Spectrum.Bootstrap.dll``` you just copied. The assembly list should look like this:  

![Assembly List](https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/img_AssemblyList.png)  
Right-click on the ```Assembly-CSharp``` tree and select ```Inject assembly reference```. In the pop-up window write ```Spectrum.Bootstrap```. The reference list should now look like this:  
![AfterModification](https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/img_ReferenceList.png)  
Open the global tree (its name is ```-```) and search for GameManager class. Open it, then search for the Awake() method. Click the cog button on toolbar.
  
![AwakeMethod](https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/img_AwakeMethod.png)  
A bytecode window will show up. Scroll to the end of the list. Select the instruction just before the last ```ret``` instruction. Right-click and select ```Create new...```. 

A new window will appear. Write ```call``` into the ```OpCode``` box, and select ```-> Method reference``` from the ```Operand Type``` list. Click the ```Operand``` box.  
![OperandBox](https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/img_OperandWindow.png)  
Select ```StartManager``` from the list. You should end up with this result:
![NewInstructionFinished](https://github.com/Ciastex/Spectrum/blob/master/Spectrum.Branding/Wiki/img_NewInstruction.png)  
Click the ```Insert after selection``` button.

The same procedure goes for the ```UpdateManager()``` in the bootstrap DLL, but you have to modify the ```Update()``` method of GameManager.

To save your changes, scroll up the assembly list, right-click the ```Assembly-CSharp``` entry and click ```Save as...``` then save your assembly. Congratulations, you have successfully installed Spectrum.