---
layout: default
permalink: /download.html
title: Download
---
<div class="container-fluid">
  <div class="container tomoko-container" markdown="1">

## Dependency management

All artifacts related to Tomoko library can be found in the io.qubite.tomoko group on [Maven Central](https://search.maven.org/#search%7Cga%7C1%7Cio.qubite.tomoko).
Examples provided below are for the newest version of the Jackson variant. For Gson use the tomoko-gson artifact instead.

### Maven

```xml
<dependency>
    <groupId>io.qubite.tomoko</groupId>
    <artifactId>tomoko-jackson</artifactId>
    <version>{{ site.version }}</version>
</dependency>
```

### Gradle

```groovy
dependencies {
    compile 'io.qubite.tomoko:tomoko-jackson:{{ site.version }}'
}
```
---
## Manual download

All releases are available on Tomoko GitHub [repository](https://github.com/Qubite/tomoko/releases).

Remember Tomoko dependends on other libraries: net.jodah Typetools, CGLib, Objenesis and SLF4J. These have to be downloaded separately and placed on the classpath.

  </div>
</div>

