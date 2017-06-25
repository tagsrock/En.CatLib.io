---
title: Service facade
type: guide
order: 104
---

## Service facade

> This article is translated by machine

The facade provides a `static 'access interface for services that are bound within the service container. Compared to the direct use of container-generated services to use, the facade can provide more flexibility in testing, flexible, concise features.

CatLib all built-in facades are placed in the `Facade` component. If you want to use the facade please refer to the `CatLib.Facade` namespace.

### The facade of the principle

A facade is a class that provides access to objects in a container. The mechanism is implemented by the `Facade` class. You need to inherit from the `facade` class and then set the required template so that the facade can be accessed through the container.

``` csharp
public sealed class Router : Facade<IRouter>
{
}
```

In this example, here we set the facade of the routing system.

``` csharp
Router.Instance.Dispatch("bootstrap://start");
App.Instance.Make<IRouter>().Dispatch("bootstrap://start");
// The above two statements are equivalent
```

### Facade class list

| Facade name                | Description      |
| -------------------- |:------------:|
| `ConfigManager`      | Configuration manager     |
| `FileSystemManager`  | File System Manager |
| `Router`             | Routing system      |
| `TimeManager`        | Time Manager     |
| `TimerManager`       | Timer manager   |
| `Env`                | Environmental information      |