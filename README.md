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

// 📚 How We Applied SOLID Principles

Whilst we develop with SOLID principles in mind and use them as a guide to solid (pun intended) software engineering practices, we do not follow them as hard rules. This is mainly to avoid having hundreds of tiny classes everywhere throughout the application. We prioritise readability and simplicity over hard rules. With that being said, here's an example of each of the principle used throughout the app:

### S – Single Responsibility Principle
> A class should have only one reason to change.

✅ **Followed**
- Each layer (data, domain, presentation) has clearly separated responsibilities.
- `RawPollenResponseModel` handles only deserialization.
- `PollenForecast` is a pure domain entity.

```dart
class PollenRemoteDataSourceImpl {
  Future<RawPollenResponseModel> getPollenForecast() async {
    final response = await dio.get('...');
    return RawPollenResponseModel.fromJson(response.data);
  }
}
```

---

### O – Open/Closed Principle
> Classes should be open for extension, but closed for modification.

✅ **Mostly Followed**
- Adding new pollen types or fields only affects the model and mapper.
- Other layers remain untouched.

```dart
extension RawPollenResponseModelMapper on RawPollenResponseModel {
  PollenForecast toDomain() {
    return PollenForecast(
      time: time,
      alderPollen: alderPollen,
      birchPollen: birchPollen,
      grassPollen: grassPollen,
      latitude: latitude,
      longitude: longitude,
      elevation: elevation,
    );
  }
}
```

---

### L – Liskov Substitution Principle
> Subtypes must be substitutable for their base types.

✅ **Followed**
- Interfaces like `PollenRepository` and `PollenRemoteDataSource` allow flexible swapping of implementations.

```dart
class PollenRepositoryImpl implements PollenRepository { ... }
```

---

### I – Interface Segregation Principle
> Clients should not depend on interfaces they don’t use.

✅ **Followed**
- Interfaces are tight and minimal. Clients only implement what they actually use.

```dart
abstract class PollenRepository {
  Future<Either<Failure, PollenForecast>> getForecast();
}
```

---

### D – Dependency Inversion Principle
> High-level modules should not depend on low-level modules. Both should depend on abstractions.

✅ **Followed**
- Dependencies are injected using `get_it`.
- `Cubit` depends on `PollenRepository` interface.

```dart
class PollenCubit {
  final PollenRepository repository;
}
```

---

We also chose to omit error handling in the data source layer, allowing the repository to centralize and standardize exception-to-failure mapping.

Domain entities remain free from framework or serialization logic.