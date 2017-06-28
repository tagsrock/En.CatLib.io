---
title: Filter Chain
type: detail
order: 103
---

## Filter Chain

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/detail/stl/filterchain.md)

The filter chain is the role of passing the data to a task sequence, the filter chain actor's pipeline, the data being processed here and then passed to the next step.

The filter chain can break down complex processes into multiple independent subtasks. Each individual task is reusable, so these tasks can be combined into complex processes.

### Scenes to be used

CatLib framework is used in many parts of the filter chain, one of the most typical is the routing system middleware.

The vast majority of the areas where we need to implement the filter chain will be at the bottom of the project.

### Method

- [Add](#Add)
- [Do](#Do)
- [FilterList](#FilterList)

### Add

Add a processor to the filter chain, the processor according to the priority of sorting.

``` csharp
filterChain.Add((data, next)=>
{
    // todo:
},100);
```

### Do

Pass the data into the filter chain and execute the processor in turn.

``` csharp
filterChain.Do("helloworld" , (data)=>
{
    //todo
});
```

### FilterList

Gets all the processors in the filter chain.

``` csharp
var list = filterChain.FilterList;
```

