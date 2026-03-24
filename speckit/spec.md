# 📋 Project Specification - Flutter Base Structure (Hattrick)

This document defines what the project is, its core capabilities, and the technical foundation it rests upon.

## Project Identity
- **Name**: Flutter Base Structure (Hattrick)
- **Type**: A modular, scalable baseline for Flutter applications.
- **Framework**: Flutter (v3.x)
- **Status**: Active / Template
- **Primary Locale**: Arabic (RTL-first)

---

## Core Architecture

The project follows a **Modified Clean Architecture** with a focus on ease of use via **AsyncCubit** and **Base CRUD** utilities.

### `lib/src/` Directory Map
| Module | Path | Description |
| :--- | :--- | :--- |
| **Config** | `src/config/` | Design tokens, themes, and localization setup. |
| **Core** | `src/core/` | Global infrastructure (Networking, Navigation, DI). |
| **Features** | `src/features/` | Feature-specific logic and UI, split by type. |

---

## Technical Standards

### State Management: BLoC / Cubit
- **Standard**: Features use `AsyncCubit<T>` for individual data objects or `PaginatedCubit<T>` for lists.
- **Helpers**: `BlocStatelessWidget` and `BlocStatefulWidget` simplify `BlocProvider` and `AsyncCubit` creation.
- **Automation**: Use `executeAsync()` to automatically handle loading, data, and error states.
- **UI Handling**: `BaseStatus.when()` extension provides an exhaustive way to handle all async states in the UI.

### Networking: Base CRUD & Dio
- **Client**: [Dio](https://pub.dev/packages/dio) with `DioService`.
- **Logic**: Centralized `baseCrudUseCase` for standard API operations (GET, POST, etc.).
- **Models**: All responses are mapped to `BaseModel<T>`.

### Navigation: Go Utility
- **Standard**: All routing is handled by the `Go` utility class for consistency and simpler Context handling.

---

## UI Documentation
- **Styling**: `ColorManager` for colors, `AppSizes` for padding/font-sizes.
- **Icons/Images**: `IconWidget` (supports IconData, SVG, Network, Lottie, Assets) and `CachedImage`.
- **Micro-interactions**: [Skeletonizer](https://pub.dev/packages/skeletonizer) for loading states and custom transitions via `Go`.
