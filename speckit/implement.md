# 🚀 Hattrick App – Implementation Workflows

## Workflow 1: Create a New Feature

1. **Scaffold**: Run `mason make feature --name <feature_name>`.
2. **Entity**: Define your data entity in `lib/src/features/[type]/[name]/entity/`.
3. **Cubit**: Implement logic in `presentation/cubits/[name]_cubit.dart` using `AsyncCubit`.
4. **DI**: Run `dart run build_runner build --delete-conflicting-outputs`.
5. **UI**: Build your screen in `presentation/view/`. **Recommendation**: Use `BlocStatelessWidget<C>` or `BlocStatefulWidget<C>` to simplify `BlocProvider` injection.
6. **State Handling**: Use the `BaseStatus.when()` extension in your UI widgets to handle Loading, Success, and Error states exhaustively.
7. **Navigation**: Navigate via `Go.to(context, NewScreen())`.

## Workflow 2: Add an API Endpoint

1. **Constants**: Add endpoint to `ApiConstants`.
2. **Params**: Create a `CrudBaseParams` object defining the endpoint and HTTP method.
3. **Implementation**: Call `baseCrudUseCase.call(params)` within `executeAsync` in your Cubit.
4. **Mapper**: Provide a mapper function to transform JSON into your entity.

## Workflow 3: Adding Localization Strings

1. **JSON**: Update `assets/translations/ar.json` and `en.json`.
2. **Generate**: Run `dart run generate/strings/main.dart`.
3. **Usage**: Use `LocaleKeys.your_key.tr()` in the UI.
