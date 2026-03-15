---
name: Project Efficiency
description: Guide for working efficiently with the Clean Architecture + BLoC/Cubit project.
---

# Project Efficiency Skill

This skill helps you work effectively with the `flutter_base_structure` project architecture.

## Architecture Patterns
- **Standard**: Clean Architecture + BLoC/Cubit.
- **Layers**: `presentation`, `domain`, `data`.
- **DI**: GetIt + Injectable. Use `injector<T>()`.
- **Networking**: `DioService` (implements `NetworkService`). Base URL is configured in `ApiConstants.baseUrl`.
- **RTL**: Arabic-first. Use `start`/`end` instead of `left`/`right`.

## State Management
- Use `AsyncCubit<T>` for API-heavy states.
- Follow the feature structure for every new feature:
  - `lib/src/features/{feature_name}/`
    - `entity/`: Data entities.
    - `presentation/`:
      - `imports/`: `view_imports.dart` for consolidated imports.
      - `cubits/`: Feature cubits.
      - `view/`: Screens.
      - `widgets/`: Local widgets.

## Development Workflows
1. **Scaffold a Feature**:
   Use the `feature` Mason brick (non-interactive):
   ```bash
   mason make feature --name <feature_name>
   ```
2. **Code Generation**:
   Always run build_runner after making changes to cubits, entities, or DTOs:
   ```bash
   dart run build_runner build --delete-conflicting-outputs
   ```
3. **Consolidated Imports**:
   Add all necessary imports for a feature in its `presentation/imports/view_imports.dart` and use `part` for screens, widgets, and cubits.

## Styling & UI
- Use `IconWidget` for icons.
- Use `CachedImage` for network images.
- Use `AppPadding`, `AppColors`, and `AppFontSize` from `config/res/`.
