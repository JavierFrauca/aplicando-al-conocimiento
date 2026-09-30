---
description: SharedKernel exploration, when looking for implementation or specific behavior.
paths:
  - "src/Service/**"
---

# SharedKernel — local source code

When reading C# files that reference or use SharedKernel NuGet packages, the full source code is available at `C:\dev\SharedKernel`. Consult it to understand the actual behavior of its types, extensions, and abstractions instead of inferring it.

Always invoke the `back-common:sharedkernel` skill before investigating SharedKernel code — it gives the platform reference (what each `Endalia.SharedKernel.*` library is for, how to wire it up, and what to implement) before diving into the source.
