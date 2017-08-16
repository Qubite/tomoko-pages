---
title: Patch JSON format
permalink: /docs/patch-format
---
## Patch JSON format

Tomoko is designed to handle HTTP PATCH requests specified in the RFC 6902 format.
The official [RFC 6902 specification](https://tools.ietf.org/html/rfc6902) is a good explanation of the format itself.
This article focuses describing the format within the context of Java objects.

The root node is an array. That array contains operations and represents the whole patch.

Each operation must contain "op" and "path" properties. "Value" property is mandatory only for particular operation types.

RFC 6902 defines six different operation types but Tomoko supports only three of them.

* ADD

   Adds "value" to the target collection or sets "value" at the target path if the target is a simple property. "Value" property is mandatory. Multiple ADD handlers can be called as a result.

* REMOVE

   Removes the value found at the target path. In case of an collection it can clear it or remove only a single element. "Value" property absent. Always a single handler is called.

* REPLACE

   A REMOVE operation followed by an ADD one on the same path. In a sense it is equivalent to the HTTP PUT method. "Value" property is mandatory. Always a single handler is called.

For examples check the [tutorial](/tutorial/patch-format.html).

### Other RFC 6902 operations

MOVE and COPY operations are not supported currently by Tomoko. They work great in the context of JSON documents but they are not really that meaningful in the context of Java objects.
Java entities and properties are not usually moved around or copied.

If these operation types are needed in your project feel free to create an issue on GitHub.
