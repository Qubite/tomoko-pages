---
title: Project setup
permalink: /tutorial/setup.html
---

## Project setup

First thing that is to be done is adding Tomoko as a dependency to the project. For Maven the dependency entry will look like this.

```xml
<dependency>
    <groupId>io.qubite.tomoko</groupId>
    <artifactId>tomoko-jackson</artifactId>
    <version>{{ site.version }}</version>
</dependency>
```

For other ways of acquiring the necessary artifacts check the [download page](/download.html).