---
title: Handler precedence
permalink: /docs/path-ordering
---
## Handler precedence

Path templates and handlers are structured as a tree. Each operation type has its own handler tree.

When a patcher resolves a path it is checked against registered path templates node by node in a given tree. Resolver tries to match the first node of a path to the first node of a path template.
When it succeeds it tries to match the rest of the path within the subtree represented by that path template node. Only the first matching path template node is taken into account.

### Path template node ordering

To avoid wilcard nodes being too greedy sibling nodes are ordered as follows:

* **Static nodes**

   They are the first to be matched. Within their group they are ordered alphabetically but it does not affect anything as at most one of them will match a path node.

* **Regex nodes**

   Parameters with regular expression defined are the second group where resolver tries to find a match. Within that group they are ordered exactly as they were added as there is no easy way to compare regular expressions.

* **Wildcard nodes**

   There can only be a single node of this type within sibling nodes and it is the last node to be checked as it accepts all values.

### How it works

This example is valid for any operation type.

Let's suppose we want to handle five path templates that form the following tree. Each row represents one path template and handler.

Collection node | ID node | Property node | Handler name
--- | --- | ---
/tickets | /12345 |  | A
 | ---> | /title | B
---> | /{ticketId} |  | C
 | ---> | /title | D
 | ---> | /description | E

<br/>
What will happen if a REMOVE operation with one of the following path is to be executed?

* **/tickets/9090**

   First resolver will find the static node /tickets. Then it will try to match the /9090 path node with the static template node /12345.
   It fails and so it moves to the second available node /{ticketId} which accepts any value. C handler is the winner.

* **/tickets/12345**

   Static node /12345 has higher priority than /{ticketId} so for sure handler A will be called as a result.

* **/tickets/12345/title**

   No surprises here. B handler

* **/tickets/12345/description**

   A HandlerNotFoundException will be thrown! /description is a child of the /{ticketId} node but that is not the case for the /12345 node.
   When /12345 is matched then only its subtree is taken in account for the rest of the process. Sibling subtrees are discarded.

### Mixing general and specific handlers

The last example threw an exception. Nonetheless this is a valid scenario with handlers valid for almost all cases and a special one we want to handle separately.
What can be done in such situation?

It's simple. Just make the special case a part of your general handler. A single "if" will do the job.

The first matching node rule is implemented by design. Other approaches exist but the path resolution logic becomes complex and still does not cover all cases.
It is especially cumbersome within the context of ADD operations where multiple handlers can be invoked to execute a single operation.
