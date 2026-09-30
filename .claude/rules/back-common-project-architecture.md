---
description: Project general architecture patterns.
paths:
  - "src/Service/**"
---

# Architecture (backend)

Clean Architecture + CQRS (MediatR). Dependency direction: **Api → Logic → Domain**; Infrastructure implements Logic's repository interfaces. Domain depends on nothing.

## Request flow
Controller (`${PROJECT}BaseController`) → `_mediator.Send(command/query, cancellationToken)` → `ICommand<T>` / `IReadQuery<T>` → nested `Handler` → repository (`IRepository<T,TKey>`) → `SaveChangesAsync`. Entities raise domain events via `AddDomainEvent`.

## MediatR pipeline — provided by SharedKernel (do not re-register)
Order: 1) `IdempotencyBehavior` (only `IIdempotentCommand<T>`) → 2) `LoggingBehavior` → 3) `TransactionBehaviour`.
- `TransactionBehaviour` wraps **commands** in a DB transaction, retries with backoff (`[RetryPolicy]`), publishes audit-trail + outbox **after commit**.
- Skips `IReadQuery<T>`, `ICommandWithoutTransaction[<T>]`.
- ⇒ `IReadQuery<T>` for reads (no transaction); `ICommand<T>` for writes (transactional + audited).
- No validation behavior — validate inside handlers.

## DI bootstrap — `EndaliaBuilder` (SharedKernel.Api.Common)
`Startup.ConfigureServices` calls `services.AddEndaliaCore<${PROJECT}Context>(config, env, opt => …)` then chains `.AddEndalia*()`; local registrations live in `${PROJECT}ServicesExtensions.Add${PROJECT}ServicesAndRepositories(this EndaliaBuilder)`. Register repositories/services as **Scoped**. No AutoMapper/Mapster — map manually / via `Select`.

## Localization
`IStringLocalizer` is SharedKernel's `JsonStringLocalizer` (Scoped); all user-facing text via `_localizer[label]`.
