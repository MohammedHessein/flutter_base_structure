# ❓ Hattrick App – Clarifications & Decisions Log

---

## Architecture Decisions

### ADR-001: AsyncCubit over manual Bloc
- **Decision**: All feature-level state management MUST use `AsyncCubit`.
- **Rationale**: Reduces boilerplate for common async operations (loading, data, error) and provides built-in `executeAsync` functionality.

### ADR-002: Base CRUD UseCase
- **Decision**: Centralize API calls through `baseCrudUseCase.call()`.
- **Rationale**: Ensures a single point of failure/success handling and consistent `BaseModel` mapping.

### ADR-003: Navigation via "Go" Utility
- **Decision**: Use the `Go` class for all navigation.
- **Rationale**: Simplified API and centralized context management.

---

## Technical Clarifications

### Q: How is error handling managed?
**A**: `executeAsync` catches exceptions thrown by `baseCrudUseCase` (via `DioService`) and updates the state. It can also show error toasts automatically.

### Q: Where are themes and styles?
**A**: `lib/src/config/res/` contains the design tokens:
- `ColorManager`: Semantically named colors.
- `AppSizes`: Padding, margins, and font sizes.
- `ConstantManager`: Global timeout and UI constants.

### Q: How do we handle dependencies?
**A**: Use `Injectable`. Features/Cubits use `@injectable`, core services use `@LazySingleton`. Always run `build_runner` after changes.
