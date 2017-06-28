---
title: Container
type: guide
order: 101
---

## Container

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/container.md)

A service container is a powerful tool for managing class dependencies and performing dependency injection. Its essence is to inject the `constructor` or the attribute selector labeled `[Inject]` property by reflection (injecting the service implementation dependency into the class).

### Introduction

Almost all service bundles are done in the service provider. If a or class does not have any interface or alias, then it is not necessary to bind it to the container.

The container does not need to be told how to build the object because it uses the reflection service to automatically resolve the specific object. In a service provider, you can access the container with the `App` variable, and then use the` Bind () `method to register a service or class binding.

> The code used in the following documents is local code. You need to adjust the use of the scene as appropriate.

### Build a service container

``` csharp
var container = new Container();
```

CatLib core (`Application`) inherited the service container, she has all the functions of the service container, so the following examples of the demo are used in this context.

### Dependent injection marks

The CatLib container uses a feature tagging method to identify which attributes are allowed to be injected.

- Any attribute selector that needs to be dependent on the injection needs to mark the `[Inject]` property, and its `Setter` access level must be` public`.
- Arbitrary constructor parameters are automatically injected
- Dependent injection marks can be applied to the parameters of the constructor to provide aliases and constraints.

``` csharp
public class Service
{
    [Inject]
    public IFileSystem LocalFileSystem { get; set; }

    public Service(IFileSystem fileSystem)
    {
        // fileSystem and LocalFileSystem are automatically injected
    }
}
```

> Note that you can not access the `property selector` marked as injected in the `constructor 'because the injection of the property selector is done after the constructor.

### Must rely on injection

In some scenarios it may be necessary to inject certain dependencies, then we can mark the strong dependency by injecting the marked `Require` so that the frame will throw a` RunTimeException` exception when it can not be injected.

``` csharp
public class Service
{
    [Inject(Require = true)]
    public IFileSystem LocalFileSystem { get; set; }
}
```

### Binding service

``` csharp
App.Instance.Bind<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

The code is a simple binding, the realization of the role is `ConfigManager` bound to the container, and given the` IConfigManager` (interface alias) and `config.manager` (string alias) alias.

You can then use the alias to build the service:

``` csharp
var manager1 = App.Instance.Make<IConfigManager>();
var manager2 = App.Instance.Make("config.manager") as IConfigManager;
```

### Binding Service (Singleton)

In the above binding we will find `manager1` and` manager2` memory address is not equal, this is due to the use of conventional binding caused by the conventional binding service, each `Make ()` will be generated New service examples.

Below we will use the single case binding to bind the service:

