Since Spectrum Gamma, plugins have to provide an additional JSON-based manifest file without which a plugin directory is ignored. This wiki page aims to provide a quick rundown of all manifest fields.

### Example `plugin.json` manifest
```JSON
{
    "FriendlyName": "MyExamplePlugin",
    "Author": "Ciastex",
    "AuthorContact": "ciastexx@live.com",
    "CompatibleAPILevel": 6,
    "IPCIdentifier": "MyExamplePlugin",
    
    "ModuleFileName": "MyExamplePlugin.dll",
    "EntryClassName": "Entry",
    
    "Priority": 10,
    "SkipLoad": false,
    
    "Dependencies": []
}
```
|Property|Description|
|--------:|:-----------|
|`FriendlyName`|Plugin's display name. End-user will see this when Spectrum loads your plugin.|
|`Author`|Describes who created the plugin.|
|`AuthorContact`|Provides some way to contact the author of the plugin.|
|`CompatibleAPILevel`|Defines the lowest safe API level the plugin will work reliably with. Highest version recommended.|
|`IPCIdentifier`|Used by Spectrum to route inter-plugin communication packets to correct plugins.|
|`ModuleFileName`|Tells Spectrum which file contains your plugin code. The file must not reside in any subdirectories.|
|`EntryClassName`|Tells Spectrum which class in your plugin contains the initialization code.|
|`Priority`|Defines the load order. Higher priority means higher place on the internal load list.|
|`SkipLoad`|Tells Spectrum not to load a plugin.|
|`Dependencies`|A JSON array of strings. Defines which files from `Dependencies/` directory of your plugin to load.|

