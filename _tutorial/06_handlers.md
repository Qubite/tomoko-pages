---
title: Patch handlers registration
permalink: /tutorial/handlers.html
---

## Patch handlers registration

Patch handler is a piece of code that is invoked when a corresponding patch operation is encountered. Basically it is your application code called by Tomoko when needed.

There are two ways to register handlers: with DSL or by using annotations and the class scanner. Here we will present the latter option. Explanation of the DSL part can be found in the [documentation]({{ site.baseurl }}/docs/dsl).

### Class scanner

To create a patcher based on the BookPatchSpecifcation class you just have to use the following code.
```java
JacksonTomoko tomoko = JacksonTomoko.instance();
BookPatchSpecification specification = new BookPatchSpecification(MemoryBookRepository.instance());
Patcher patcher = tomoko.scanPatcher(specification);
```
We are scanning an instance of a class, not the class itself as only instance methods can be registered as handlers. Static methods are excluded.

### Annotations

Handlers can be annotated in a way similar to Spring controllers. First we annotate BookPatchSpecification class with `@PatcherConfiguration`. Its main property is a path that will be prepended to all handlers within the annotated class. `@PatcherConfiguration` usage is optional.

```java
@PatcherConfiguration("/books/{bookId}")
public class BookPatchSpecification {

    private static final String BOOK_ID_PARAMETER = "bookId";

    private final BookRepository bookRepository;

    public BookPatchSpecification(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }
    /* handlers omitted */
}
```

/books/{bookId} represents a path with one static node matching only the "books" string and one wildcard parameter named "bookId" accepting any string up to the next '/' character.

### Add operation

Now it's time to annotate handlers.
The first annotated method will handle changes done to the title of a book identified by the value of the "bookId" path parameter. It will be registered on the `/books/{bookId}/title` path.
```java
@PatcherHandler(action = CommandType.ADD, path = "/title")
public void updateTitle(@Parameter(BOOK_ID_PARAMETER) String isbn, String newTitle) {
    bookRepository.getByISBN(isbn).setTitle(newTitle);
}
```
Handlers for ADD and REPLACE operations must accept at least one argument. Value associated with a given operation will be passed to the handler as the last argument. The rest of arguments will be treated as path parameters.

The 'isbn' argument is marked with the `@Parameter` annotation. It allows to specify the name of the path variable it is connected to.
If method parameter names are available (ie. code is compiled with -parameters flag) then the `@Parameter` annotation can be omitted if a particular argument is to be connected to a path variable with the same name.

A valid JSON patch request corresponding to this handler should look like this.

```json
[
  {
    "op": "add",
    "path": "/books/978-83-942488-1-9/title",
    "value": "Fight Club"
  }
]
```

The next method will handle updates to the description property.
```java
@AddHandler("/description")
public void updateDescription(@Parameter(BOOK_ID_PARAMETER) String isbn, String newDescription) {
    bookRepository.getByISBN(isbn).setDescription(newDescription);
}
```
It is almost the same as the first one except it is annotated by `@AddHandler`. It is just a specialized version of `@PatcherHandler` implicitly setting action to CommandType.ADD.
There are also such annotations for the rest of supported operation types: `@RemoveHandler` and `@ReplaceHandler`.

### Complex operation value

In this example you can see that the value parameter (the last one) does not have to be a primitive or a string. It can be any class that Jackson can take care of.
Here we want a handler for adding a new comment to a particular book. The operation corresponding to this handler must provide a value that can be parsed to an instance of the Comment class.

Why is a comment created through a PATCH request and not POST? In some cases it is valid to do so.
Comment does not have an ID of its own. It is completely owned by its parent so it is natural to PATCH a book instead of POSTing a comment.
It is just a tip. Tomoko can be used any way you want.

```java
@AddHandler("/comments/-")
public void addComment(@Parameter(BOOK_ID_PARAMETER) String isbn, Comment comment) {
    bookRepository.getByISBN(isbn).addComment(comment);
}
```

This handler will work well with a JSON operation defined as below.

```json
[
  {
    "op": "add",
    "path": "/books/978-83-942488-1-9/comments/-",
    "value": {
      "author": "John Doe",
      "content": "Great book!"
    }
  }
]
```

The '-' character indicates the end index of a collection according to RFC 6902. It is quite descriptive but not mandatory.
Handler on path /books/{bookId}/comments would work as good as this one. Just be sure to be consistent across your configuration and generated JSON patches.

### Remove operation

The final example shows a handler for a remove operation. This time the path is a bit more complicated. It contains a second path parameter called commentId that is restricted by a regular expression accepting only the numbers.
The commentId parameter of the removeComment method is an int. The string value of the path parameter "commentId" will be automatically converted to int if possible. Long is also supported.
```java
@RemoveHandler("/comments/{commentId:[0-9]+}")
public void removeComment(@Parameter(BOOK_ID_PARAMETER) String isbn, @Parameter("commentId") int commentId) {
    bookRepository.getByISBN(isbn).removeComment(commentId);
}
```
And an example of the patch request.
```json
[
  {
    "op": "remove",
    "path": "/books/978-83-942488-1-9/comments/0"
  }
]
```
