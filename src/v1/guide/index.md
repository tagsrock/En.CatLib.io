---
title: Introduction
type: guide
order: 0
---

## Introduction

<a href="https://github.com/yb199478/CatLib/blob/master/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" title="license-mit" /></a> <a href="https://github.com/yb199478/catlib/"><img src="https://badge.fury.io/gh/catlib%2Fcatlib.svg" title="GitHub version" /></a> <a href="https://ci.appveyor.com/project/yb199478/catlib"><img src="https://ci.appveyor.com/api/projects/status/f12rb3x5hxvq6yr7?svg=true" title="Build status"/></a> <a href="https://codecov.io/gh/CatLib/CatLib"><img src="https://codecov.io/gh/CatLib/CatLib/branch/master/graph/badge.svg" alt="Codecov" /></a> <img src="https://img.shields.io/badge/unity-min%205.3-red.svg" alt="min unity" />

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/index.md)

　
> CatLib requires Unity minimum version 5.3+

### What is CatLib?

CatLib is a set of `progressive` service provider frameworks. The framework provides multiple implementations for the client and decouples them from multiple implementations. Service provider changes are transparent to their clients, which provides better scalability. CatLib is not only simple to get started with, but also easy to integrate with third-party libraries or existing projects.

### Advantages of CatLib

- CatLib is a progressive framework that integrates seamlessly with existing frameworks. You can easily integreate CatLib regardless of the stage of your project.
- CatLib provides a dependency injection scheme that can greatly assist in decoupling the project.
- CatLib provides a large number of reliable, sustainable public components to help companies reduce development costs.
- Released under the MIT license, developers can utilize CatLib in their open source projects, as well as commercial ones.
- CatLib is a lightweight framework; all components can be removed, so you can choose what is right for your project.
- Interface-oriented programming allows you to implement underlaying components without breaking other parts.

### Clone CatLib

> CatLib: [https://github.com/catlib/catlib](https://github.com/catlib/catlib)

First, clone the CatLib project from Github!

`git clone https://github.com/catlib/catlib.git`

After cloning the repository, you will find that there are folders in the `catlib` folder: `CatLib.Unity` and `CatLib.VS`, which correspond to the `CatLib Unity project` and `CatLib compilation project` respectively.

Your project development will start with `CatLib.Unity`, and` CatLib.VS` will be used to compile the CatLib framework into a `dll` file. In general, CatLib source code will be in the `CatLib.VS` directory.

When using Unity to open the `CatLib.Unity` project, first run a unit test first to make sure you downloaded the framework project correctly. Open the unit test window with `Window -> Test Runner`, then execute `Run All`.

Wait a moment for the tests to complete (the time will depend on the performance of your computer, but the unit tests generally take about 1 minute).

### First Time Use

CatLib is easy to use. If you have a good foundational understanding of C#, you can quickly read through the documentation of this guide (_in progress_).

### Links

#### **UI framework**

- [FairyGUI](http://www.fairygui.com/) The editor is easy to use, and the habits are consistent with the Adobe family of software, planning and art designers can easily get started. In the editor to combine a variety of complex UI components, as well as the design of animation for the UI, without writing any code. Can be a key to export to Unity, Starling, Egret, LayaAir, Flash and other mainstream applications and gaming platform.

#### **Hot update program**

- [ILRuntime](https://github.com/Ourpalm/ILRuntime) The project provides a fast, easy, and reliable IL runtime for a C # -based platform, such as Unity, that enables a hot update of code in a hardware environment that does not support JIT (such as iOS)
- [XLua](https://github.com/Tencent/xLua) For the Unity,. Net, Mono and other C # environment to increase Lua scripting capabilities, with xLua, these Lua code can be easily and C # call each other.