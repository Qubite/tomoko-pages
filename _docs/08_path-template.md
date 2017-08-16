---
title: Path template
permalink: /docs/path-template
---
## Path template

A path template is to a path what a regular expression is to a string. A given handler can be register on all paths matching a single template. It will then be invoked when any such path is encountered.

A path template can contain three types of nodes.

* **/tickets**

   Static node accepting only the specified value.

* **/{ticketId}**

   Wildcard path variable named "ticketId". Will accept any value

* **/{ticketId:[0-9]+}**

   Path variable named "ticketId" accepting only values matching the "[0-9]+" regular expression. In this case only integers are accepted.

Here are some examples.

* **/tickets/12345/state**

   This template is static, it will only match the "/ticket/12345/state/" path.

* **/tickets/{ticketId}**

   There is a single node wildcard parameter named "ticketId". This template will match any two node long path starting with "/tickets/" including "/tickets/12345" and "/tickets/asdf".
Path "/tickets/1234/state" will not match this template. Multinode wildcard parameters are not supported.

* **/tickets/{ticketId:[0-9]+}**

   Same as before but the "ticketId" node must be an integer according to the "[0-9]+" regular expression.

* **/tickets/{ticketId:[0-9]+}/comments/{commentId:[0-9]+}**

   Template with two path variables named "ticketId" and "commentId" each accepting only integers.
