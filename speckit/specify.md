# 📋 Hattrick App – Project Specification
# What this app IS, what it DOES, and how it's built.

## Project Identity
- **Name**: Hattrick
- **Type**: Social & Management platform for the sports community.
- **Framework**: Flutter
- **Package Name**: `my_new_app`
- **Default Language**: Arabic (RTL-first)

---

## Core Architecture Modules

### `lib/src/` Directory Map
| Module | Path | Contents |
|--------|------|----------|
| Config | `src/config/` | Resources, theme, language, and global configurations. |
| Core | `src/core/` | Shared infrastructure: network, navigation, error handling, widgets, base CRUD. |
| Features | `src/features/` | Feature modules split into `logic/` and `settings/`. |

---

## Tech Stack & Standards

### State Management: AsyncCubit
- **Standard**: Every feature Cubit extends `AsyncCubit<T>`.
- **Pattern**: Uses `executeAsync()` to handle state transitions (Loading/Data/Error).

### Networking: Base CRUD
- **Standard**: API calls use `baseCrudUseCase.call(CrudBaseParams(...))`.
- **Response**: Standardized `BaseModel<T>` mapping.

### Navigation: Go Utility
- **Standard**: Centralized `Go` class for all routing needs.

---

## UI Standards
- **Styling**: `ColorManager` for colors, `AppSizes` for layout, `ConstantManager` for defaults.
- **Widgets**: `lib/src/core/widgets/` for global reusable components.
- **Imaging**: `CachedNetworkImage` and `flutter_svg`.
- **Loading**: `Skeletonizer` for polished async states.
