---
title: REMOVE handlers
permalink: /docs/remove-handlers
---
## REMOVE handlers

REMOVE handlers are responsible for removing a part of your data defined by operation's path.

While only a single handler can be registered on any given path handlers for all above paths can be registered within a single patcher.
All remove handlers are independent from each other. Each of them deals with a different section of the system and they cannot be combined.

The handler to be invoked is determined only operation's path.

 * **/tickets/{ticketId}/comments**

   This one should result in removal of all comments from the ticket with ID equal to the value of the "ticketId" path variable.

 * **/tickets/{ticketId}/comments/{commentId}**

   Would remove only a single comment identified by number {commentId} within the context of ticket number {ticketId}.

 * **/tickets/{ticketId}/comments/{commentId}/message**

   REMOVE is not really useful in the context of simple properties. If you wish to return the value of a property to its default state (an empty string) then it would be more descriptive to use a REPLACE or ADD operation.

 * **/tickets/{ticketId}**

   This is not a good idea even if it is a valid system requirement to delete an existing ticket. Such an operation should be handled through a separate HTTP DELETE request as it deals with a whole entity and not just its property.

### Method signature

To register a method as a handler for a particular REMOVE path it must match the following criteria.

 * public
 * non-static
 * does not return anything
 * each argument represents a single path variable

