---
title: Time
type: guide
order: 240
---

## Time

> This article is translated by machine

The CatLib time component allows you to create your own independent time system.

### Default time

The default time is based on the simple packaging of Unity time.

### Time API

The description of the following is based on the `default time`, and if the `extended time` is used, its meaning may change.

#### **Time**

From the beginning of the game to the present time (seconds)

``` csharp
var time = timeManager.Default.Time;
```

#### **DeltaTime**

Time from the previous frame to the current frame (seconds)

``` csharp
var deltaTime = timeManager.Default.DeltaTime;
```

#### **FixedTime**

Game start to the present time (seconds)

Updated by fixed time

``` csharp
var fixedTime = timeManager.Default.FixedTime;
```

#### **TimeSinceLevelLoad**

From the current scene to the current time (seconds)

``` csharp
var timeSinceLevelLoad = timeManager.Default.TimeSinceLevelLoad;
```

#### **FixedDeltaTime**

Fixed frame update time

``` csharp
var fixedDeltaTime = timeManager.Default.FixedDeltaTime;
```

``` csharp
timeManager.Default.FixedDeltaTime = 0.2f;
```

#### **MaximumDeltaTime**

Can be obtained between the maximum frame and the frame between the update time

``` csharp
var maximumDeltaTime = timeManager.Default.MaximumDeltaTime;
```

#### **SmoothDeltaTime**

Smooth update time, according to the weighted average of the previous N frames

``` csharp
var smoothDeltaTime = timeManager.Default.SmoothDeltaTime;
```

#### **TimeScale**

Time scaling factor

``` csharp
var timeScale = timeManager.Default.TimeScale;
```

``` csharp
timeManager.Default.TimeScale = 0.5f;
```

#### **FrameCount**

The total number of frames from the beginning of the game to the present

``` csharp
var frameCount = timeManager.Default.FrameCount;
```

#### **RealtimeSinceStartup**

From the beginning of the game to the total time so far (even if the time scale factor is 0 will grow)

``` csharp
var realtimeSinceStartup = timeManager.Default.RealtimeSinceStartup;
```

#### **CaptureFramerate**

Frame rate per second

``` csharp
var captureFramerate = timeManager.Default.CaptureFramerate;
```

``` csharp
timeManager.Default.CaptureFramerate = 30;
```

#### **UnscaledDeltaTime**

The update time between the frame and the frame of the time scaling factor is not calculated.

``` csharp
var unscaledDeltaTime = timeManager.Default.UnscaledDeltaTime;
```

#### **UnscaledTime**

Regardless of the time scaling factor, from the beginning of the game to the total time so far

``` csharp
var unscaledTime = timeManager.Default.UnscaledTime;
```

### Expansion time

You can extend the new time through the `Extend ()` method.

``` csharp
timeManager.Extend(()=>
{
    return new UnityTime();
},"NewTime");
```

With `Get ()` you can get your time to expand.

``` csharp
var timeSystem = timeManager.Get("NewTime");
```