---
title: Request parsing process
permalink: /docs/parsing-process
---
## Request parsing process

A patch request is parsed in two phases. One is performed after an HTTP request has been received to convert it to the form expected by a Patcher instance.
The second one is executed for ADD and REPLACE operations when a registered handler is to be invoked. REMOVE operation does not contain the "value" property so there is nothing to parse.

### Patch parser

First step involves checking if it does conform to the RFC 6902 format. The end result is a list of operations each described by a type, path and a value in some cases.

If the request is not valid then a PatchParseException is thrown.

At this point operations are validated and parsed but handlers are yet to be found and the type expected by the corresponding handler is unknown so the "value" property is converted to an implementation of the ValueTree interface.
tomoko-jackson PatchParser produces JacksonTree objects and Gson variant creates GsonTree objects.

JacksonTree really is just a wrapper for a Jackson JsonNode object that makes unified tree traversal possible which is required for ADD operations. GsonTree holds inside a JsonElement.
Unfortunately there is no JSON API within JDK so we have to deal with a multitude of implementations.

There is also the DirectTree class for programmatic creation of value trees based on Java objects bypassing the string form. It is composed of string path nodes and normal Java objects on leafs i.e. List, String, Integer and so on.

### Value tree parser

After a handler has been found for a particular operation it is known what Java type that handler expects. ValueTreeParser is responsible for converting the provided ValueTree to that type.
JacksonTreeParser uses ObjectMapper while GsonTreeParser uses Gson. DirectTreeParser does not have to perform any transformations.

If for any reason the requested conversion cannot be performed a ConverterException is thrown with the original Jackson/Gson exception as the cause.

### Patcher and multiple patch sources

Each ValueTreeParser implementation is able to parse only a single ValueTree implementation.
If your Patcher is to receive Patch objects from different sources parsed with different serialization libraries then you have to use TomokoConfiguration to register all required parsers.

To accept patches parsed by Jackson and created programmatically you can use the following code.

```java
TomokoConfiguration configuration = TomokoConfiguration.builder().registerValueTreeParser(JacksonTreeParser.instance())
    .registerValueTreeParser(GsonTreeParser.instance()).build();
Tomoko tomoko = Tomoko.instance(configuration);
Patcher multiSourcePatcher = tomoko.scanPatcher(handlerSpecification);
```

### Support for other serialization libraries

While only Jackson and Gson are supported out of the box it is possible to implement your own patch parser and value tree parser. It is advisable to follow tomoko-jackson and tomoko-gson subprojects design.

Not only other JSON libraries can be supported this way but YAML and Protocol Buffers as well and any other serialization framework for that matter.