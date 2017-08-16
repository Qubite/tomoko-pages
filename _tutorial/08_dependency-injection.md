---
title: Dependency injection
permalink: /tutorial/dependency-injection.html
---
## Dependency injection

Tomoko is not tied to any dependency injection container. JacksonTomoko class contains static factory methods to create its instances based on the provided configuration.
An instance of JacksonTomoko in turn can instantiate all other important elements. This works well both in non-DI and DI environment.

### Guice

To integrate Tomoko with Guice you can use the following code.

```java
public class TomokoModule extends AbstractModule {

    @Override
    protected void configure() {
    }

    @Provides
    @Singleton
    JacksonTomoko providesTomoko() {
        return JacksonTomoko.instance();
    }

    @Provides
    @Singleton
    PatchParser providesPatchParser(JacksonTomoko tomoko) {
        return tomoko.patchParser();
    }

    @Provides
    @Singleton
    Patcher providesBookPatcher(JacksonTomoko tomoko, BookPatchSpecification bookPatchSpecification) {
        return tomoko.scanPatcher(bookPatchSpecification);
    }

    /* omitted providers related to BookPatchSpecification */
}
```
That's it! Now you can inject these objects into your controllers and application services to parse patches and handle them.

Protip: the @Named annotation is useful when you need to create and inject multiple different patchers.