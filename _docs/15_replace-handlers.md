---
title: REPLACE handlers
permalink: /docs/replace-handlers
---
## REPLACE handlers

REPLACE handlers are responsible for setting a new value at the target specified by the path. It should behave as if a REMOVE operation was executed first followed by an ADD operation on the same path. "Value" property is mandatory.

While only a single handler can be registered on any given path handlers for all above paths can be registered within a single patcher.
All REPLACE handlers are independent from each other. Each of them deals with a different section of the system and they cannot be combined.

The handler to be invoked is determined only operation's path.

 * **/tickets/{ticketId}/comments**

   This one should remove all comments from the ticket with ID defined in the "ticketId" path variable and insert all comments defined in the "value" property. "Value" property is expected to be an array.

 * **/tickets/{ticketId}/comments/{commentId}**

   Would remove only a single comment identified by "commentId" path variable within the context of ticket identified by path variable "ticketId" and replace it with a comment found in the "value" property.

 * **/tickets/{ticketId}/comments/{commentId}/message**

   For a simple property REPLACE operation is semantically equivalent to an ADD operation on the same path.

 * **/tickets/{ticketId}**

   As in the case of REMOVE this is not a good idea. If such functionality is required then a separate PUT request should be defined as ticket is an entity and not just a complex property.

### Method signature

To register a method as a handler for a particular REPLACE path it must match the following criteria.

 * public
 * non-static
 * does not return anything
 * contains at least a single argument
 * the last argument represents the "value" property of a particular REPLACE operation
 * each of the remaining arguments represents a single path variable