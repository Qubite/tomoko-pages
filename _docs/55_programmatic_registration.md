---
title: Programmatic handler registration
permalink: /docs/dsl
---
## Programmatic handler registration

Instead of relying on annotations and reflections you can register handlers programmatically with the HandlerConfigurationDSL class.
This class and all related are wrappers for a builder. For each patcher you have to construct a new instance through a facade.

First you define a path using the same syntax as in annotations. The `path(String path)` method returns a PathDSL instance which can be configured further.
When the path is fully specified you can pass a reference to a handler or any other lambda expression with `handle*()` methods. Once this is done DSL moves to the parameter configuration phase.

When you reference a method then in fact you create a new lambda expression wrapping the target method invocation, not the method itself.
Because of that some of type metadata is unavailable. To counter that the details (name, type) of parameters can be specified explicitly through methods `firstParameter()`, `secondParameter()` etc.

Same goes for the "value" parameter. Tomoko will try to determine that parameter's type but it is impossible to do so you can always define it directly through the `value()` method.
This is especially useful when working with generics (type erasure is not Tomoko's friend) or testing with mocks when even less metadata is present.

The last step involves actually registering the handler with the `register()` method.

After all handlers have been registered the initial DSL instance can be used to create the patcher.

### Descriptors

`register()` method call returns a type-safe representation of the registered handler. These are of the same type as in annotation-based approach described [here](/docs/descriptors).

