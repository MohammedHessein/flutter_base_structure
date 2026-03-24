# 🚀 Hattrick App – Implementation Workflows
# Step-by-step recipes for common development tasks.

---

## Workflow 1: Create a New Feature (Base CRUD)

### Step 1: Scaffold the Structure
```
lib/src/features/logic/[feature_name]/
├── entity/
│   └── (params/models)
└── presentation/
    ├── cubits/
    │   └── [feature]_cubit.dart
    ├── view/
    │   └── [feature]_screen.dart
    └── widgets/
        └── (feature-specific widgets)
```

### Step 2: Create the Cubit
Extend `AsyncCubit` and use `executeAsync`:
```dart
@injectable
class FeatureCubit extends AsyncCubit<List<FeatureEntity>> {
  FeatureCubit() : super([]);

  Future<void> fetchData() async {
    await executeAsync(
      operation: () async => baseCrudUseCase.call(
        CrudBaseParams(
          api: ApiConstants.featureEndpoint,
          httpRequestType: HttpRequestType.get,
          mapper: (json) => /* transform json */,
        ),
      ),
    );
  }
}
```

### Step 3: Global Registration
Run the build runner to register the new Cubit:
```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

### Step 4: Routing & Navigation
1. Add route to `NamedRoutes`.
2. Navigate via `Go.toNamed(NamedRoutes.featureName)`.

---

## Workflow 2: Add a New API Endpoint

1. **Add constant** to `ApiConstants` in `lib/src/core/network/api_endpoints.dart`.
2. **Implement request** in the relevant Cubit using `CrudBaseParams`.
3. **Handle success/failure**: `executeAsync` automatically manages state transitions.

---

## Workflow 3: Localization

1. Update `assets/translations/ar.json` and `en.json`.
2. Generate keys: `flutter pub run easy_localization:generate`.
3. Use `LocaleKeys.key.tr()`.
