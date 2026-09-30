---
description: Entity Framework Core — DbContext, configurations, repositories, queries, migrations.
paths: 
  - "src/Service/*.Infrastructure/**"
  - "src/Service/*.Logic/**"
---

# EF Core (Infrastructure)

## DbContext
`${PROJECT}Context : EndaliaDbContext` (SharedKernel.Api.Common.Data). DbSets in a `partial` class, `= null!`. `OnModelCreating` calls `base.OnModelCreating(...)` then `modelBuilder.ApplyConfiguration(new XxxConfig())`. Schema = `${PROJECT}Constants.Schema`.

## Entity configurations
One config per entity, always `internal sealed class XxxConfig : IEntityTypeConfiguration<T>`. Map legacy columns with `HasColumnName(...)`. CoreOrganization entities extend SharedKernel base mappings.

## Repositories
Each repository has a matching interface `IXxxRepository : IRepository<TEntity, TKey>` declared in the Logic layer (`${PROJECT}.Logic/Repository/…`, plus any domain-specific query methods). The implementation lives here: `public class XxxRepository : Repository<TEntity, TKey>, IXxxRepository` (`Repository<,>` from SharedKernel.Api.Common.Data.Repository), ctor takes `EndaliaDbContext`, registered Scoped in `${PROJECT}ServicesExtensions`. Use base helpers:
- Repository methods — reuse threshold: only add a named method to the specific repository when the same query is called from more than one file. For single-caller (ad-hoc) queries, use the generic methods inherited from `Repository<,>` (already available on the specific repository) directly in the handler — no wrapper method needed.
- Reads — always project: never return or load full entity graphs for read-only operations. Always project down to the minimum needed shape — an anonymous type for internal queries, or a named `record` when the result is exposed as a method return type or parameter. This applies whether you are calling the base repository directly from a handler (use its projection-based overloads) or writing a named repository method (the method must apply the same projection pattern and return the mapped type, not the entity). When projection is not viable, explicitly include the necessary navigation properties via `Include()`/`ThenInclude()` — and add `AsSplitQuery()` when loading multiple collection navigations — to avoid multiple synchronous hidden queries at access time. Two shapes are exempt from the projection requirement by design, not by exception: cache-population methods (e.g. a `CachedRepository<>`'s data-loading override) that load a full graph once to serve many future in-memory queries — projecting here would defeat the cache; and methods that accept a caller-supplied `Func<IIncludable<TEntity>, IIncludable>` includes parameter, where the projection/include decision belongs to the call site, not the repository method.
- Writes — minimal fetch: load only the domain entities that need to be mutated. Any supplementary data needed (validation, enrichment) must be retrieved through a projected query, not as additional tracked entities.
- Filtering by a collection: when a query needs to filter entities by membership in a list of IDs or values, use SharedKernel's `WhereIn` helper rather than a `Where(x => list.Contains(x))` expression.
- Pass `cancellationToken` to every async EF call, incl. `SaveChangesAsync(cancellationToken: cancellationToken)`.
- Bulk/legacy: Dapper via `_context.GetDbConnection()` in the ambient transaction.
- **Never expose the DbContext from a repository** (no `GetContext()`). A single, logic-driven exception may exist in the codebase — don't replicate it.


## Migrations

**Never create, delete, or run any migration — schema or data — unless the user explicitly requests it.**

When asked:
- To **create** a schema migration → invoke the `back-common:ef-migration-add` skill (`dotnet ef migrations add`, file conventions).
- To **revert/delete** a migration → invoke the `back-common:ef-migration-revert` skill (`dotnet ef migrations remove`).
- To **seed or transform data** (not schema) → invoke the `back-common:add-data-migration` skill (`IMigrationData` / `IMigrationDataExtended` patterns).
