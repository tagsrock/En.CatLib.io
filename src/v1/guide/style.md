---
title: Code Style
type: guide
order: 4
---

## Code Style

> This article is translated by machine

This document describes the naming conventions in the CatLib framework.

> If you want to contribute to CatLib, you must follow this naming convention document.

　
> If the access level (public | private, etc.) is not explicitly specified, it means that all access levels are valid.

### Overview

- The code must be indented with `four spaces` instead of using tabs
- Use `using XXX` instead of `new XXX.xxx()`
- `{}` Must wrap, and the internal code `top grid write`
``` csharp
if(true)
{
    var tf = true;
}
```

- Use `///` to comment on `code`
``` csharp
/// <summary>
/// The code is commented here
/// </summary>
public string VariableName = "VariableName";
```

- For easy `ambiguous expressions` you should use `brackets` parcels
``` csharp
if(variable1 + (++variable2) > 0)
{
}
```

- The `template` must start with `T` for `TStudlyCaps`
``` csharp
public class Bootstrap<TType>
{
}
```

- `Interface` must start with `i` for `IStudlyCaps`
``` csharp
public interface IBootstrap 
{ 
}
```

### Class and function

- `Class` must be `StudlyCaps`
``` csharp
public class Bootstrap
{ 
}
```

- `Function` must be `StudlyCaps`
``` csharp
public void MyFunc()
{
}
```

### Variables, constants and attributes

- Class `public variable name` must be `StudlyCaps`
``` csharp
public string VariableName = "hello";
```

- The `protected variable name` must be `camelCase`
``` csharp
internal string variableName = "hello";
protected string variableName = "hello";
protected internal string variableName = "hello";
private string variableName = "hello";
```

- Class `property name` must be `StudlyCaps`
``` csharp
public string VariableName{ get; set; }
```

- `Static variables` and `constants` must be `StudlyCaps`
``` csharp
public const string VariableName = "hello";
public static readonly string VariableName = "hello";
public static string VariableName = "hello";
```

- `Parameter` must be `camelCase`
``` csharp
public void FunctionName(Action callFunction)
{
    var localVariable = callFunction;
}
```

### Enumerate

- `Enumeration` must be `StudlyCaps`
``` csharp
public enum ApplicationEvents
{ 
}
```

- `Enumeration element name` must be `StudlyCaps`
``` csharp
public enum ApplicationEvents
{
    OnStart  = 1,
    OnInited = 2,
}
```

### Namespaces

- `Code` must be in the root namespace of the `project name`
``` csharp
namespace CatLib
{
}
```

- The component `code` must be in the namespace of the `project name.Component name`
``` csharp
namespace CatLib.Routing
{
}
```

### File
- `File name` must be consistent with `class name`
- `File` must use `UTF-8` instead of BOM code
- A file can not appear in `two or more` classes, unless it is an internal class or class overload