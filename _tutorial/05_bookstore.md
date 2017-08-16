---
title: Bookstore application
permalink: /tutorial/bookstore.html
---

## Bookstore application

In order to make this tutorial not so abstract we will base it on an example application called "Bookstore".

### Model

The main model class is show below. It is identified by the ISBN number. For simplicity author is a value object, it does not have a unique id. Same for comments. They are identified only by their index on the list.

```java
public class Book {

    private String isbn;
    private String title;
    private Author author;
    private String description;
    private List<Comment> comments;

    /* constructors, getters, setters etc. omitted */

}
```
For simplicity author is a value object, it does not have a unique id. It contains only the first name and the last name.

```java
public class Author {

    private String firstName;
    private String lastName;

    /* constructors, getters, setters etc. omitted */

}
```
Same goes for comments. They do not have ID of their own. They are identified only by their index on the list. The empty constructor is important as it allows Jackson to create Comment instances by itself.
Normally you use Jackson to transform DTO classes.

```java
public class Comment {

    private String author;
    private String content;

    Comment() {
    }

    /* getters, setters etc. omitted */

}
```

### Controller

Tomoko is usually used directly in a controller handling HTTP PATCH requests. While the BookController class is not tied to any web framework you can get the general idea how Tomoko should be woven into your architecture.

```java
public class BookController {

    private final PatchParser parser;
    private final Patcher bookPatcher;

    public BookController(PatchParser parser, Patcher bookPatcher) {
        this.parser = parser;
        this.bookPatcher = bookPatcher;
    }

    public void processPatchRequest(String requestBody) {
        Patch patch = parser.parse(requestBody);
        bookPatcher.execute(patch);
    }

}
```
PatchParser is an utility class provided by Tomoko to parse a JSON string to a Patch class instance that later will be handled by a patcher.

The bookPatcher field holds an instance of the Patcher class that does all the heavy lifting. It contains all registered handlers and the Tomoko mechanism for matching these handlers with incoming patch operations.

Let's find out now how to configure and create a patcher that will handle updates to our model.