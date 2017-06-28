---
title: Sort Set
type: detail
order: 100
---

## Sort Set

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/detail/stl/sortset.md)

Ordinal collections and collections are also collections of elements, and do not allow duplicate members. The difference is that each element will be associated with a score that can be sorted.

An ordered set is a fraction of the order of the members of the collection from small to large sort. The members of the ordered collection are unique, but the score can be repeated.

### Method

- [Add](#Add)
- [Clear](#Clear)
- [Count](#Count)
- [Contains](#Contains)
- [First](#First)
- [GetElementByRank](#GetElementByRank)
- [GetElementByRevRank](#GetElementByRevRank)
- [GetElementRangeByRank](#GetElementRangeByRank)
- [GetElementRangeByScore](#GetElementRangeByScore)
- [GetScore](#GetScore)
- [GetRank](#GetRank)
- [GetRevRank](#GetRevRank)
- [GetRangeCount](#GetRangeCount)
- [Last](#Last)
- [Pop](#Pop)
- [Remove](#Remove)
- [RemoveRangeByRank](#RemoveRangeByRank)
- [RemoveRangeByScore](#RemoveRangeByScore)
- [ReverseIterator](#ReverseIterator)
- [Shift](#Shift)
- [ToArray](#ToArray)

### Add

Add a `element` to an ordered collection, or update its score if it already exists and sort it to the correct location.

``` csharp
sortset.Add("this is element" , 10);
```

### Clear

Clear all elements of an ordered set.

``` csharp
sortset.Clear();
```

### Count 

Get the number of `element`s for an ordered set.

``` csharp
var num = sortset.Count;
```

### Contains

Determine whether there is `element` in order.

``` csharp
var isExists = sortset.Contains("this is element");
```

### First

Gets the first element in an ordered set. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = sortset.First();
```

### GetElementByRank

Get elements based on rank (ordered members are sorted by Score from small to large)

``` csharp
var element = sortset.GetElementByRank(10);
```

### GetElementByRevRank

Get the elements according to the rankings (ordered members are sorted by Score from big to small)

``` csharp
var element = sortset.GetElementByRevRank(10);
```

### GetElementRangeByRank

According to the list of elements within the scope of the `list, the order is sorted from small to large. (Defaults to `min` or` max`)

``` csharp
var list = sortset.GetElementRangeByRank(10,20);
```

### GetElementRangeByScore

According to the `score 'to get the list of elements within the range, sorted by scores from small to large. (Defaults to `min` or` max`)

``` csharp
var list = sortset.GetElementRangeByScore(100,200);
```

### GetScore

Returns the fraction of the `element` in the ordered set,` `keyNotFoundException` if` element` does not exist.

``` csharp
var score = sortset.GetScore("this is element");
```

### GetRank

Get the rank of `element`, ordered by the members in accordance with Score` from small to big` sort. If -1 is returned, the element is not found.

``` csharp
var rank = sortset.GetRank("this is element");
```

### GetRevRank

Get the rank of `element`, and the ordered members are sorted by Score` from big to small. If -1 is returned, the element is not found.

``` csharp
var rank = sortset.GetRevRank("this is element");
```

### GetRangeCount

Gets the number of elements in the fractional interval. (Defaults to `min` or` max`)

``` csharp
var num = sortset.GetRangeCount(10,100);
```

### Last

Gets the last element in an ordered set. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = sortset.Last();
```

### Pop

Remove and return the elements of the ordered set tail. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = sortset.Pop();
```

### Remove

Removes the specified `element` from the ordered collection and returns` false` if the removal fails.

``` csharp
var isSuccess = sortset.Remove("this is element");
```

### RemoveRangeByRank

Remove all `element`s within the scope of the specified 'list' and return the number of removed elements. (Defaults to `min` or` max`)

``` csharp
int remove = sortset.RemoveRangeByRank(10，20);
```

### RemoveRangeByScore

Remove all `element`s within the range of the specified 'fractional range' and return the number of removed elements. (Defaults to `min` or` max`)

``` csharp
int remove = sortset.RemoveRangeByScore(100，200);
```

### ReverseIterator

Invert iterator iterations.

> Note that this is not true meaning reversing the entire ordered set, but reversing the iteration order.

``` csharp
sortset.ReverseIterator();
```

### Shift

Remove and return the elements of the ordered set header. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = sortset.Shift();
```

### ToArray

Converts an ordered set to an array.

``` csharp
var array = sortset.ToArray();
```