---
title: Path syntax
permalink: /docs/path
---
## Path syntax

Operation path indicates which resource and optionally which property should be updated. It is the same format as found in URL. Path is the part right after the port up to the query.

* **/tickets/12345/state**

   Points to the "state" property of a ticket with ID 1234. Contains three nodes.

* **/posts/how-to-catch-a-rabbit/content**

   Points to the "content" property of a post identified by slug "how-to-catch-a-rabbit". Also contains three nodes.

* **/books/978-1-56619-909-4/author/first-name**

   Points to the "author.firstName" property of a book identified by ISBN 978-1-56619-909-4. Contains four nodes.

* **/tickets/12345/comments/12**

   Points to a comment with ID 12 within the context of a ticket with ID 12345. Contains four nodes.

There are two legal paths with not so obvious meaning.

*  &nbsp;

   This is not a bug but an empty path. Zero-length path represents the root. There is no slash character therefore it contains zero nodes.

* **/**

   Just a slash character, points to a property with an empty name "". Contains a single node.
