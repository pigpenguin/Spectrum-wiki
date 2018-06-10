Since Spectrum Gamma, the plugins no longer consist of a single DLL file. Instead, they are loaded from their own directories, have their own dependencies and own private filesystems. This wiki page attempts to explain the packaging process of every Spectrum Gamma plugin.

## Directory structure
The complete structure of a correctly packaged plugin looks like this:
```
MyExamplePlugin/
| - Dependencies/
| - Settings/
| - Assets/
| - Data/
| - Logs/
| - MyExamplePlugin.dll
\ - plugin.json
```
### Directory descriptions
|Directory name|Description|
|--------------|-----------|
|`Dependencies/`| Stores DLLs referenced by the plugin, outside of Spectrum-provided assemblies.|
|`Settings/`|Stores JSON configuration files for the plugin.|
|`Assets/`|Stores Unity AssetBundles the plugin can load at runtime.|
|`Data/`|Stores any data that doesn't fit the above directories. Accessed with `FileSystem` class.|
|`Logs/`|Stores any logs your plugin may create. Accessed with `Logger` class.|
|`MyExamplePlugin.dll`|Plugin's code file.|
|`plugin.json`|Plugin's [manifest](Plugin-manifest) file.|