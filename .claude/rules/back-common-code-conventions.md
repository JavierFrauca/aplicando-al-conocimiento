---
description: General backend C# idioms — naming, file organization, async, DI, errors, DTOs, documentation.
paths:
  - "src/Service/**/*.cs"
---

# Code conventions (backend C#)

## Naming
- Classes/methods/properties/constants `PascalCase`; locals/params `camelCase`; private fields `_camelCase`.
- File-scoped namespaces matching folders: `namespace ${PROJECT}.Logic.Commands.Salary;`.

## File organization (one type per file, grouped by `{Domain}` subfolder)
- Commands:        `${PROJECT}.Logic/Commands/{Domain}/{Name}Command.cs`
- Queries:         `${PROJECT}.Logic/Queries/{Domain}/{Name}Query.cs`
- Repo interfaces: `${PROJECT}.Logic/Repository/{Domain}/I{Name}Repository.cs`
- Controllers:     `${PROJECT}.Api/Controllers/{Domain}/{Name}Controller.cs`
- Domain models:   `${PROJECT}.Domain/Models/{Domain}/{Name}.cs`
- Repo impls:      `${PROJECT}.Infrastructure/Data/Repository/{Domain}/{Name}Repository.cs`
- EF configs:      `${PROJECT}.Infrastructure/Data/EntityTypeConfiguration/…`

## Async
- Public async methods take `CancellationToken cancellationToken` (full word) and propagate it as a named arg to every async call. No `return await Task.FromResult(...)`.

## Dependency injection
- Constructor-inject all dependencies as their **interfaces** (`IXxx`), into `_camelCase` `readonly` fields. No property/field/service-locator injection.

## Errors & validation (validate in-handler; no FluentValidation)
- Preconditions: `Ensure.That<${PROJECT}DomainException>(cond, _localizer[Label]);` (or `Ensure.Not<…>(badCond, …)` if it improves readability).
- Not-found: `var e = await _repo.FirstOrDefaultAsync(...) ?? throw new ${PROJECT}DomainException(_localizer[Label]);`
- `${PROJECT}DomainException` for business rules; `ForbiddenDomainException` for authorization.

## DTOs / data shapes
- **Reusable shapes** (shared by several commands/queries/services) are standalone types — `public sealed record` when possible — with `required` and/or `get; init;` properties (`get; set;` only when mutation is needed). **Document every property** with `/// <summary>`.
  - Suffix is contextual: `Dto`, `Projection`, or none — pick what reads best; don't force one.
  - Don't create a standalone shape for something used by a single command/query — use its nested `Request`/`Response` instead (see `back-common-cqrs-mediatr`).

## Domain & controllers
- Domain-layer modeling (entities, events, persistence attributes) → see `back-common-domain-modeling`.
- API controllers (base/inbound/outbound, routing, auth, responses) → see `back-common-controllers`.

## Documentation
- Comment non-obvious **business logic** (the *why*); skip narrating self-evident code. Public API surface uses XML `///`.
