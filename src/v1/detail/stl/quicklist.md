---
title: Quick list
type: detail
order: 101
---

## Quick list

Quick list sorted by insert order. You can add an element to the list of head (left) or tail (right).

Quick list macro is a two-way list, micro-array. Compared to the traditional list, to avoid the changes caused by large-scale data migration.

### Method

- [Add](#Add)
- [Clear](#Clear)
- [Count](#Count)
- [First](#First)
- [GetRange](#GetRange)
- [InsertAfter](#InsertAfter)
- [InsertBefore](#InsertBefore)
- [Last](#Last)
- [Length](#Length)
- [Pop](#Pop)
- [Push](#Push)
- [Remove](#Remove)
- [ReverseIterator](#ReverseIterator)
- [Shift](#Shift)
- [Trim](#Trim)
- [UnShift](#UnShift)

### Add

Insert `element` into the end of the list.

``` csharp
quicklist.Add("hello world");
```

### Clear

Clear the quick list

``` csharp
quicklist.Clear();
```

### Count

Get the number of `element`s for the quick list.

``` csharp
var num = quicklist.Count;
```

### First

Gets the first element in the quick list. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = quicklist.First();
```

### GetRange

Get all the `element` in the interval.

``` csharp
var range = quicklist.GetRange(10,20);
```

### InsertAfter

Insert new `element` after specifying` element` '.

``` csharp
quicklist.InsertAfter("hello world" , "new hello world");
```

### InsertBefore

Insert new `element` before specifying` element` '.

``` csharp
quicklist.InsertBefore("hello world" , "new hello world");
```

### Last

Gets the last element in the quick list. If the ordered set is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = quicklist.Last();
```

### Length

Get the number of `nodes' for the quick list. (Quick list macro for the two-way linked list, each node can have N elements)

``` csharp
var num = quicklist.Length;
```

### Pop

Remove and return `element` at the end of the list. If the list is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = quicklist.Pop();
```

### Push

Insert `element` into the end of the list. (And Add is equivalent)

``` csharp
quicklist.Push("hello world");
```

### Remove

According to the value of the parameter `count`, remove the elements in the list that are equal to the parameter` element`.
If `count> 0` starts searching from the beginning of the table, remove the elements equal to` element`, the number `count`.
If `count = 0` starts searching from the beginning of the header to the end of the table, remove all elements that are equal to` element`.
If `count <0` starts looking at the header from the end of the table, remove the elements equal to` element`, the number `count`.

The return value is the number of elements to be removed.

``` csharp
var num = quicklist.Remove("hello world" , 10);
```

### ReverseIterator

Invert iterator iterations.

> Note that this is not really the meaning of reversing the entire quick list, just reversing the iteration order.

``` csharp
quicklist.ReverseIterator();
```

### Shift

Remove and return the `element` of the list header. If the list is empty, the `InvalidOperationException` exception is thrown.

``` csharp
var element = quicklist.Shift();
```

### Trim

Trim the list, leaving only the elements within the subscript range (including min and max). The return value is the number of elements to be removed.

``` csharp
quicklist.Trim(10,200);
```

### UnShift

Insert `element` into the list header.

``` csharp
quicklist.UnShift("hello world");
```

