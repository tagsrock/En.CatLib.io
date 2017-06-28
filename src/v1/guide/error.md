---
title: Common mistakes
type: guide
order: 3
---

## Common mistakes

> This article is translated by machine , I would like to participate in [manual translation](https://github.com/catlib/en.catlib.io/blob/master/src/v1/guide/error.md)

This document describes the bugs that developers often encounter to help developers solve problems quickly.

### Dependent injection

- Attribute dependency injection is always performed after the constructor dependency injection. Please do not use features in the constructor to rely on the injected object.

### routing

- The host matching block for uri defined in the routing system `Route()` does not allow variable matching. That is: `ui://{myname}/hello/world/1/` (in the host can not use {myname} do variable matching, this is a wrong example)