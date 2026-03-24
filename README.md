
# 🚀 Flutter Base Structure

A robust, scalable, and feature-rich Flutter starter template built with **Clean Architecture** and **BLoC/Cubit**. Designed for high productivity and maintainable codebases.

---

## 🏗️ Architecture Overview

This project follows **Clean Architecture** principles, ensuring that business logic is isolated from UI and data concerns.

- **Presentation Layer**: UI Widgets, Screens, and BLoCs/Cubits.
- **Domain Layer**: Business logic, use cases, and repository interfaces.
- **Data Layer**: Repository implementations, data sources (Remote/Local), and DTOs.

For detailed coding standards, see the [Coding Rules](.agents/skills/project_efficiency/rules.md).

---

## 🛠️ Tech Stack & Key Features

- **State Management**: [BLoC/Cubit](https://pub.dev/packages/flutter_bloc) (with `AsyncCubit` & `PaginatedCubit` extensions).
- **Dependency Injection**: [GetIt](https://pub.dev/packages/get_it) + [Injectable](https://pub.dev/packages/injectable).
- **Navigation**: Custom `Go` utility for simplified routing.
- **Networking**: [Dio](https://pub.dev/packages/dio) with a centralized `DioService`.
- **UI Components**: Custom `IconWidget`, `CachedImage`, and shared core widgets.
- **RTL Support**: Built with Arabic-first (RTL) support in mind.
- **Code Generation**: [build_runner](https://pub.dev/packages/build_runner) for DI, JSON serialization, and more.
- **Scaffolding**: [Mason](https://pub.dev/packages/mason) for rapid feature development.

---

## 🚀 Getting Started

### 1. Prerequisites
- Flutter SDK (Recommended: v3.x or latest stable)
- [Mason CLI](https://pub.dev/packages/mason_cli) installed globally.

### 2. Installation
```bash
flutter pub get
mason get
```

### 3. Running the App
```bash
flutter run
```

---

## 📝 Essential Commands

| Action | Command |
| :--- | :--- |
| **Install dependencies** | `flutter pub get` |
| **Generate code** | `dart run build_runner build --delete-conflicting-outputs` |
| **Generate localization** | `dart run generate/strings/main.dart` |
| **Scaffold new feature** | `mason make feature --name <feature_name>` |

---

## 📁 Documentation

- [Coding Rules & Standards](.agents/skills/project_efficiency/rules.md)
- [Mason Scaffolding Guide](MASON_GUIDE.md)
- [Spec Kit (Project Design & Decisions)](speckit/spec.md)

---

## 🤝 Project Metadata

- **Bundle ID**: `com.smartvision.flutter_base` or `com.smartvision.cleansolution`
- **Firebase Project**: `Flutter_Base`
- **Backend Type**: ASP / PHP
