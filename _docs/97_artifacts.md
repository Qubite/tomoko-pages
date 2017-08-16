---
title: Artifacts
permalink: /docs/artifacts
---
## Artifacts

All artifacts related to Tomoko library can be found in the **io.qubite.tomoko** group.

* **tomoko**

   The core artifact. It contains majority of the code including the main mechanism.

* **tomoko-jackson**

   Jackson-oriented Tomoko variant. Offers a specialized facade for quick setup with Jackson as the main parser. It explicitly depends on the tomoko artifact and the Jackson library.

* **tomoko-gson**

   Plays essentially the same role as tomoko-jackson. The only difference is that it uses Gson instead of Jackson to parse requests.

* **tomoko-parent**

   Container for all submodules. Contains only the Maven configuration.
