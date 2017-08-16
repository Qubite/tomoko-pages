---
title: Testing
permalink: /docs/testing
---
## Testing

In order to test the previously defined Patcher instance you have to decide if you want to test the whole process together with patch parsing or just the patcher itself.

The whole process involves preparing a JSON document with a patch defined within, parsing it and passing the result to a patcher.
Patch JSON can be created either manually or by instantiating a Java object and converting it to JSON using one of the serialization libraries.

You can also test only the patcher itself by skipping the deserialization/serialization part with the help of DirectTomoko, DirectTree and PatchBuilder classes.

### DirectTomoko

The io.qubite.tomoko.direct package was designed to allow testing the core library without dependency on any serialization-specific submodule.

DirectTomoko is a facade class similar to JacksonTomoko and GsonTomoko but made specially for the DirectTree implementation of the ValueTree interface.

With DirectTree and PatchBuilder you can easily create operations with mandatory "value" property by building the "value" out of normal Java objects.
DirectTree is just a tree of path nodes and leaf nodes containing Java objects expected by the target handler.

DirectTreeValueParser does not really parse anything. It just retrieves objects added to the value tree as they are.
This format is less flexible that JSON but also less cumbersome when it comes to testing.

### Type-safe testing

You can also use descriptors together with the PatchBuilder to test particular handlers in a type-safe manner without memorizing exact paths.

For more information check the [descriptors article](/docs/descriptors).