Spectrum API provides you with a JSON-based plugin-specific settings manager. This wiki page aims to explain how to correctly utilize this capability. To keep things simple, this guide will use example project setup used in [this part](https://github.com/Ciastex/Spectrum/wiki/Creating-a-Plugin:-The-Basics).

## Introduction
The settings manager allows each plugin to have multiple settings files distinguished by a postfix. The postfix is defaults to `string.Empty`. The `Settings` class resides in the `Spectrum.API.Configuration` namespace.

The `Settings` constructor prototype is:   
```CSharp
public Settings(Type type, string postfix = "")
```
### Standard behavior
Upon correct object creation, Spectrum generates an empty JSON file following the `$"{PluginName}.{postfix}.json"` template if postfix is provided, or `$"{PluginName}.json"` if the postfix is empty. The files are created in `%DISTANCE_PATH%\Distance_Data\Spectrum\Settings` folder.
If an error occurs, you will be notified in the development console.

## Code structure
### Creating a settings file
To create a settings file interface for your plugin you need a class-wide variable for that file. Add the following below the public property declarations and above the `Initialize(IManager manager)` method:
```CSharp
private Settings _settings;
```
This will allow you to initialize and access your settings file.

In the `Initialize(IManager manager)` method, add the following line on top of the method:
```CSharp
_settings = new Settings(typeof(Entry));
```
This is the standard way of creating your settings file.

### Adding values and sections to the settings file
#### Adding a value
To add values to your settings file you can either use the indexer facility of the created object, or its `Add(string key, object value)` method. By default, all values will be created in **root** of your settings file. You can add the setting called `foo` with a value of `123` (integer) using the following code:
```CSharp
_settings.Add("foo", 123);
// ^equivalent: _settings["foo"] = 123;
```

#### Adding a section
Adding a section is very similar to adding typical values, just create a new settings key with a value of a `Section` object:
```CSharp
_settings.Add("section", new Section());
// ^equivalent: _settings["section"] = new Section();
```

### Accessing values and sections
#### Accessing a value
To access a value stored in your settings file, you can use the `GetItem<T>` method of your `Settings` object:
```CSharp
var myValueFoo = _settings.GetItem<int>("foo"); // foo was integer type
```

#### Accessing a value from inside section
```CSharp
var myValueBar = _settings.GetItem<Section>("section").GetItem<string>("bar");
```

### Saving settings
A method called `Save(bool formatJson = true)` is used to save a settings file to a hard disk. The `formatJson` parameter is used to determine whether or not you want your file to be pretty-printed for easy reading and manual modification and defaults to `true` if not provided:
```CSharp
_settings.Save();
```

## Conclusion
After finishing this part of tutorial, you should have a fair understanding of the settings facility exposed by Spectrum.

### Final plugin code
```CSharp
using Spectrum.API;
using Spectrum.API.Configuration;
using Spectrum.API.Interfaces.Plugins;
using Spectrum.API.Interfaces.Systems;
using System;

namespace ExamplePlugin
{
    public class Entry : IPlugin
    {
        public string FriendlyName => "Example Plugin";
        public string Author => "someone";
        public string Contact => "contact@example.com";
        public APILevel CompatibleAPILevel => APILevel.UltraViolet;

        private Settings _settings;

        public void Initialize(IManager manager)
        {
            _settings = new Settings(typeof(Entry));

            _settings.Add("foo", 123);
            // ^equivalent: _settings["foo"] = 123;

            _settings.Add("section", new Section());
            // ^equivalent: _settings["section"] = new Section();

            _settings.GetItem<Section>("section")["bar"] = "baz";

            var myValueFoo = _settings.GetItem<int>("foo"); // foo was integer type
            var myValueBar = _settings.GetItem<Section>("section").GetItem<string>("bar");

            _settings.Save();

            Console.WriteLine("Hello, world.");
        }

        public void Shutdown()
        {

        }
    }
}
```