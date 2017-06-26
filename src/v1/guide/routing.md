---
title: Routing
type: guide
order: 210
---

## Routing

> This article is translated by machine

The CatLib routing system gives you the ability to schedule a function through a uri.

### Basic concept

#### **Basic composition**

The CatLib routing system consists of a router, a feature route, a route entry, a routing compiler, and a routing group.

`Router` is responsible for the scheduling of the entire routing system

`The routing entry` is the basic unit in the routing system, and she identifies the specific route and route destination.

`Routing group` is a group scope, a routing group can have multiple routing entries, and vice versa routing entries can also have multiple routing groups.

The `attribute route` is an extension of the routing system that allows the developer to mark the routing method after the route object method is marked in the form of a feature.

`The routing compiler` is used to compile the routing entries. The compiled routing entries can be used by the router.

#### **Glossary**

`Dispatch` a method that calls the route by way of routing.

### Scenes to be used

- (client, server) through the URI control client switch UI, scenes and so on.
- allow blocking the UI, the scene jump process, handle the client buried and other logic.
- Cross-component API calls allow parameter passing and parsing, and component decoupling by controlling inversion.
Debug instruction control

### Initial configuration

The initial configuration must be configured before the framework is initialized. The following is the initial configuration required by the routing component:

| Configuration name                            | Must | Configuration description (click to view details)                 |
| -------------------------------- |:------:|:--------------------------------------:|
| `routing.stripping.reserved`     | No      | [Compile-time compiler compiled assembly](#Routing compiler stripping)  |

### Legal URI

The URIs that conform to the following RFC definitions are legal URIs that can be parsed by the CatLib routing system:

- [RFC1808](https://www.ietf.org/rfc/rfc1808.txt)
- [RFC1738](https://www.ietf.org/rfc/rfc1738.txt)
- [RFC2396](https://www.ietf.org/rfc/rfc2396.txt)
- [RFC2732](https://www.ietf.org/rfc/rfc2732.txt)

### Basic registration

Registering a basic route requires only a `url` with a` lambda`, and if you do not define uri's `scheme` then the default value will be used:

``` csharp
Router.Instance.Reg("main", (request, response) =>
{
    response.SetContext("hello world");
});
Router.Instance.Dispatch("catlib://main");
```

### Attribute routing registration

CatLib also allows you to add a route tag `Routed`'for the class. Note that the class to be routed must be marked as `[Routed]` and you need to mark `[Routed]` on the routing method, and ensure that the method Access level is `public`:

``` csharp
[Routed]
public class AttrRouting
{
    [Routed]
    public void Call(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }
}
```

``` csharp
Router.Instance.Dispatch("catlib://attr-routing/call");
```

### Required routing parameters

Sometimes we need to catch some fragments of the `url`'in the route. For example, if we need to capture from the url to access the props detailed interface `props id`, we can define the routing parameters:

``` csharp
Router.Instance.Reg("props-detail/{id}", (request, response) =>
{
    response.SetContext("props id:" + request["id"]);
});
```

You can also define multiple parameters as needed:

``` csharp
Router.Instance.Reg("bag/{type}/{sub_type}", (request, response) =>
{
    response.SetContext("bag type:" + request["type"] + " , sub type:" + request["sub_type"]);
});
```

The routing parameters are usually placed in `{}`, and the parameter name can only be: `a-z`,` A-Z`, `0-9`, and `_`.

> Note that the routing parameter can not contain the `-` character. Please use `_` to replace.

### Optional routing parameters

You can also specify that the routing parameter is an optional parameter. If you need to specify an argument as an optional parameter, you can add the `?` to the end of the parameter.

``` csharp
Router.Instance.Reg("character/{tag?}", (request, response) =>
{
    response.SetContext("tag:" + request["tag"]);
});
```
If `tag` does not match the appropriate parameter, the default value will be used.

### Routing dispatcher

If you want to call a registered route, you can use the `Dispatch ()` method. Method to accept a `uri` and` context `

``` csharp
IResponse response = Router.Instance.Dispatch("character/10" , "hello world");
```

### Parameter regular constraint

You can use `Where()` to constrain the format of your routing parameters. The `where()` method accepts regular expressions for parameter names and defined parameter constraints.

``` csharp
Router.Instance.Reg("character/{tag?}", (request, response) =>
{
    response.SetContext("tag:" + request["tag"]);
}).Where("tag", "[0-9]+");
```

### Parameter default value

If you define an optional route, then when we do not match the corresponding parameters, we often need a default value. You can use the `Defaults()` method to set a default value for a parameter.

``` csharp
Router.Instance.Reg("character/{tag?}", (request, response) =>
{
    response.SetContext("tag:" + request["tag"]);
}).Default("tag", "1");
```

### Routing group

The routing group allows you to share the characteristics of the route. The features that can be shared are: parameter defaults, parameter regular constraints, processing middleware, and route exception middleware.

``` csharp
Router.Instance.Group(()=>
{
    Router.Instance.Reg("character/{type?}", (request, response) =>
    {
        response.SetContext("type:" + request["type"]);
    });

    Router.Instance.Reg("bag/{type?}", (request, response) =>
    {
        response.SetContext("type:" + request["type"]);
    });
}).Where("type" , "[0-9]+");
```

### Associate multiple routing groups

A routing entry can be associated with multiple routing groups, which can be set by the name of a given routing group.

``` csharp
IGroup group1 = Router.Instance.Group("group1");
group1.Where("type" , "[0-9]+");

IGroup group2 = Router.Instance.Group("group2");
group1.Where("page" , "[0-9]+");

Router.Instance.Reg("character/{type?}", (request, response) =>
{
    response.SetContext("type:" + request["type"]);
}).Group("group1").Group("group2");
```

> Note that in general we do not recommend associating multiple routing groups, which can cause the problem to become more complicated. If you need to use, please plan ahead.

### Master compilation process

Get a valid routing entry that can be used, CatLib has done a lot of work inside. Routing entries are usually compiled twice, compiled as follows:

`Attribute routing compilation` => `entry attribute compilation`

The attribute routing compilation takes place when the framework starts, and the entry feature compiles until it is compiled with the route entry.

### Attribute routing compilation process

The CatLib Routing Compiler scans the classes marked by the feature when the framework starts and parses it into the corresponding routing entries (during the compilation process, the parameters are not overwritten). The execution flow is as follows:

- `Scan all sorted routes`
    - `A method of resolving all marked routes in a class`
        - `Compile method Where constraint`
        - `Compile method Defaults default value`
        - `Compilation method group`
    - `Compile class Where constraint`
    - `Compile class Defaults constraint`
    - `Compile class Group`

### Feature routing configuration

If you use feature routing, you can do this if you want to express the characteristics of the route in the feature route:

``` csharp
[Routed(Defaults="type=>1,tag=>2",Where="type=>[0-9]+,tag=>[0-9]+")]
public class AttrRouting
{
    [Routed(Defaults="type=>0")]
    public void Call(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }

    [Routed]
    public void Func(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }
}
```

The configuration of the feature route is distinguished using `=>` and `,`. `Parameter 1 => parameter 1 value, parameter 2 => parameter 2 value`.

In this example, we define two default values for all the feature routes under `AttrRouting` 'class:` type` and `tag`, which are` 1` and `2`, respectively. Also defines the parameter regular constraints.

In the method `Call` we define the method's routing default value` type`. According to `attribute routing compilation process` so the default value of the default will be `0`.

### Feature routing scheme

Feature routing can define Scheme by giving an absolute address

``` csharp
[Routed("ui://main")]
public class AttrRouting
{
    [Routed("call")]
    public void Call(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }

    [Routed("ui2://func")]
    public void Func(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }
}
```

``` csharp
Router.Instance.Dispatch("ui://main/call");
Router.Instance.Dispatch("ui2://func");
```

The above code defines the attribute route in the form of an absolute address so that the default `Scheme` will not be used at compile time.

### What is routing middleware

Routing middleware allows interception, modification of routing requests and responses, and middleware will be executed in order.

Routing middleware on the macro side is divided into: `global middleware`, `middleware`, `routing entry middleware`, middleware implementation process is:

(Dispatching entry) `global middleware` => `class middleware` => `routing entry middleware` => `target routing method`
(Dispatching exit) `global middleware` <= `class middleware` <= `routing entry middleware` <= `target routing method`

> Global middleware, middleware, and routing entries Middleware are different filter chain pools so their `priority 'does not have an impact.

### Routing entry middleware

The routing entry middleware belongs to the filter chain pool of the routing entry middleware.

The routing entry middleware can set the middleware for the current routing entry, which can specify the specific route to execute the specified middleware.

``` csharp
Router.Instance.Reg("character/{type?}", (request, response) =>
{
    response.SetContext("type:" + request["type"]);
}).Middleware((request,response,next)=>
{
    // Middleware logic before routing execution
    var result = next(request,response);
    // Middleware logic after the route is executed
    return result;
});
```

### Routing group middleware

The routing group middleware belongs to the filter chain pool of the routing entry middleware.

The routing group middleware will take effect on routes within all groups.

``` csharp
Router.Instance.Group(()=>
{
  // Register the routing entry  
}).Middleware((request,response,next)=>
{
    // Middleware logic
    return next(request,response);
});
```

### Class Middleware

Class middleware belongs to the `class middleware` 'filter chain pool.

By way of feature routing and class registration, you will be allowed to set the class middleware interface: `IMiddleware`

``` csharp
[Routed]
public class AttrRouting : IMiddleware
{
    public IFilterChain<IRequest, IResponse> Middleware 
    {
        get
        {
            // Filter chain
        }
    }
    
    [Routed]
    public void Call(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }
}
```

If the middleware interface is set in your routing tag class, the middleware will be set when scheduling the routing method under the current class.

### Global middleware

The global middleware belongs to the filter chain pool of `global middleware`.

The global middleware handles all scheduled routing entries.

``` csharp
Router.Instance.Middleware((request,response,next)=>
{
    // Middleware logic
    return next(request,response);
});
```

### Can not find route entry

The CatLib routing system allows you to use middleware to handle `NotFoundRouteException` (you can not find a route entry that can be used).

``` csharp
Router.Instance.OnNotFound((request , next)=>
{
    // Logical processing
    return next(request);
},100);
```

You can pass a `priority 'to determine which processing scheme is executed first, which is useful when terminating bubbling.

### Exception handling

In the implementation of the target method, may lead to some exceptions, so we need to deal with exception.

CatLib routing exception handling system in the event of an exception will be bubbling to deal with exception handling chain, exception handling is divided into: `global exception handling` and `local exception handling`, as with middleware, their filter chain pool is different.

Exception handling always starts locally, and CatLib's exception handler calls the exception handler in turn, bubbling (if the bubble is terminated, then the exception is thrown).

`Trigger exception` => `local exception handling` => `global exception handling` => `Dispatch() throws exception`

> Please note the exception handling of `recursive route scheduling`.

#### **Routine handling of routing entries**

The exception handling of the routing entry belongs to `local exception handling`.

``` csharp
Router.Instance.Reg("character/{type?}", (request, response) =>
{
    response.SetContext("type:" + request["type"]);
}).OnError((request,response,ex,next) =>
{
    // Before other exception handling classes are executed
    var result = next(request,response,ex);
    // After other exception handling classes are executed
    return result;
});
```

#### **Routing group exception handling**

The exception handling of the routing entry belongs to `local exception handling`.

``` csharp
Router.Instance.Group(()=>
{
  // Register the routing entry  
}).OnError((request,response,ex,next) =>
{
    // Before other exception handling classes are executed
    var result = next(request,response,ex);
    // After other exception handling classes are executed
    return result;
});
```

#### **Global exception handling**

The exception handling of the routing entry belongs to the `global exception handling`.

``` csharp
Router.Instance.OnError((request,response,ex,next) =>
{
    // Before other exception handling classes are executed
    var result = next(request,response,ex);
    // After other exception handling classes are executed
    return result;
});
```

### Recursive routing scheduling

CatLib routing system has been a good recursive call to support, you can safely recursive call.

#### **The process of exception handling when recursive routing**

Assuming we have made a recursive call, the exception will be triggered by the following process:

`Trigger Exception` => `Local Exception Handling (Nested Dispatch ("ui://call2"))` => `Global Exception Handling` => `Local Exception Handling (ui://call)` => `Global Exception Handling` => `Dispatch () throws an exception`

``` csharp
Router.Instance.Reg("ui://call" , (request,response)=>
{
    Router.Instance.Dispatch("ui://call2");
});
Router.Instance.Reg("ui://call2" , (request,response)=>
{
    throw new Exception("ex");
});
Router.Instance.Dispatch("ui://call");
```

### Modify the default Scheme

The default scheme for the CatLib routing system is `catlib`. But you can modify the default Scheme by `SetDefaultScheme ()`.

``` csharp
Router.Instance.SetDefaultScheme("home");
```

### Route compilation stripping

The CatLib routing system strips off unwanted scans when compiling.

The following assembly will not be stripped at compile time:

- Assembly-CSharp
- Assembly-CSharp-Editor-firstpass
- Assembly-CSharp-Editor
- CatLib
- CatLib.Tests

You can do this by configuring: `routing.stripping.reserved` to fill in an assembly that is not to be stripped , Use `;` to separate assemblies.

### Routing is automatically named

When you use a feature route, if you do not have a given route `uri` then you will use the route automatic naming mechanism.

``` csharp
[Routed]
public class AttrRouting
{
    [Routed]
    public void Call()
    {
        res.SetContext("hello world");
    }
    [Routed]
    public void CallNPC()
    {
        res.SetContext("hello world");
    }
}
```

Automatic naming rules are: according to class name or function name to all lower case, between the words with `-`for the segmentation of the string, continuous capitalization as a word.

The above route will be converted to: `catlib://attr-routing/call` and `catlib://attr-routing/call-npc`.

### Automatic route injection

When you use feature routing, you can use automatic parameter injection to get the required parameters. The CatLib container automatically parses and injects the demand parameters.

Where the `IRequest` and` IResponse` types are requests and responses for the current route.

``` csharp
[Routed]
public class AttrRouting
{
    [Routed]
    public void Call(IRequest request, IResponse response)
    {
        res.SetContext("hello world");
    }

    [Routed]
    public void Call2(IResponse response, IApplication App , IRouter router)
    {
        res.SetContext("hello world");
    }
}
```

### Performance optimization

The CatLib routing system is a complex system that you should not use in high performance requirements or frequently called.

We have given the following optimization options:

- Define different schemes for different types of routing destinations
- Reduce the complexity of constraints
- Use manually registered routes