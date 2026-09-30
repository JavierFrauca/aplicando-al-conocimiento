---
description: Project layout and technology stack.
paths:
  - "src/Service/**"
---

# Project: ${PROJECT}

Endalia microservice (.NET) + Angular admin SPA.

## Layout
- `src/Service/${PROJECT}.Api` — controllers, startup, API models.
- `src/Service/${PROJECT}.Logic` — CQRS commands/queries/handlers, repository interfaces, DTOs, services.
- `src/Service/${PROJECT}.Domain` — entities, enums, domain events, `${PROJECT}DomainException`.
- `src/Service/${PROJECT}.Infrastructure` — `${PROJECT}Context`, EF configs, repositories, migrations.
- `src/Service/Test` — unit tests (xUnit) + acceptance tests (Reqnroll/Playwright).
- `src/Web Apps/${PROJECT}.Management/ClientApp` — Angular SPA (separate conventions).

## Stack & governance
- .NET 10. 
- TFM set per-`.csproj`.
- `Nullable=enable`, `ImplicitUsings=enable` and `LangVersion=latest` are defined in `Directory.Build.props` (repo root). Individual `.csproj` files do not repeat them unless there is an explicit override. If they appear in neither `Directory.Build.props` nor the `.csproj`, assume they are disabled by default (repo policy).
- `.editorconfig` (Endalia-governed) + analyzers are the source of truth for formatting — don't hand-edit. Local opt-out: gitignored `Directory.Build.props.user`.
- `DateTime.Now` is banned except for timestamp on generated files (names) → always `DateTime.UtcNow`. SQL Server; EF schema `${PROJECT}`.

## SharedKernel
`Endalia.SharedKernel.*` from the private Azure DevOps feed. Do not modify SharedKernel; extend in the Logic layer.

## Tooling
- MCP: `ado`, `codegraph` (`.codegraph/`), `LSP` (Roslyn), `engram`.
