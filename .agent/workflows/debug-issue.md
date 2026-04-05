# Workflow: debug issue (this repo)

Use for bugs or wrong behavior. Checklist adapted from the [Flutter AI Prompt Guide](https://ahmedslman.github.io/Flutter-Guide-To-Use-AI/) for **Clean Architecture + AsyncCubit + NetworkService**.

## 1. Gather (no code)

- Error message / stack trace, steps to reproduce, expected vs actual.
- Feature path: `lib/src/features/<feature>/`.

## 2. Classify

### UI_WRONG

- Leaf widget: hardcoded sizes/colors/strings?
- Bridge: passing wrong props from state?
- Compare with design if applicable.

### STATE_ISSUE

- Trace `emit` / `setSuccess` / `setError` / `executeAsync` in cubit.
- `buildWhen` on `BlocBuilder`; `BlocSelector` keys.
- `AsyncState` / `BaseStatus` handling in UI.

### API_FAILURE

- Data source: parsing / `BaseModel` mapping / null safety.
- `ApiConstants` path and method.
- Use case / repo: is `Failure` propagated? Cubit using `executeAsync`?

### NAVIGATION_BUG

- `Go` / `NamedRoutes` / arguments / `context` mounted after async.

### CRASH

- Top of stack trace → exact file/line.
- Null checks, casts, context after dispose.

### PERF_ISSUE

- `BlocBuilder` scope too large; missing `buildWhen` / selector.
- `ListView` not `.builder`; images without sizing/caching (`CachedImage` project rules).

## 3. Report before large fixes

- **ROOT CAUSE**: file + reason.  
- **FIX PLAN**: minimal files.  
- **NOT TOUCHING**: out-of-scope areas.

## 4. Fix

- Smallest change; add `// Fix: ...` when helpful.
- No unrelated refactors.

## 5. Verify

- Repro gone; `dart analyze` clean.
