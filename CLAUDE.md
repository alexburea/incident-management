# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

SAP CAP (Cloud Application Programming model) project, `incident-management`, following the standard CAP layout:

- `db/schema.cds` — domain model (CDS entities). Currently empty.
- `srv/services.cds` — service definitions. References `sap.capire.incidents` namespace (`Incidents`, `Customers` entities) that must be defined in `db/schema.cds` — not yet present, so the service definitions will not compile until the schema is filled in.
- `app/` — UI frontend content. Currently empty.

No `package.json` exists yet, so this is not yet a runnable CAP project. Before `cds` commands work, the project needs `npm init` / `cds init`-style setup (package.json with `@sap/cds` as a dependency).

## Commands (once initialized)

Standard CAP CLI commands, run from the project root:

- `cds watch` — start the dev server with auto-reload (also wired as the VS Code task "cds watch" in `.vscode/tasks.json`).
- `cds deploy` — deploy the data model (e.g. to SQLite/HANA).
- `cds compile db/schema.cds` / `cds compile srv/services.cds` — check a single CDS file compiles.

## Architecture

Two services are defined over the same domain model, for different audiences:

- `ProcessorService` — for support personnel ("processors"): read-write `Incidents`, read-only `Customers`.
- `AdminService` — for administrators: read-write on both `Customers` and `Incidents`.

Both are projections on entities in `sap.capire.incidents` (`db/schema.cds`), so any change to the domain model needs review against both service projections.
