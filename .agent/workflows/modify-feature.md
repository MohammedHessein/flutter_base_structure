# Workflow: modify existing feature (this repo)

Use for changes to an existing module (new screen, field, API, UI). Same discipline as prompt 07 in the [Flutter AI Prompt Guide](https://ahmedslman.github.io/Flutter-Guide-To-Use-AI/), paths are **`lib/src/features/{feature}/`**.

## Phase 1 — Read & plan (no code)

1. List `entity/`, `domain/`, `data/`, `presentation/` (cubits, view, widgets, imports).
2. Read cubit(s): states, `executeAsync` usage, public methods.
3. Read repository interface + impl + data sources.
4. Read view files: which bridge widgets they use.
5. Read bridge widgets: what leaf widgets they feed.

**Impact analysis** (document before coding):

- **WILL CHANGE**: file → change.  
- **MIGHT BE AFFECTED**: why.  
- **WILL NOT TOUCH**: reason.  
- **NEW FILES**: purpose.

Get approval for the plan when the change is non-trivial.

## Phase 2 — By change type

- **ADD_SCREEN**: cubit methods + use case/repo/datasource if needed → DI → `NamedRoutes` / `Go` → view + bridge + leaf widgets.
- **ADD_FIELD**: entity/DTO/fromJson → repo → use case → cubit state → bridge → leaf props.
- **MODIFY_UI**: prefer **leaf widgets**; adjust bridge only if state wiring changes.
- **ADD_API_CALL**: `ApiConstants` → datasource → repo → use case → cubit (`executeAsync`).
- **REFACTOR**: behavior unchanged; extract widgets, add `buildWhen` / selectors, `const`.

## Phase 3 — Verify

- All states handled in UI.
- `.cursorrules` forbidden list not violated.
- `dart analyze`, `dart format .`
