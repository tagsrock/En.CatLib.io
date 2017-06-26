---
title: Service provider
type: guide
order: 100
---

## Service provider

> This article is translated by machine

The service provider is a bridge between the component and CatLib. It is also the center of CatLib startup, your own program and all the core services of CatLib are launched through the service provider.

> The service provider is a feature provided by the CatLib core. If you need to migrate CatLib components to other frameworks, you need to remove the service provider.

### Implement the service provider

Your service provider class must inherit from the `CatLib.ServiceProvider` class. Most service providers contain two methods: `Register()` and `Init()`, where `Register()` is required The

In the `Register()` method, the only thing you need to do is `bind the service implementation` to the service container, and do not try to perform any other functionality in it. Because it is possible that you are using a service that is not registered.

When the service provider's `Init()` method is triggered, it means that all the service providers of the framework of the `Register()` have been executed, that is, we can in Init() access to other service providers Of the service, only when all the service providers Init() implementation will be triggered after the completion of the framework to complete the event.

``` csharp
using CatLib;
public class ConfigProvider : ServiceProvide
{
    public override IEnumerator Init()
    {
        yield return base.Init();
    }
    public override void Register()
    {
        // Registration service
    }
}
```

### Registered service provider

> Registering a service provider does not mean that the service will be instantiated instantly. Normally, many are instantiated by instantiation, and will only be instantiated when they are actually used.

If the framework user wants to use a service, then the service must be registered first, you can only register the service provider before the frame `Application.Init()` is triggered:

``` csharp
App.Instance.Register(typeof(ConfigProvider));
```

The registration of the service provider should normally be done in the bootstrap of the `Application` 's boot process.

In addition to standard registration, the CatLib.Unity program provides developers with a quick registration scheme so that you do not need to write a standard registration code to register your service provider (if you do not have to do this from CatLib.Unity yourself ), You can configure the service provider with the `Game/Providers.cs` file. You only need to configure the return value of the `ServiceProviders` field to complete the service provider configuration.

``` csharp
public class Providers
{
    public static Type[] ServiceProviders
    {
        get
        {
            return new[]
            {
                typeof(ConfigProvider),
            };
        }
    }
}
```

### Initialize the priority

You can adjust the startup order of the service provider startup process by configuring the startup priority, and note that all of the priority features in CatLib are of the same principle. That is, the priority attribute of the function tag will take precedence over the priority attribute of the class.

``` csharp
using CatLib;
[Priority(200)]
public class ConfigProvider : ServiceProvide
{
    [Priority(100)]
    public virtual IEnumerator Init()
    {
        // Init's boot priority will use 100 instead of 200 due to the nearest principle
        yield break;
    }
    public override void Register()
    {
        // Registration service
    }
}
```

For more information on priority, please refer to: [Priority](application.html#Priority)

### Startup order

In addition to `Init()` can specify a specific service provider between the start priority outside the Register() is disordered.

Their processes are like this: All registrar providers execute `Register()` -> Execute the `Init()` -> framework startup by priority.