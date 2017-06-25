---
title: CatLib Core
type: guide
order: 102
---

## CatLib Core

> This article is translated by machine

`Application` is the core of the CatLib program and is the so-called program entry. The application loads the service provider and other necessary resources by booting. The application is only allowed to start one in general, and can only be started in the main thread.

### Start the process

`Start boot` -> `initialize` -> `finish`

Boot boot is typically used to load a service provider, initial configuration, or some other resource, and do not generate any services through the framework during the boot process (unless you are familiar with the framework's startup process), you can use an unregistered service.

### Configuration

The CatLib configuration is divided into `initial configuration 'and' general configuration '.

`Initial configuration` The configuration required for the service provider to start, the initial configuration must be completed before the framework is started.

`General configuration` The configuration that is required for the user to use the functionality provided by the service provider.

### Initial configuration

The initial configuration must be configured before the framework is initialized. The following is the initial configuration required by the CatLib core:

| Configuration name                            | Must |  Description (click to view details)                |
| -------------------------------- |:------:|:--------------------------------------:|
| `env.debug`                      | No      | [Frame debug level](#Environment)                    |
| `env.asset.path`                 | NO      | [Resource file path](#Environment)                    |

### From scratch

Now we will start from scratch to start the catlib framework so that you can understand the framework to start the principle, in fact, in the real project we have to help you write the boot process.

First, we need to define a boot program, the boot program must inherit from the `IBootstrap` interface, we can in the boot process to register the service provider or perform other boot business. In general, we recommend that priority be given to the registration of service providers. Note that you can register the service provider only before initialization, and calling the `Register ()` once the frame is initialized will cause an exception.

``` csharp
public class GameBootstrap : IBootstrap
{
    public void Bootstrap()
    {
        App.Instance.Register(typeof(CoreProvider));
    }
}
```

Then we need an entry program for mounting with Unity and triggering the startup process. In the entry program, we only need to instance a CatLib application, and set the corresponding boot program (the boot program will be loaded according to the order of the order.) Finally, the implementation of initialization.

> Note that if you are using `CatLib.dll` there will be building cross-assembly issues, you need to provide the` OnFindType () `method to help the framework get the current assembly type.

``` csharp
public class Program : MonoBehaviour 
{
    public void Awake()
    {
        var application = new Application(this);
        application.OnFindType((t) => Type.GetType(t));
        application.Bootstrap(new Type[]{ typeof(GameBootstrap) });
        application.Init(()=>
        {
            // 框架启动完成
        });
    }
}
```

### Start with CatLib.Unity

In addition to the program described above, CatLib has prepared the boot program for the developer, and in the CatLib.Unity project you can configure the `Providers.cs` file to set the service provider for the framework.

CatLib.Unity default user code entry in the `Main.cs` file, of course, you can also use the routing tag to specify the boot entry (which requires you to delete the Main.cs file, in fact, the main.cs entry is completed by routing of).

Through the route to mark the conventional configuration entry, the regular configuration of the uri fixed to `bootstrap://config`
Through the route to mark the start entry, the import uri fixed to `bootstrap://start`:

``` csharp
[Routed]
public class Main
{
    [Routed("bootstrap://config")]
    public void Config()
    {
        //todo: config code here
        Debug.Log("config code here!");
    }

    [Routed("bootstrap://start")]
    public void Bootstrap()
    {
        //todo: user code here
        Debug.Log("hello world! user code here!");
    }
}
```

### Main thread call

The CatLib core provides the ability of subroutine code block scheduling to perform the main thread. This is very common in the game system.

``` csharp
App.Instance.MainThread(()=>
{
    //The code block executed in the main thread
});
```

Of course, you can also use `IsMainThread` to determine whether it is in the main thread.

``` csharp
var isMainThread = App.Instance.IsMainThread;
```

### Mount execution

Mount execution allows you to inherit from the part of `MonoBehaviour` with part of` MonoBehaviour` 's ability

You need to achieve any one of the enhanced interface can be mounted, you can achieve the enhanced interface: `IUpdate`,` ILateUpdate`, `IDestroy`

``` csharp
public class Element : IUpdate
{
    public void Update()
    {
        //todo
    }
}
```

``` csharp
var ele = new Element();
App.Instance.Attach(ele);
```

An object can only be mounted once, but you can uninstall the mounted object by uninstalling it:

``` csharp
App.Instance.Detach(ele);
```

In addition to the manual mount, if you have bound the service to the container and the binding type is singleton binding, then once you have implemented the enhanced interface, the framework will automatically help you mount the execution, and when you release Will also be automatically uninstalled.

> Note that only when your service is a single case, to achieve the enhanced interface, and is generated through the container, will automatically mount (if your own `new` object will not automatically mount Oh)

### Synergy

CatLib allows classes that are not inherited from `MonoBehaviour` to start a collaborative program through this interface.

``` csharp
App.Instance.StartCoroutine(/* todo: 你的协同函数 */);
```

### Priority

All of the enhanced interfaces of CatLib support the priority feature. You can use `[Priority ()]` to define the priority.

Definition: The lower the priority value in CatLib, the more priority.

> If other components also use the priority then they also follow the definition of CatLib priority.

### Global event

CatLib core provides a global event system, you only need a simple API can be called.

#### **Trigger a global event**

``` csharp
App.Instance.Trigger("event");
```

#### **Listen to global events**

When you listen to a global event, you are allowed to pass in the number of triggers. If you do not have an incoming message, the event is always valid. If the trigger is reached, the event is automatically revoked.

``` csharp
App.Instance.On("event" , (sender , e)=>
{
    // Accept the event
}, 10)
```

#### **Revoke the global event**

``` csharp
var handler = App.Instance.On("event" , (sender , e)=>
{
    // Accept the event
});
handler.Cancel();
```

#### **Listen to a global event**

``` csharp
var handler = App.Instance.One("event" , (sender , e)=>
{
    // Accept the event
});
```

### Intra-procedural GUID

The CatLib core provides the only GUID to get the current core instance.

``` csharp
var guid = App.Instance.GetGuid();
```

### Get the CatLib version number

You can get the current CatLib core version number with `Version`.

``` csharp
var version = App.Instance.Version;
```

### Environment

You can get environmental information via `IEnv` 's.

``` csharp
var env = App.Instance.Make<IEnv>();
```

#### **SetAssetPath**

You can set the resource path by `SetAssetPath`, and note that the resource path must be written.

- If the incoming value is `null` or` empty string ', it is not considered as a developer manually.

``` csharp
env.SetAssetPath(UnityEngine.Application.PersistentDataPath);
```

#### **AssetPath**

Gets the set resource path. `Default '

In editor mode:

- `Production environment` for : `UnityEngine.Application.PersistentDataPath`
- `Emulation Environment` for : `StreamingAssets`文件夹
- `Developer mode ` for : `UnityEngine.Application.dataPath`

Out of editor mode:

- Automatically set to : `UnityEngine.Application.PersistentDataPath`

In any mode, if the developer manually sets `SetAssetPath`. Then always return to the developer settings, `AssetPath`.

``` csharp
var assetPath = env.AssetPath;
```

#### **SetDebugLevel**

Set the debug level of CatLib. CatLib's debug level is divided into:

- `Prod`Production Environment
- `Staging`Simulation environment
- `Dev`Developer mode
- `Auto`Automatic mode, under the Unity editor for the developer mode, after the release for the production environment.

``` csharp
env.SetDebugLevel(DebugLevels.Prod);
```

#### **DebugLevel**

Get debug level.

``` csharp
var debugLevel = env.DebugLevel;
```