---
title: Type-safe handler descriptors
permalink: /docs/descriptors
---
## Type-safe handler descriptors

Each descriptor holds information about a single handler including the path template where handler is registered, path variables and handler's parameters.
They can be used to generate paths and whole operations in a type-safe manner for a particular handler.

Normally the JSON document is constructed first and then parsed to a Patch instance. The next step involves converting string path variables to types accepted by a handler.
With descriptors the process is reversed. You pass in values accepted by a particular handler and as result an operation is generated.
It is worth mentioning the DirectTree implementation of the ValueTree interface is used underneath.

When handler accepts all path variables as parameters the path is constructed only using the path template and the values of handler's parameters.
If that is not the case Tomoko will automatically generate values for not bound path variables. It is easy when dealing with wildcard parameters as they accept any value.
However for regex parameters you have to define it yourself and pass in the parameter map.

### Annotation-based

To retrieve descriptors for handlers registered with class scanning first you have to create a class descriptor for the root configuration class with `Tomoko.descriptorFor(Class<T> clazz)`.

Class descriptor `*Handler()` method family accepts lambda expressions representing a particular handler you need descriptor for.
It is important to note these methods accept function references in a static way. They must be bound to the class itself, not to its instance.

To retrieve a handler from a linked configuration you have use the `linked()` method which will return the linked configuration class descriptor.
`linked()` method accepts only getter methods decorated with `@LinkedConfiguration`.

