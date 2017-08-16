---
title: Patch JSON format
permalink: /tutorial/patch-format.html
---
## Patch JSON format

Tomoko does not introduce its own JSON patch operation format but instead uses [RFC 6902](https://tools.ietf.org/html/rfc6902).

RFC 6902 defines operations changing the structure of a JSON document. It contains six different operation types however Tomoko only supports three of them as the rest is not as meaningful in the context of Java objects.

Body of an HTTP PATCH request can look like this.

```json
[
  {
    "op": "add",
    "path": "/tickets/1234/title",
    "value": "New ticket title"
  }, {
    "op": "remove",
    "path": "/tickets/1234/comments/12"
  }, {
    "op": "replace",
    "path": "/tickets/1234/description",
    "value": "Updated description"
  }
]
```
Multiple operations can be defined in a single patch.
Each operation must specify its type and a path. Type must contain one of the following values: "add", "remove" or "replace". Path is similar to the URL format but there is no protocol, no host and no query string.

ADD and REPLACE operations require the "value" property. REMOVE operation on the other hand does not accept value.
The "value" property can be as complex as necessary. It can be an object with multiple properties or an array of strings. Any valid JSON element can be placed there. It will be later converted to a Java object and consumed by a handler.

### Combining ADD operations

Let's suppose there are registered handlers on paths: "/tickets/1234/title" and "/tickets/1234/description".

These three JSON patches would end up calling the same handlers and the end result would be the same.

Separate ADD operations version.
```json
[
  {
    "op": "add",
    "path": "/tickets/1234/title",
    "value": "New ticket title"
  }, {
    "op": "add",
    "path": "/tickets/1234/description",
    "value": "Updated description"
  }
]
```
Combined ADD operations version.
```json
[
  {
    "op": "add",
    "path": "/tickets/1234",
    "value": {
      "title": "New ticket title",
      "description": "Updated description"
    }
  }
]
```
Combined operations with path fully included in the "value" property.
```json
[
  {
    "op": "add",
    "path": "",
    "value": {
      "tickets": {
        "1234": {
          "title": "New ticket title",
          "description": "Updated description"
        }
      }
    }
  }
]
```

These three variants are equivalent in this scenario.

REPLACE and REMOVE operations cannot be combined in such a way.