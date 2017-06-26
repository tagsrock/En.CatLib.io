---
title: Timer
type: guide
order: 250
---

## Timer

> This article is translated by machine

Use the timer to quickly and easily implement a series of timing-related functions such as timing callback, repeat execution, deferred execution, and interval execution.

### Get the timer manager

``` csharp
var timerManager = App.Instance.Make<ITimerManager>();
```

### Create a timer

You can create a timer by using the timer manager's `Make()` method.

> The `Make()` function essentially constructs an anonymous timer queue internally, with only one timer in the queue.

``` csharp
var timer = timerManager.Make(()=>
{
    // The timer needs to perform the task
});
```

### Delayed execution

#### **Delay after the specified number of seconds to execute**

You can use `Delay()` to delay the specified number of seconds after executing the timer task.

``` csharp
timer.Delay(3.25f);
```

#### **Delay Specifies the number of frames to execute**

You can use `DelayFrame()` to delay the specified number of frames after executing the timer task.

``` csharp
timer.DelayFrame(3.25f);
```

### Loop execution

#### **Cyclic execution within the specified time**

You can use `Loop()` to execute a timer task on each frame within a specified length of time.

``` csharp
timer.Loop(3.25f);
```

#### **Loop until the function returns false**

You can use `Loop()` to define a loop execution, each frame is executed until the function returns false.

``` csharp
timer.Loop(()=>
{
    return false;
});
```

#### **Cycle the specified number of frames**

You can use `LoopFrame()` to define a loop execution, each frame is executed until it reaches the specified number of frames and stops.

``` csharp
timer.LoopFrame(10);
```

### Interval execution

If you want to cancel the interval execution, use the handle to cancel the task.

#### **Executes once every specified number of seconds**

The timer task is executed once after each specified number of seconds.

``` csharp
timer.Interval(3.25f);
```

#### **Execute once every specified number of frames**

The timer task is executed once after the specified number of frames.

``` csharp
timer.IntervalFrame(10);
```

### Timer queue

The timer queue is executed by concatenating multiple timers in series. Only a timer will be executed after the previous timer completes the task.

You can pass a `priority` to determine the order in which the timer queue is executed globally.

``` csharp
timerManager.MakeQueue(()=>
{
    timerManager.Make(()=>
    {
        //Timer 1 task
    }).Delay(3);
    timerManager.Make(()=>
    {
        //Timer 2 task
    }).Loop(5);
},100);
```

### Cancel execution

Undo the execution of the timer (queue). After the revocation of the queue state can not be restored.

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Cancel(handler);
```

### Pause execution

Pause the execution of the timer (queue).

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Pause(handler);
```

### Resume execution

Recovery timer (queue) execution.

``` csharp
var handler = timerManager.MakeQueue(()=>
{
    // todo
});
timerManager.Play(handler);
```