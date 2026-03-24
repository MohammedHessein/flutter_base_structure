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
- **Shared Layer (`lib/src/core/shared/`)**:
    - Contains app-wide models (`BaseModel`), global cubits (`UserCubit`), and `AppBlocObserver` for centralized logging.

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
- **Response Format**: All API responses are wrapped in `BaseModel<T>`, which provides `message`, `key`, and `data`.
- **Interceptors**:
    - `ConfigurationInterceptor`: Maps backend-specific keys (PHP/ASP) to standard HTTP status codes.
    - `UnAuthenticatedInterceptor`: Listens for 401s or "unauthenticated/blocked" keys to trigger the global `UnAuthenticatedBottomSheet`.
- All API constants must be placed in `ApiConstants`.

## 🗺️ Navigation

- Use the centralized `Go` utility for all navigation.
- **Routing**: Static routes are defined in `NamedRoutes` and built via `PageRouterBuilder`.
- **Transitions**: Comprehensive support for `fade`, `rotation`, `scale`, `shake`, `size`, `slide`, and platform-default `cupertino`.
- Methods: `Go.to(context, Screen())`, `Go.back(context)`.

## 🛠️ Core Utilities & Extensions

### UI Extensions (`lib/src/core/extensions/`)
- **Widget**: Chainable `.paddingAll()`, `.marginSymmetric()`, `.visibility()`, `.center`.
- **TextStyle**: Chainable `.bold`, `.s16`, `.setPrimaryColor`, `.underline`.
- **BuildContext**: `.width`, `.height`, `.isArabic`, `.hideKeyboard()`, `.isKeyboardOpen`.
- **String**: `.capitalize()`, `.toEnglishNumbers()`, `.toCurrency()`, `.tr()` (via `.locale`).
- **DateTime**: `.toFullDate()`, `.toTime()`, `.toDayMonthYear()`.

### Global Helpers (`lib/src/core/helpers/`)
- **ImageHelper**: Unified API for Picking (Gallery/Camera), Multi-picking, and Cropping.
- **LauncherHelper**: Native deep links for social media (WhatsApp, TikTok, etc.) and system actions (Call, Mail, URLs).
- **CacheStorage**: Standard `SharedPreferences` wrapper for simple data and JSON Maps.
- **SecureStorage**: `FlutterSecureStorage` for tokens and sensitive info.
- **Validators**: Secure field validation (includes script-injection protection).
- **LoadingManager**: `FullScreenLoadingManager` for global blocking overlays.

### Specialized UI Widgets (`lib/src/core/widgets/`)
- **Image Widgets**: `CachedImage` (networking), `CustomAvatar`, `CustomImageSlider`, `UploadImage` (with picker logic), `ImageView`.
- **Media**: `UniversalMediaWidget` (controller-based handling of SVG, Video, and Network/Asset images).
- **Tools**: `PaginatedListWidget` (for infinite scrolling) and `AsyncBlocBuilder` (for reactive UI).
- **Notifications**: `NotificationService` handles FCM tokens, local notification display, and automatic routing via `NotificationNavigator` and `NotificationRoutes`.

## 🏛️ Project Structure (`lib/src/`)
- **`config/`**: Global configuration (`themes`, `language`, `res`).
- **`core/`**: Shared foundational logic.
  - `base_crud/`: Generic Clean Architecture CRUD framework.
  - `error/`: Custom exceptions (`Forbidden`, `Blocked`) and failures.
  - `shared/`: `UserModel`, `UserCubit` (Session Management), and DI setup.
  - `widgets/`: Rich base library (`AppTextField`, `DefaultScaffold`, `LoadingButton`).
  - `network/`: Dio-based `NetworkService` with `UnAuthenticatedInterceptor`.
- **`features/`**: App-specific business modules.

## ⚠️ Error Handling & Resilience
- **Exceptions**: Comprehensive HTTP status mapping in `exceptions.dart`.
- **Failures**: Domain-level `Failure` objects for predictable propagation.
- **Handling Views**: UI states for `Offline`, `Error`, and `Empty` data.

## 🎨 UI & Styling
- **RTL First**: Arabic-first design. Use `start`/`end` instead of `left`/`right`.
- **Responsive UI**: Always use `ScreenUtil` (.h, .w, .sp, .r).
- **Theming**: Access via `context.theme` and `context.textStyle` extensions.

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
