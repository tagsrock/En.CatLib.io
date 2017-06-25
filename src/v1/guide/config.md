---
title: Configuration
type: guide
order: 200
---

## Configuration

CatLib allows you to configure the system to configure the functional parameters required by the service provider. Each service provider will provide their own unique configuration information. You can learn about the configuration information from these providers' documents.

CatLib's configuration system helps you find, load, autofill, any type of configuration you need. Regardless of what the source may be (code embedded, CSV, YAML, XML, INI file or database).

### Basic concept

CatLib's configuration system allows you to extend multiple independent configuration containers, each of which can be configured with a different configuration locator to get the configuration.

What is the configuration locator? The configuration locator is used to get configuration from code, CSV, YAML, XML, INI files or databases.

> Note that all CatLib components will get data from the default configuration locator

### Document context

The snippet code that is demonstrated in the document does not have a special description using the following contextual relationship and the default configuration container:

``` csharp
IConfigManager manager = App.Instance.Make<IConfigManager>();
IConfig config = manager.Default;
```

### Default configuration container

CatLib's default configuration container, automatically mounted: Code configuration locator (used to get the configuration set from the code).

Your new expansion of the configuration container, will not mount any of the configuration locator, you need to manually mount.

### Get the configuration

If you need to get the configuration from the configuration component, you can get it through the `Get` method. The first parameter is the default value of the configuration parameter. When the configuration does not exist will use the default value to fill the missing configuration.

``` csharp
string val = config.Get("config.name", string.Empty);
```

### Set and save configuration

You can use the `Set` method to set the configuration data, the first parameter is the configuration name, the second parameter is the configuration value

``` csharp
config.Set("config.name", "hello world");
```

Simple `Set` does not save the data, you also need to call` Save` to store the data.

Note that not all configuration locators support saving data, for example: Code configuration locator will not save data, but will not throw an exception.

``` csharp
config.Save();
```

### Add locator

Adding additional configuration locators provides the ability to configure the system to access different areas of data.

``` csharp
config.AddLocator(new UnitySettingLocator(), 100);
config.AddLocator(new CodeConfigLocator(), 200);
```

The configuration locator allows the 'priority' to be passed to determine which configuration locator is used first, giving priority to the configuration data that was first matched.

> Note that when the configuration is configured, the configuration locator is traversed by priority, and if the configuration is found in the corresponding configuration locator, the configuration is overwritten and the iteration is terminated. If no configuration is found in all configuration locators, the configuration is written to the last configuration locator.

### Provided locator

CatLib offers you the following two configuration locators, which you can choose according to the scene.

| Locator name                | Description                                                                     |
| ---------------------- |:-----------------------------------------------------------------------:|
| `CodeConfigLocator`    | The code configures the locator to be able to set and read the configuration via code. The configuration of the code configuration locator is not saved.    |
| `UnitySettingLocator`  | Unity provides the PlayerPrefs implementation of the configuration locator adaptation class. The configuration can be saved.               |

### Add converter

CatLib configuration system allows you to obtain any type from the configuration, you only need to achieve the type of converter can be corresponding.

``` csharp
internal sealed class BoolStringConverter : ITypeStringConverter
{
    public string ConvertToString(object value)
    {
        // To achieve conversion
    }
    public object ConvertFromString(string value, Type to)
    {
        // To achieve conversion
    }
}
```

``` csharp
config.AddConverter(typeof(Bool) , new BoolStringConverter());
```

### Default support for conversion

CatLib default configuration container defaults to the following conversion types:

| Support type         | Conversion processing class                |
| -------------------- |:-----------------------:|
| `Bool`               | BoolStringConverter     |
| `Byte`               | ByteStringConverter     |
| `Char`               | CharStringConverter     |
| `DateTime`           | DateTimeStringConverter |
| `Decimal`            | DecimalStringConverter  |
| `Double`             | DoubleStringConverter   |
| `Enum`               | EnumStringConverter     |
| `Int16`              | Int16StringConverter    |
| `Int32`              | Int32StringConverter    |
| `Int64`              | Int64StringConverter    |
| `SByte`              | SByteStringConverter    |
| `Single`             | SingleStringConverter   |
| `String`             | StringStringConverter   |
| `UInt16`             | UInt16StringConverter   |
| `UInt32`             | UInt32StringConverter   |
| `UInt64`             | UInt64StringConverter   |

### Expand the configuration container

CatLib allows you to extend the configuration container, and you can define additional configuration containers by extending the method.

> If you want to use a custom configuration container, please implement the `IConfig` interface.

``` csharp
manager.Extend(()=>
{
    return new Config();
}, "new")
```

If you need to get a newly expanded configuration container, you only need to use `Get ()` to access.

``` csharp
IConfig extendConfig1 = manager.Get("new");
IConfig extendConfig2 = manager["new"];
// extendConfig1 == extendConfig2
```

### Modify the default configuration container

You can use a custom configuration container instead of the default configuration container. Modifying the configuration container used by Default will be your replacement configuration container.

> Note! Because CatLib's component configuration uses the default configuration container, it is prudent to modify the configuration of the component without knowing it.

``` csharp
manager.SetDefault("new.default");
IConfig config = manager.Default;
```