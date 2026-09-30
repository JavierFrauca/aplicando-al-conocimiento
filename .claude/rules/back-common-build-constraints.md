---
description: Machine execution policy — never run dotnet builds or tests in parallel; sequential only.
paths:
  - "src/**/*.cs"
  - "**/*.csproj"
  - "**/*.sln"
---

# Build & test execution constraints

## Golden rule: never build or test in parallel

Resources are limited. Every `dotnet build` / `dotnet test` / `dotnet run` can spin up a shared compiler process (`VBCSCompiler.exe`, Roslyn build server) that stays resident in memory — several GB in this solution. Running more than one at a time multiplies that consumption and can leave the machine out of resources.

- **Forbidden**: more than one `dotnet build` / `dotnet test` / `dotnet run` running simultaneously — not as parallel tool calls, not in background (`run_in_background`), and not spread across parallel subagents (Agent/Workflow) running at the same time against this solution. Always sequential, one after another.
- If spawning subagents in parallel for other tasks (search, analysis, etc.), ensure none of them compile or run tests at the same time as another.
- If the machine becomes slow or runs out of memory during a build or test run, execute `dotnet build-server shutdown` to kill resident compiler processes before retrying.
