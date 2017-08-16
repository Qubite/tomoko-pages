---
title: ADD handlers
permalink: /docs/add-handlers
---
## ADD handlers

ADD handlers are responsible for inserting new objects to collections and setting new values for properties both simple and complex. "Value" property is mandatory for ADD operations.

Each handler must be registered in a collision-free way. There must not exist two handlers such that one's path is an extension of another's.
Handlers cannot be registered both on /tickets/{ticketId}/comments/{commentId} and /tickets/{ticketId}/comments/{commentId}/message paths as the second one is a direct extension of the first one.
In other words all handlers must be registered on leaf path nodes.

Thanks to that restriction multiple ADD handlers can be invoked as a response to a single ADD operation depending on the structure of the "value" tree and registered handlers.

To make the most out of ADD handler combination these handlers should be as fine-grained as possible.

 * **/tickets/{ticketId}/title**

   Sets the new title of the ticket identified by the "ticketId" path variable.

 * **/tickets/{ticketId}/description**

   Same as above but for the description property.

 * **/tickets/{ticketId}/priority**

   Same as above but for the priority property.

 * **/tickets/{ticketId}/comments/{commentId}/message**

   Sets a new message for the comment with ID {commentId} in the context of ticket with ID {ticketId}.

 * **/tickets/{ticketId}/comments/{commentId}/author/first-name**

   Assuming "author" is a value object this handler sets a new first name for the author of the comment with ID {commentId} in the context of ticket with ID {ticketId}.

 * **/tickets/{ticketId}/comments/-**

   Add a new comment to the comments collection as defined in the "value" property. "/-" indicates the end index of the collection. Thanks to that it does not collide with other comment handlers as it would without the "/-" path node.

   Id of the newly created comment is not returned to the caller in any way.
   If you need it then you probably should use a separate POST request to create a new comment as it is an entity with an identity of its own and not just a complex property indexed within the context of a parent object.

### Combining ADD handlers

All handlers defined above can coexist within a single patcher as each of them deals with a leaf path node.

Because of this to update both the title and the description of the same ticket within a single patch you don't need to defined two separate operations. Path can be partially defined in the "value" tree.

Given a handler configuration as defined above the following JSON Patch can be issued to update title and description.

```json
[
  {
    "op": "add",
    "path": "/tickets/12/title",
    "value": "Database does not work"
  }, {
    "op": "add",
    "path": "/tickets/12/description",
    "value": "Just fix it"
  }
]
```

Or we can use a combined version.

```json
[
  {
    "op": "add",
    "path": "/tickets/12",
    "value": {
      "title": "Database does not work",
      "description": "Just fix it"
    }
  }
]
```

This is a valid request that will be successfully executed. There is no handler registered on the "/tickets/{ticketId}" path but there are handlers registered deeper on "~/title" and "~/description" paths.

Tomoko will start with the operation's path and traverse the "value" tree to find the matching handlers.

### Method signature

To register a method as a handler for a particular ADD path it must match the following criteria.

 * public
 * non-static
 * does not return anything
 * contains at least a single argument
 * the last argument represents the "value" property of a particular ADD operation
 * each of the remaining arguments represents a single path variable