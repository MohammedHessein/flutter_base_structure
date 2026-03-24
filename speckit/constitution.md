# 🏛️ Hattrick App – Project Constitution
# The immutable laws of this codebase.

## 1. Architecture Law: Clean Structure (lib/src)
- All source code MUST live under `lib/src`.
- `lib/src/core/` → Global shared infrastructure.
- `lib/src/features/` → Feature-specific modules, organized by type (`logic/`, `settings/`).
- Feature folder structure: `entity/` (params/models) and `presentation/` (view/widgets/cubits).

## 2. State Management Law: AsyncCubit Standard
- **Primary**: All feature Cubits MUST extend `AsyncCubit<T>` from core.
- **Workflow**: Use `executeAsync()` within Cubit methods to handle loading/error states automatically.
- **Data Fetching**: Use `baseCrudUseCase.call(CrudBaseParams(...))` for all standard API operations.

## 3. Navigation Law: The "Go" Utility
- **Standard**: Always use the `Go` class for navigation.
- **Methods**: Prefer `Go.toNamed()` with `NamedRoutes` for navigation across features.

## 4. Network Law: DioService + BaseModel
- **Service**: API calls are executed via `DioService` (NetworkService).
- **Data Mapping**: All responses MUST be wrapped in `BaseModel<T>`.
- **Params**: Use `CrudBaseParams` to define API path, method, and mapper.

## 5. Dependency Injection Law: Injectable
- **Standard**: Use `@injectable` for Cubits and `@LazySingleton` for core services.
- **Generation**: Run `flutter pub run build_runner build` after adding new injectable components.

## 6. UI Law: Component Reuse
- **Shared Widgets**: Check `lib/src/core/widgets/` before creating new generic components.
- **Styling**: Access sizes consistently via `AppSizes` and colors via `ColorManager`.

## 7. Localization Law
- **Framework**: `easy_localization`.
- **Keys**: Use generated `LocaleKeys` for all strings. Never hardcode user-facing text.
