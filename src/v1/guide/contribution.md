---
title: Contribution
type: guide
order: 5
---

## Contribution

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/contribution.md)

This document describes how to contribute to the framework and contribute to the framework, components, documentation, bug fix.

### Defect report

CatLib strongly encourages the use of GitHub's pull requests (hereinafter referred to as PR) to provide defect reports. The Defect Report can also be submitted by a pull requests containing failed assertions.

If you submit a bug report in the form of a document, your question should contain a title and a clear description of the problem, and include as much information as possible and a code model that demonstrates the problem. The purpose of the defect report is to keep you It is easier for others and others to reproduce the defects and repair them.

### Development discussion

You can propose new features on the issue board or optimize existing functions. If you are new, please implement at least part of the code to complete the new function development.

### Pull Request

- do not make a larger content PR unless it is a brand new component
- each PR only do one thing
- Make sure that the PR code can be compiled
- When submitting PR, make sure that all tests for the code must pass
- When submitting PR, be sure to record the cause of PR
- Submit the PR, be sure to ensure that the unit test can cover as soon as possible (90% core components, 75% of conventional components)
- If the submitted PR can not complete the cover, please describe the reason.
- All PR unit tests must be run in VS (MSTest) and Unity (NUnit)
- Please write the code according to the code specification

### Submitted to the branch

- all bug fixes should be submitted to the latest stable branch, never submit bug fixes to the master branch, unless they can fix the next release version of the existing features.
- The new features that are fully backward compatible in the current version can also be submitted to the latest stable branch.
- Important new features are to be submitted to the master branch.
- If you are not sure whether it is an important feature or a secondary feature, consult in the calib.slack or QQ group.

### Frame designer

- 喵喵大人 - github：[yb199478](https://github.com/yb199478)

### Component contributor

> Virtual to be

### Documents contributor

> A:[alecnunn](https://github.com/alecnunn)

### Other contributors

- A:[AlianBlank](https://github.com/AlianBlank)
- D:[DawnKing](https://github.com/DawnKing)
- I:[idle](https://github.com/views63)
- L:[LrvingCong](https://github.com/LrvingCong)
- S:[silingfei](https://github.com/silingfei)
