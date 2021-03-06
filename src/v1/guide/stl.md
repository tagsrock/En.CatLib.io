---
title: Standard library
type: guide
order: 220
---

## Standard library

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/stl.md)

CatLib standard template library provides a common set of components, they are added to the C # standard library. They are divided into: containers, algorithms, iterators, etc.

> Standard library components can not be generated by the container, you need to manually `new` out of the standard library components.

### Container

The CatLib service container is part of Stl. For more information about service containers, please refer to: [service container] (container.html)

### SortSet

Ordinal collections and collections are also collections of elements, and do not allow duplicate members. The difference is that each element will be associated with a score that can be sorted.

An ordered set is a fraction of the order of the members of the collection from small to large sort. The members of the ordered collection are unique, but the score can be repeated.

For more information on the ordered set, please refer to: [SortSet](/v1/detail/stl/sortset.html)

### QuickList

Quick list sorted by insert order. You can add an element to the head of the list (left) or tail (right).

Quick list macro is a two-way list, micro-array. Compared to the traditional list, to avoid the changes caused by large-scale data migration.

For a quick list of detailed documents please refer to: [Quick List](/v1/detail/stl/quicklist.html)

### LruCache

Recent use of the cache based on the historical record of data to carry out the data to eliminate the data. When the chain is full, the data will be discarded at the end of the list.

For more information on the recent use of the cache, please refer to: [LRU Cache](/v1/detail/stl/lrucache.html)

### FilterChain

The filter chain is the role of passing the data to a task sequence, the filter chain actor's pipeline, the data being processed here and then passed to the next step.

The filter chain can break down complex processes into multiple independent subtasks. Each individual task is reusable, so these tasks can be combined into complex processes.

For more information on the filter chain, please refer to: [Filter Chain](/v1/detail/stl/filterchain.html)
