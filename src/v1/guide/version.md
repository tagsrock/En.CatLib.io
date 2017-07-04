---
title: Release Notes
type: guide
order: 2
---

## Release Notes

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/version.md)

　
> CatLib version of the standard is used: [Semver semantic version of the standard](http://semver.org/lang/zh-CN/)

### V0.8.3 Beta

- Fixed a problem with `UnitySettingLocator` could not be properly stored and accessed
- Fixed a problem with `Application.Version` inconsistencies

### V0.8.2 Beta

- Fixed the bug that was caused by configuration errors due to the `FileSystemManager` facade model

### V0.8.1 Beta

- Fixed the problem of inconsistent version names
- Fixed the missing registered file service provider
- Fixed a bug, this bug will lead to the correct path in `/` and `\` mixed positioning
- Fixed a bug, this bug results in a default path that cannot be returned without a given `AssetPath`

### V0.8.0 Beta

> This release is pre-release for BetaLib.

- Added CatLib core
- Added standard library - container
- Add a standard library - an ordered set
- Add a standard library - Quick list
- Add a standard library - least use caching
- Add standard library - filter chain
- Add a new routing component
- Added file system
- Add configuration components
- Add time component
- Add a timer component
