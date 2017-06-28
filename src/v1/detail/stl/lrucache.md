---
title: LRU Cache
type: detail
order: 102
---

## LRU Cache

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/detail/stl/lrucache.md)

Recently, the use of the cache at least based on the historical record of data to record data. When the chain is full, the data will be discarded at the end of the list.

> When there is hot data, LRU efficiency is very good, batch operation will lead to a sharp decline in LRU hit rate, cache pollution is more serious. So this structure is only suitable for simple cache.

### Method

- [Add](#Add)
- [Count](#Count)
- [Get](#Get)
- [Remove](#Remove)

### Add

Add a `element` to least use the cache in the near future, or move it to the cache header if it already exists.

``` csharp
lruCache.Add("key" , "val");
```

### Count

Gets the number of `element`s in the Lru cache.

``` csharp
var num = lruCache.Count;
```

### Get

Fetches `element` according to `key`, and returns the 'default' if passed.

``` csharp
var element = lruCache.Get("key" , "default val");
```

### Remove

Remove `element` according to` key`.

``` csharp
lruCache.Remove("key");
```