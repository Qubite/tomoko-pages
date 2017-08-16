---
title: Tomoko configuration
permalink: /docs/configuration
---
## Tomoko configuration

Some of Tomoko class factory methods require an instance of the TomokoConfiguration class. It holds all configuration settings.
Through these methods you can set up Tomoko to fit your use case.

TomokoConfiguration is immutable. Once you build it it cannot be modified.

### Value parsers

Right now the only configurable option is the list of registered value parsers. It controls Tomoko capability of converting different value tree implementations to the expected types.

For more information check the value parser [article](/docs/parsing-process).
