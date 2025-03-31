# 🌿 Pollen Forecast App

A simple, one-page Flutter app that displays an hourly forecast of pollen levels using clean architecture, Bloc state management, and 100% test coverage. Built with scalability, modularity, and maintainability in mind.

---

## ✅ Project Highlights

- 📁 **Feature-first folder structure**
- 🧼 **Clean Architecture** (Data → Domain → Presentation)
- 🔁 **Cubit/Bloc** for predictable state management
- 🧪 **100% test coverage goal** across all layers
- ⚙️ **IoC container** (`get_it`) for dependency injection
- 🚦 **go_router** for declarative, scalable routing
- 🌐 **Dio** for API requests with interceptors
- ❄️ **Freezed** for union types and immutability
- 🔄 **json_serializable** for safe and clean model parsing

---

## 🧠 Architectural & Tooling Decisions

### 🧼 Clean Architecture
- We believe in clear separation of concerns between layers (data, domain, presentation)
- Promotes modularity and testability
- Keeps business logic decoupled from framework-specific code
- Allows feature isolation and potential reuse across apps

### 📁 Feature-First Folder Structure
- Organizes code by feature, not by type
- Makes it easier to navigate, especially as the app scales
- Keeps all logic related to a feature in one place

### 🔁 Bloc/Cubit for State Management
- Provides predictable, testable state transitions
- Works well with union types (Freezed)
- Easy to mock and unit test
- Scopes state to widgets/screens cleanly using `BlocProvider`

### ⚙️ IoC Container (`get_it`)
- We believe in the inversion of control principle
- Decouples instantiation from usage
- Enables easier mocking and dependency replacement in tests
- Central place to wire up dependencies

### 🌐 Dio
- We prefer structured, powerful API clients over low-level solutions
- Built-in interceptors make error handling, logging, and headers easy
- Cancellation tokens and timeouts improve UX and resiliency
- Easy to mock and extend for testing

### 🚦 go_router
- We prefer declarative and testable navigation
- Built-in support for named routes, path parameters, redirection, and guards
- Encourages clean, modular routing definitions
- Scales well with nested navigation and deep linking

### ❄️ Freezed
- We believe in immutability and exhaustive pattern matching
- Reduces boilerplate with `copyWith`, equality, and `toString`
- Enables union/sealed classes, ideal for `Cubit` states and domain failures
- Increases code safety and readability

### 🔄 json_serializable
- We prefer to generate `fromJson`/`toJson` methods over writing them manually
- Reduces parsing bugs and improves code clarity
- Ensures type-safe model serialization
- Works well with `freezed` for immutable DTOs and entities

---

## 📦 Folder Structure

```
lib/
└── features/
    └── pollen/
        ├── data/
        │   ├── datasources/
        │   ├── models/
        │   └── pollen_remote_data_source.dart
        ├── domain/
        │   ├── entities/
        │   ├── repositories/
        │   └── use_cases/
        │       └── get_pollen_forecast.dart
        ├── presentation/
        │   ├── screens/
        │   │   └── pollen_screen.dart
        │   ├── widgets/
        │   └── providers/
        │       └── pollen_cubit.dart
└── router/
    └── app_router.dart
└── core/
    └── service_locator.dart
```

---

## 🧪 Testing Strategy

| Layer         | Tests                             |
|---------------|------------------------------------|
| `data/`       | API integration, model parsing     |
| `domain/`     | Entities, use cases, repository contracts |
| `presentation/`| Widget tests, Cubit states        |

- Use `mocktail` or `mockito` to mock dependencies
- Use `bloc_test` for Cubit testing
- Target: **100% test coverage**

---

## 🚀 Getting Started

1. Run `flutter pub get`
2. Run code generation:
   ```sh
   flutter pub run build_runner build --delete-conflicting-outputs
   ```
3. Run the app:
   ```sh
   flutter run
   ```
4. Run tests:
   ```sh
   flutter test --coverage
   ```

---

Let me know if you want to generate models, states, or set up Cubit testing next!