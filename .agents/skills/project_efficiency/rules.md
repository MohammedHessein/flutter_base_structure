# Flutter Project Coding Rules & Standards

This document defines the mandatory coding standards and architectural patterns for the `flutter_base_structure` project.

## 🏗️ Architecture: Clean Architecture + BLoC/Cubit

We follow a strict separation of concerns across three main layers:

- **Presentation Layer (`lib/src/features/{feature}/presentation/`)**:
    - Contains UI widgets, screens, and BLoC/Cubit classes.
    - Use `AsyncCubit<T>` for standard API calls.
    - Use `PaginatedCubit<T>` for lists with pagination.
    - Use `BlocStatelessWidget<C>` or `BlocStatefulWidget<C>` to simplify `BlocProvider` injection.
- **Domain Layer (`lib/src/features/{feature}/domain/`)** - *Note: Often merged with Entity for simpler features*:
    - Contains business logic, use cases, and repository interfaces.
- **Data Layer (`lib/src/features/{feature}/data/`)**:
    - Contains repository implementations, data sources (Remote/Local), and DTOs.

## 🚦 State Management (BLoC/Cubit)

- **Reactive UI**: Rebuild UI using `BlocBuilder` or use the `BaseStatus.when()` extension for exhaustive state handling.
- **Local State**: For CRUD operations, update the local state immediately (via `AsyncCubit.updateData()`); do not re-fetch the entire list from the server unless necessary.

## 💉 Dependency Injection (DI)

- We use **GetIt** and **Injectable** for DI.
- Register dependencies using `@injectable`, `@lazySingleton`, or `@singleton`.
- Access dependencies using the global `injector<T>()` function.
- Always run `build_runner` after adding/changing injectable classes.

## 🌐 Networking

- We use **Dio** with a centralized `DioService`.
- All API constants must be placed in `ApiConstants`.
- Response handling should be done via `Either<Failure, T>` (using the Dartz package).

## 🗺️ Navigation

- Use the centralized `Go` utility for all navigation.
- Methods: `Go.to(context, Screen())`, `Go.back(context)`.

## 🎨 UI & Styling

- **RTL First**: The app is Arabic-first. Use `padding.start`, `padding.end`, `Alignment.centerLeft/Right`, etc., instead of absolute `left/right`.
- **Icons**: Use the custom `IconWidget`. It supports `IconData`, SVG paths, URLs (via `CachedImage`), Lottie JSON, and asset paths.
- **Images**: Use `CachedImage` for all network-based images.
- **Resources**: Use `AppPadding`, `AppColors`, and `AppFontSize` from `lib/src/core/config/res/`.
- **Typography**: The primary font family is **Expo**.

## 🛠️ Helpers & Utilities

- **Global Loading**: Use `FullScreenLoadingManager.show()` and `FullScreenLoadingManager.hide()` for blocking overlays.
- **General Helpers**: The `Helpers` class provides utilities for FCM tokens, status bar colors, sharing, and language-based string selection (`showByLang`).

## 🛠️ Essential Commands

- **Install dependencies**: `flutter pub get`
- **Generate code**: `dart run build_runner build --delete-conflicting-outputs`
- **Generate localization**: `dart run generate/strings/main.dart`
- **Make Feature**: `mason make feature --name <feature_name>`

## 📁 Feature Structure

Every new feature must follow this structure inside `lib/src/features/{name}/`:
- `entity/`: Data entities.
- `presentation/`:
    - `imports/`: `view_imports.dart` (Consolidated imports using `part`).
    - `cubits/`: State management.
    - `view/`: Screens/Pages.
    - `widgets/`: Local feature-specific widgets.