``` csharp
App.Instance.Singleton<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

### Bind instance

CatLib also allows you to bind the instance you built, and then the call to the instance of the container always returns the given instance.

``` csharp
App.Instance.Instance("config.manager" , new ConfigManager());
```

It should be noted that in the following cases will not be able to bind:

- The service uses regular binding when binding services

### Alias

Alias is a very important function of the service container, it can provide: interface binding specified implementation of the ability.

``` csharp
App.Instance.Singleton<ConfigManager>().Alias<IConfigManager>().Alias("config.manager");
```

In the high-level use only need to use aliases can get ConfigManager services, without having to focus on the underlying implementation, which can be achieved without perception replacement.

### Context binding

Sometimes we may have two classes using the same interface, but we want to inject different implementations in each class, and we can specify the implementation for it by the following code.

``` csharp
App.Instance.Singleton<FileLog>().Alias("log.file");
App.Instance.Singleton<DatabaseLog>().Alias("log.database");
```

(`FileLog` and` DatabaseLog` both implement the `ILog` interface)

``` csharp
App.Bind<LogService1>().Needs<ILog>().Given("log.database");
App.Bind<LogService2>().Needs<ILog>().Given("log.file");
```

So that when the `LogService1` service is generated, the given ILog instance will be` DatabaseLog`, and the ILog instance given by the `LogService2` service will be` FileLog`.

In addition, we can use the `[Inject]` tag to specify the implementation of the injection instance. In general, CatLib's injection context binding function is used in the nearest principle (unless you do an extra context definition for aliases, this rare The situation is not covered by the description of this basic document).

``` csharp
public class LogService
{
    [Inject("log.database")]
    public ILog Log { get; set; }
    public LogService([Inject("log.file")]ILog logWithFile)
    {
    }
}
```

### Generate service

You can use the `Make ()` method to generate a service, which can be an interface or an alias. If the container is used to generate a service that is not bound, the container will attempt to resolve the service that needs to be generated, and if it tries to resolve the failure, it will return a `null`.

``` csharp
var manager = App.Instance.Make<IConfigManager>();
manager = App.Instance.Make("config.manager") as IConfigManager;
```

### Function injection call

The container allows you to function on a dependency injection. As with service injection, the container automatically recognizes the parameters required by the function and populates the demand parameters using dependency injection.

``` csharp
var instance = new DemoService();
// There is a method called FuncName in the DemoService class
var result = App.Instance.Call(instance , "FuncName");
```

### Service grouping

You can group multiple services, which will allow you to generate multiple services at once. This is useful in some scenarios such as: Logs will be logged to the file log service and the web log service.

``` csharp
App.Instance.Singleton<FileLog>().Alias("log.file");
App.Instance.Singleton<NetworkLog>().Alias("log.network");
App.Instance.Tag("log" , "log.file" , "log.network");
var logServices = App.Instance.Tagged("log");
```

### Release static service

The container allows you to release static services that have already been generated in order to reduce unnecessary memory overhead.

You can release static instances with a service name or alias:

``` csharp
App.Instance.Release("config.manager");
```

### Unbind

You can unbind a service that has been bound by `UnBind ()`. If the bound class is static, unbinding will automatically release the static instance that has been generated, and the `OnRelease ()` release event is triggered.


``` csharp
var binder = App.Instance.Singleton<ConfigManager>().Alias("config.manager");
binder.UnBind();
```

### When the query generates the type

When the container tries to guess a service that has not been bound, it queries the `Type` of the service by traversing the query function registered with` OnFindType () `.

In general, this use scenario is mostly used for type lookups across assemblies.

``` csharp
App.Instance.OnFindType((finder)=> Type.GetType(finder) , 200);
App.Instance.OnFindType((finder)=> MyType.GetType(finder) , 100);
```

`OnFindType ()` allows a priority to be passed, and a higher priority will be called first.

> Note that when the generated service relies on `OnFindType ()` to guess the service, the service build will use the c # reflection service, which is very damaging to performance. So you should use static bindings as much as possible instead of relying on speculation to generate them dynamically.

### When the service is resolved (built)

The service container triggers a service-resolved event every time a new parsing object can be used, and the event can be monitored using the `OnResolving ()` method.

Use scenes are often used to correct service modifications (initialization, monitoring, etc.), allowing you to set additional properties for an object before it is delivered to the consumer.

The resolution event is divided into two types, and the local resolution event will be better than the global resolution event being called:

- Resume based on the specified service will only be triggered if the specified service is resolved.
- Based on global resolution of the event, any service solution or binding instance will trigger.

``` csharp
// Service-based settlement event
App.Instance.Singleton<ConfigManager>().OnResolving((binder, obj) =>
{
    //todo: An instance of this type of ConfigManager is modified
    return obj;
});
```

``` csharp
// Based on global resolution
App.Instance.OnResolving((binder, obj) =>
{
    //todo: An instance of a solution that resolves all services or binds
    return obj;
});
```

### When the static instance is released

If a static instance that is hosted on a service container is released, a release event is triggered and the event can be monitored using the `OnRelease ()` method.

Hosted static instances include:

- Static instance bound by `Instance ()`
- Bind and generate static instances via `Singleton ()`

``` csharp
// Service-based static instance release event
App.Instance.Singleton<ConfigManager>().OnRelease((binder, obj) =>
{
    //todo: For ConfigManager this type of static instance is released
    return obj;
});
```

``` csharp
// Global event release event based on global
App.Instance.OnRelease((binder, obj) =>
{
    //todo: For a static instance that resolves all services or binds when released
    return obj;
});
```

### Performance optimization

In general, we recommend using a static binding method to instance without dependency injection. So that you can get better performance.

The following code will be statically bound:

``` csharp
App.Instance.Singleton<ConfigManager>((binder, param)=> new ConfigManager()).Alias("config.manager");
```

The following code will be dynamically bound:

``` csharp
App.Instance.Singleton<ConfigManager>().Alias("config.manager");
```

In addition to static binding, we also need to set the service as a single case binding as much as possible, the service container for the optimization of the single case service is better than the case service.

We have tested the time it takes for the service to generate the call for your reference, and all tests are based on the `1 million` service generation test (the average of 100 tests), and the compiled assembly on Debug.

- static binding generation (for singleton binding): `329 ms`
- static binding generation (instance binding): `844 ms`
- Dynamic binding generation (for singleton binding): `365 ms`
- Dynamic binding generation (instance binding): `11537 ms`