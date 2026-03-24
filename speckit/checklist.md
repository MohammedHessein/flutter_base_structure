# ✅ Hattrick App – Development Checklist
# Use this checklist for every feature addition or modification.

---

## 🆕 New Feature Checklist

### 1. Structure
- [ ] Created module in `lib/src/features/[type]/[feature_name]/`
- [ ] Created `entity/[name]_params.dart` if needed.
- [ ] Created `presentation/view/[name]_screen.dart`
- [ ] Created `presentation/cubits/[name]_cubit.dart` (injectable)
- [ ] Created `presentation/widgets/` for feature UI.

### 2. Registration & Routing
- [ ] Registered Cubit with `@injectable`.
- [ ] Added route to `NamedRoutes` enum.
- [ ] Verified route string matches requirements.

### 3. Data & Logic
- [ ] Cubit extends `AsyncCubit<T>`.
- [ ] API calls implemented using `executeAsync`.
- [ ] Operation calls `baseCrudUseCase.call(CrudBaseParams(...))`.
- [ ] Mapper correctly transforms JSON into the entity/model.

### 4. UI Layer
- [ ] Screen uses `Go` class for navigation.
- [ ] Resources accessed via `ColorManager`, `AppSizes`, `ConstantManager`.
- [ ] Loading state handled (AsyncCubit provides status).

### 5. Localization
- [ ] Strings added to AR/EN JSON files.
- [ ] Re-generated `LocaleKeys`.

---

## 🧪 Pre-PR Checklist

- [ ] `flutter analyze` passes.
- [ ] No `print()` statements.
- [ ] Unused imports removed.
- [ ] Verified both AR and EN locales.
