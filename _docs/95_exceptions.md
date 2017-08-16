---
title: Exceptions
permalink: /docs/exceptions
---
## Exceptions

In Tomoko there are multiple exception types to accurately describe the encountered problem.

* **TomokoException**

   All other exceptions are TomokoException subclasses. Thrown only in extreme cases. You most probably will not ever see this one but it can be used in a catch all clause.

* **PatchParseException**

   Could not parse the provided patch as it does not follow the RFC 6902 format.

* **ConverterException**

   Path variable or operation's value could not be converted to the type expected by a handler.

* **ConfigurationException**

   Invalid handler registration, no value parser registered etc.

* **InvalidOperationException**

   Operation was parsed properly but could not be processed by a patcher i.e. no handler was found for the requested path.

* **HandlerException**

   All problems encountered within the handler but before passing control to the client code.

### Client code exceptions

There is one more exception that can be thrown and it deals with exceptions thrown by registered handlers.

If client code throws an instance of Error or RuntimeException then the same instance is rethrown right away. However if a checked exception is encountered then it is wrapped in a HandlerExecutionException and thrown.
It is guaranteed the cause of a HandlerExecutionException will be an exception thrown by your code.
