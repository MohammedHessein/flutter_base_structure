# Workflow: create feature (this repo)

Use when adding a new business module. Aligns with the [Flutter AI Prompt Guide](https://ahmedslman.github.io/Flutter-Guide-To-Use-AI/) flow but uses **this** structure.

## Pre-flight

- [ ] Skim an existing feature under `lib/src/features/` as reference.
- [ ] Check `lib/src/core/shared/` (or equivalent) for DI module patterns with **Injectable**.
- [ ] Check `NamedRoutes` / `PageRouterBuilder` for route registration style.
- [ ] Reuse `lib/src/core/widgets/` where possible.

## Steps

1. **Scaffold**  
   `mason make feature --name <feature_snake_case>`

2. **Data**  
   DTOs / mappers in `data/`, remote/local data sources using **`NetworkService`** (or project base data source), **`ApiConstants`** for paths. Repository **impl** calls data sources only.

3. **Domain**  
   Entities, repository **interfaces**, use cases returning **`Result<T, Failure>`** (or project standard).

4. **Presentation**  
   - `cubits/`: extend **`AsyncCubit`** / **`PaginatedCubit`** as appropriate; use **`executeAsync`** with use cases.  
   - `view/`: thin screen, provider, composition.  
   - `widgets/`: split **bridge** (state) vs **leaf** (pure UI).  
   - `imports/view_imports.dart`: `part` / consolidated imports per project convention.

5. **DI**  
   Annotate new classes; run **`dart run build_runner build --delete-conflicting-outputs`**.

6. **Routes**  
   Register in **`NamedRoutes`** / router setup; navigate via **`Go`**.

7. **Verify**  
   `dart analyze`, `dart format .`

## Architecture reminders

- View: composition only.  
- Bridge widget: Bloc + mapping; minimal styling.  
- Leaf widget: UI only; no cubit.  
- No hardcoded URLs/strings/colors in features.
