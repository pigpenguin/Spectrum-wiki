## How to use Spectrum
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
Modified ```Assembly-CSharp.dll``` has a new reference to ```Spectrum.Bootstrap.dll```. The bootstrap DLL has a method which searches for ```Distance_Data/Spectrum/Spectrum.Manager.dll``` and passes control to it. The bootstrap DLL is also responsible for update calls to the manager DLL which in turn calls Update() on all loaded plug-ins.

#### Installing Spectrum
Drop the ```Spectrum.Prism.exe``` and ```Spectrum.Bootstrap.dll``` into ```Distance_Data/Managed``` folder and run it with the following syntax: 
```Spectrum.Prism.exe Assembly-CSharp.dll Spectrum.Bootstrap.dll```

Spectrum should now be installed.