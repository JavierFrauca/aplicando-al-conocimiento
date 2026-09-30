---
description: C# style choices NOT already enforced by .editorconfig/analyzers.
paths: 
  - "src/Service/**/*.cs"
---

# Code style

`.editorconfig` (Endalia-governed) + analyzers are the source of truth for indentation, CRLF, UTF-8 BOM, file-scoped namespaces, and diagnostic severities — don't restate or fight them. Beyond that:
- Use **expression-bodied members** for simple properties/accessors.
- Prefer `var` when the type is obvious from the right-hand side.
- Initialize non-nullable members explicitly: `= null!;` or `= string.Empty;`.
- Shared domain references go in each project's `GlobalUsings.cs`; file-specific usings at the top.
