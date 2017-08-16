---
title: Annotation-based handler registration
permalink: /docs/annotations
---
# Annotation-based handler registration

Tomoko provides a mechanism to configure a patcher in a way similar to how Spring MVC handles controller classes.

First you need to define a class. Within it you declare methods that are to be registered as handlers and getter methods linking to other configuration classes.
The next step involves instantiating that class and passing the created instance to `Tomoko.scanPatcher()` method. It will return a fully configured Patcher object that is ready to be used by your controllers.

Tomoko is not bound to any dependency injection container. It is completely unaware of that concept. Because of that you have to construct an instance of the configuration class yourself.
More often than not you will use a DI implementation of your choice to help you with that.

### Class-level configuration

The configuration class can be decorated with the `@PatcherConfiguration` annotation. While it is optional you can use it to define the common path for all handlers defined within.
Each handler in this class will be registered on a path constructed out of the class-level path and the path of the handler itself. You can treat it as a prefix.

### Handler configuration

Each handler method must be public, non-static and be declared to return `void`.
To mark a method as a handler you can use a generic `@PatcherHandler` annotation or its operation type-specific variants: `@AddHandler`, `@RemoveHandler` and `@ReplaceHandler`.

Operation type-specific variants contain exactly the same configuration options as the generic one except the operation type which is implicitly set.

`@PatcherHandler` accepts two parameters. One for setting the operation type (ADD, REMOVE, REPLACE) and one for specifying the path where this handler should be registered.

### Path variables

Method arguments can be used to receive values of path variables. All arguments (except the last one in ADD and REPLACE) will be treated as linked to a corresponding path variable of the same name.
If your code is compiled with -parameters flag then method parameter names are available and Tomoko will read them.
However if that is not the case or you want to link a parameter to a differently named path variable you can use the `@Parameter` annotation.

Parameters linked to path variables can be declared as String, int or long. In case of the latter types the string value extracted from the path will be automatically converted to the target type.

Tomoko supports URL encoded path variables. To use it you need to mark the proper method parameter with the `@UrlEncoded` annotation.

If there is a type you need but is not yet supported or is very specific to your use case then you can declare the parameter as String and convert its value as you require within the handler itself.

### "Value" parameter

Additionally ADD and REPLACE handlers must contain at least one parameter to represent the "value" property of a particular operation. In case there are multiple arguments then by convention the last one is considered to be the "value" argument.

The "value" parameter can be declared as any type you want. The only restriction is related to generics.
If the "value" parameter is declared as List<String> then through reflections it is possible in JDK 8+ to read value of type arguments as it resides in class metadata.
However if it is declared as List<T> then it might be impossible to determine the actual required type and so the registration will fail.

### Linking other configuration classes

Sets of handlers can be grouped together in smaller classes and linked together using the `@LinkedConfiguration` annotation.

This is a method-level annotation that should be used on a getter method accepting no parameters and returning an instance of the linked configuration.
`@LinkedConfiguration` accepts one optional parameter that will be treated as a path prefix for all handlers found within the linked configuration.

`@PatcherConfiguration` prefix is honoured as well so the final path on which a linked handler will be registered can be expressed with the following formula: parentConfigurationPrefix + linkPrefix + childConfigurationPrefix + handlerPath.

Configurations can be linked as deep as required but cycles are not allowed.

### Examples

For examples check the [tutorial](/tutorial/handlers.html).
