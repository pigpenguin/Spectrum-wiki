#### Prism doesn't even start
Be sure you have .NET Framework 4.6 installed. Can be downloaded [here](https://www.microsoft.com/en-us/download/details.aspx?id=48130).

#### Prism starts, but nothing ever comes out to the console and it's frozen
If you have installed Distance inside a system protected directory (i.e. **Program Files**), make sure you run the .bat as the Administrator. If that still doesn't work, **disable your antivirus**. They are known to cause problems with Prism.

#### Prism starts, but says the assembly has already been patched
It's because you're trying to install Spectrum second time on the same patched DLL. This behavior is expected.