# Flutter Project Coding Standards & Architecture

this project use Clean Architecture + BLoC/Cubit.

## Architecture
- **Pattern**: Clean Architecture + BLoC/Cubit.
- **Layers**:
  - `presentation`: Widgets, Cubits (using `AsyncCubit<T>` or `PaginatedCubit<T>`).
  - `domain`: UseCases, Repository interfaces, Entities.
  - `data`: RemoteDatasource, DTOs, Dio.
- **DI**: GetIt + Injectable. Use `injector<T>()` to access dependencies.

## Key Rules
- **RTL First**: This app is Arabic-first. Use `start`/`end` instead of `left`/`right`.
- **State Management**: Use `AsyncCubit` for API calls. Update state locally for CRUD operations; do not re-fetch.
- **Navigation**: Use `Go.to()` and `Go.back()`.
- **Icons**: Use `IconWidget` instead of `Icons.*`.
- **Images**: Use `CachedImage` for network images.

## File Structure
- `lib/src/features/{name}/`:
  - `entity/`: Data entities.
  - `presentation/`: 
    - `imports/`: Consolidated imports.
    - `cubits/`: Feature cubits.
    - `view/`: Screens.
    - `widgets/`: Local widgets.

## Commands
- `flutter pub get`: Install dependencies.
- `dart run build_runner build --delete-conflicting-outputs`: Generate code.
- `dart run generate/strings/main.dart`: Generate localization keys.
