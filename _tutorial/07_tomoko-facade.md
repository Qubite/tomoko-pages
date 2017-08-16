---
title: Tomoko facade
permalink: /tutorial/tomoko-facade.html
---
## Tomoko facade

Once configured JacksonTomoko acts as a root able to create all related objects.

* `patchParser()`

   Returns an instance of PatchParser that is able to parse incoming patch requests using Jackson ObjectMapper.

* `scanPatcher()`

   Scans the specification class instance and return a fully configured patcher.

* `descriptorFor(Class<?> specificationClass)`

   Returns descriptor class instance that can be used to create operations corresponding to registered handlers in a type-safe way.

* `specificationDsl()`

   Gives access to DSL classes to register handlers programmatically.

As long as you are using JacksonTomoko as the entry point instead of instantiating object yourself you should see as few API breaking changes as possible.

Remember that JacksonTomoko is immutable. Once you create it using any of the factory methods it can no longer be modified in any way.
