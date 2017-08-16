---
title: What's the problem?
permalink: /tutorial/why.html
---
## What's the problem?

So you develop a REST API for your application. You have GETs retrieving data, DELETEs removing resources from the system and POSTs creating them. But you also have to support data updates.

Suppose there is a class named Bag containing multiple properties: id, property01, property02, property03 and so on up to property10. These properties are not related to each other in any way other than being a part of the same class. Updates done to them are completely independent.

You want to allow external clients to make changes to values of these properties except the id.

### Separate handcrafted PUT requests for each property

At first you might be tempted to handle each property completely separately. In your controller you create ten methods and make them handle PUT requests on paths "/api/bags/{id}/property01", "/api/bags/{id}/property02", "/api/bags/{id}/property03" and so on.

That's a lot of boilerplate code. What's more if an external client would want to update seven properties at once it would have to send seven HTTP requests each with its own network overhead. That is not a satisfying solution.

### Single handcrafted PATCH request for the whole class

So let's create a single controller method for updating all properties in one PATCH request on path "/api/bags/{id}". It expects a JSON document like this one.

```json
{
  "propertyA": "New value for propertyA",
  "propertyE": 5
}
```
It is a map where each entry contains the name of the property to be updated and its new value. If property name does not appear in the request then it should not be modified in any way.

Great. Now any update within a particular Bag instance can be done in just a single request.

### Tomoko

But that is not the end of the story. We can improve on that idea. There are still some problems we would like see solve.

* It is impossible to update multiple Bag instances within a single request.
* Property does not have to be a simple string or an integer. What if it is a complex object containing inner properties? How do we address them?
* Only updates assigning a new value to a property are supported. What if we want to add or remove a phone number from a list? Phone number is just a value object so a separate DELETE request is an overkill.

Tomoko takes care of all these issues by expecting incoming JSON PATCH request to be compliant with RFC 6902.
On top on that Tomoko provides you with a set of tools for handling such requests in the Java context including request validation, programmatic and annotation-based handler configuration and automatic type conversion of path variables.
All tools are available through a convenient facade.

It is DI and web framework agnostic so it can be used in any environment.