---
description: Domain layer modeling — entities, enums, events, and persistence attributes.
paths: 
  - "src/Service/*.Domain/**"
---

# Domain modeling

- Entities inherit `BaseEntity<TId>` / `BaseEntityWithCreation<TId>` (SharedKernel.Api.Common.Data.Entities).
- Navigation collections: `private readonly HashSet<T> _items = []; public ICollection<T> Items { get => _items; init => _items = [.. value]; }`.
- Nested `public struct Constants { public const string LABEL_BASE = "${PROJECT}.<Entity>"; }` for localization label bases.
- Enums: XML `<summary>` on every value.
- Domain events implement `INotification`/`INotificationPreviousSave`, raised via `AddDomainEvent(...)`.
- **Lifecycle events** — an aggregate that must emit events on CRUD implements the SharedKernel markers and raises concrete domain events *inside* each method (never raise events from the handler directly — go through the entity):
  - `IEntityWithCreateEvent` → `void AddCreateEvent()`
  - `IEntityWithEditEvent` → `void AddEditEvent(List<ChangePropertyDto> changedProperties)` — iterate the changed properties, `continue` when `Equals(OldValue, NewValue)`, and raise only for the properties that matter.
  - `IEntityWithDeleteEvent` → `void AddDeleteEvent()`
  You implement these on the entity; `EndaliaDbContext` invokes them automatically during `SaveChangesAsync` (per the change-tracker: insert/update/delete) and the pipeline dispatches the raised events — **don't call them from the handler**.
- **Class-level attributes**: `[Table("Name", Schema = ${PROJECT}Constants.Schema)]`; `[Index(nameof(A), nameof(B), IsUnique = true)]` for DB indexes / unique constraints; `[EnabledAudit]` to turn on audit-trail tracking.
