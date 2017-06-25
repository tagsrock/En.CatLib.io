---
title: API
type: guide
order: 103
---

## API

A service interface is a set of interfaces provided by the framework that define the core component services (hereinafter referred to as the service). Such as: `IRouter` interface defines the routing service can be used method.

Each interface frame has a corresponding implementation. For example, CatLib provides a variety of driver implementations for file systems.

All CatLib services are defined in a simple interface, it is easy to determine the function provided by a given service, the program interface can also act as a framework of the concise document.

CatLib all core interfaces are placed under the `CatLib.API` namespace.

### When to use

#### **Loose coupling**

In this class, the code and the given file system to achieve tight coupling (this is a `negative example`), because we need a specific implementation class, if the component API has changed, then the corresponding, our code must be modified.

``` csharp
public class Service
{
    private FileSystem localFileSystem;
    public Service()
    {
        localFileSystem = new FileSystem();
    }
}
```

Similarly, if we want to replace the underlying file service for other technology implementations, we will once again have to modify our code to adapt to the new file service.

So to avoid this, we can optimize our code based on a simple, provider-independent interface that replaces the implementation of the above:

``` csharp
public class Service
{
    [Inject]
    public IFileSystem LocalFileSystem { get; set; }
}
```

Now the code is not coupled with any particular provider, even with CatLib are irrelevant. Since the API does not contain any implementation and dependencies, you can easily write replaceable implementation code for a given interface, and you can replace the file service implementation without having to modify any high-level consumer code.

#### **Concise**

In addition, based on a simple interface, the code is easier to understand and maintain.

In a large and complex class, with which to track which methods are effective, it is better to turn to a simple, clean interface.

### How to use

You can mark `[Inject]` for the property selector or define the variable type as the interface type in the `constructor 'of the class.

``` csharp
public class Service
{
    [Inject]
    public IFileSystem LocalFileSystem { get; set; }

    public Service(IFileSystem fileSystem)
    {
        //Both the constructor and the property selector can be injected
    }
}
```

### Interface list

The following list of interfaces in the Catlib core that can be directly `Application.Make ()` (for CatLib.Unity):

| Interface name       | Description              |
| -------------------- |:------------------------:|
| `IConfigManager`     | Global configuration     |
| `IEnv`               | Environment              |
| `IFileSystemManager` | File system              |
| `IRouter`            | Routing                  |
| `ITimeManager`       | Time                     |
| `ITimerManager`      | Timer                    |