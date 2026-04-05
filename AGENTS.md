# Flutter base structure — AI agent rules

**Primary for [Google Antigravity](https://antigravity.codes/)** (and other tools that read `AGENTS.md`). Workspace rules: keep this file in the repo root. If your Antigravity build only uses **Settings → Agent → Customizations → + Workspace**, paste the same content there, or rely on automatic loading of root `AGENTS.md` when that option is enabled.

Inspired by the [Flutter AI Prompt Guide](https://ahmedslman.github.io/Flutter-Guide-To-Use-AI/) (architecture + forbidden practices), **adapted to this repository** — not the guide’s sample `lib/features/` layout.

## Project stack (authoritative)

- **Layers**: `presentation` → `domain` → `data` under `lib/src/features/{feature_name}/`.
- **Shared core**: `lib/src/core/`, global config in `lib/src/config/` (`themes`, `language`, `res`).
- **DI**: GetIt + **Injectable** — register with `@injectable` / `@lazySingleton`; resolve with `injector<T>()`. Run `dart run build_runner build --delete-conflicting-outputs` after DI changes.
- **State**: `AsyncCubit<T>`, `PaginatedCubit<T>`; prefer `executeAsync` + `Result<T, Failure>` from use cases (`multiple_result`).
- **Networking**: Centralized **Dio** via `DioService` / `NetworkService` — not ad-hoc `Dio()` in features. Responses use `BaseModel<T>`. Paths and hosts live in **`ApiConstants`** (no hardcoded URLs in feature code).
- **Navigation**: `Go` utility, `NamedRoutes`, `PageRouterBuilder` — not raw `Navigator` pushes scattered in features.
- **UI**: RTL-first; **ScreenUtil** (`.h`, `.w`, `.sp`, `.r`); theme via `context.theme` / `context.textStyle`; shared resources via `AppColors`, `AppPadding`, `AppFontSize` in `config/res/`.
- **Strings**: Localization / `.tr()` / generated keys — no user-facing literals in widgets.
- **New feature scaffold**: `mason make feature --name <feature_name>` (non-interactive).

## Presentation layering (map from View → Section → Widget)

This repo uses **`presentation/view/`** and **`presentation/widgets/`** instead of `views/sections/widgets/`. Keep the same separation of concerns:

| Role | Where | Allowed | Forbidden |
|------|--------|---------|-----------|
| **Screen / View** | `presentation/view/` (and `*_screen.dart` where used) | `BlocProvider` / `BlocStatelessWidget`, compose children, navigation entry | Business rules, `BlocBuilder` where avoidable, **any styling or decoration**, direct API/repo calls |
| **Bridge** | `presentation/widgets/` files that orchestrate state (e.g. `*_body.dart`, tab bodies, coordinators) | `context.read<Cubit>()`, `BlocBuilder` / `BlocListener` / `BlocSelector`, map state → props for children, **Structural layout only** (Row/Column/Padding/ListView) | **Inline decoration, colors, text styles, or styling that belongs in leaf widgets**; data-layer calls |
| **Leaf widget** | `presentation/widgets/` pure UI pieces | `const` where possible, data via constructor params only, `AppColors` / theme / ScreenUtil | `context.read` / `BlocProvider`, repository, `Dio`, use cases |

Use **`AsyncBlocBuilder`** / `BaseStatus.when()` patterns from core when they fit; prefer **`BlocSelector`** or **`buildWhen`** when rebuild scope is large.

## Cubit & errors

- Prefer **`executeAsync(operation: () => useCase(...))`** so success/error flows stay consistent with `Result` / `Failure`.
- Avoid **duplicate `try/catch` in every cubit method** when the use case or data layer already returns `Result` or maps failures.
- Do **not** bypass domain: UI → Cubit → Use case → Repository interface; repository **implementation** calls data sources only.

## Forbidden practices (stop and fix if violated)

1. **Thick screens**: Logic or heavy `BlocBuilder` trees in `view/` that should live in bridge widgets.
2. **Styling in Bridge/Sections**: Putting `BoxDecoration`, `AppColors`, or `textStyle` inside bridge widgets; these MUST be delegated to Leaf widgets.
3. **Logic in View**: View files must be thin wrappers using `DefaultScaffold` or similar; no business logic or state manipulation.
4. **Cubit or `context.read` inside leaf widgets** meant to be reusable/pure UI.
3. **`_buildXxx()` forests** in a single file instead of extracted widgets (especially leaf UI).
4. **Hardcoded hex colors** or one-off `Color(0xFF...)` in feature UI — use theme / `AppColors`.
5. **Hardcoded user-visible strings** — use localization.
6. **`Dio()` or raw HTTP** in presentation or domain; **hardcoded base URLs** outside `ApiConstants` / env config.
7. **`Navigator.push` sprawl** — use `Go` / `NamedRoutes` unless an exception already exists in that file.
8. **`BlocBuilder` without `buildWhen`** when the built subtree is expensive or high in the tree.
9. **`ListView` with a huge `children:` list** — use **`ListView.builder`** (or project pagination widgets) for dynamic/long lists.
10. **`Opacity` widget** for simple transparency — prefer **`Color.withOpacity` / `withValues`** on the colored widget.
11. **`print()`** for diagnostics — use **`AppBlocObserver`** / proper logging.
12. **Monolithic files (~200+ lines)** — split by responsibility (view vs bridge vs leaf widget vs cubit).

## When the agent violates a rule

Say explicitly: **"Violated rule #N in AGENTS.md. Fix only that, nothing else."**

## Verification after substantive edits

- `flutter analyze` (or `dart analyze`) clean.
- `dart format .` when many files touched.

## Optional SOPs

Step-by-step workflows live in **`.agent/workflows/`** (`create-feature.md`, `debug-issue.md`, `modify-feature.md`).
