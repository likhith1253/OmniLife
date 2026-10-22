# OmniLife Project - Comprehensive Q&A Guide

Complete question-and-answer style explanation of the entire OmniLife project covering architecture, Flutter basics, GetX, BLoC, all features, widgets, and code implementation details.

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Flutter Basics](#2-flutter-basics)
3. [Architecture - Clean Architecture](#3-architecture---clean-architecture)
4. [GetX State Management](#4-getx-state-management)
5. [BLoC (Business Logic Component)](#5-bloc-business-logic-component)
6. [Domain Layer](#6-domain-layer)
7. [Data Layer](#7-data-layer)
8. [Presentation Layer](#8-presentation-layer)
9. [Firebase Integration](#9-firebase-integration)
10. [SQLite Database](#10-sqlite-database)
11. [Theme System](#11-theme-system)
12. [Tasks Feature](#12-tasks-feature)
13. [Calendar Feature](#13-calendar-feature)
14. [Notes Feature](#14-notes-feature)
15. [Habits Feature](#15-habits-feature)
16. [Offline-First Architecture](#16-offline-first-architecture)
17. [Notifications](#17-notifications)
18. [Widget Deep Dives](#18-widget-deep-dives)

---

## 1. Project Overview

### Q1: What is OmniLife?
**A:** OmniLife is a placement-level Flutter Android application for the course **23CSE465 Mobile Application Development**. It's an AI-connected personal life operating system that integrates 12 core life management domains:
- Tasks & Projects
- Calendar & Schedule
- Notes & Knowledge Base
- Habits & Goal Tracking
- Personal Finance
- Wellness & Mood
- Maps & Geofencing
- Sensors & Hardware
- Media & Focus Mode
- Notifications & Cloud Messaging
- Analytics & Performance Insights
- AI Life Copilot (future)

### Q2: What technologies are used?
**A:**
- **Framework**: Flutter (Android, Web & Desktop)
- **Language**: Dart
- **State Management**: GetX (main) + BLoC (for AI)
- **Backend**: Firebase (Auth, Firestore, Storage, FCM, Analytics, Crashlytics)
- **Local Database**: SQLite (sqflite)
- **Architecture**: Clean Architecture (Domain, Data, Presentation layers)

### Q3: What is the project structure?
**A:**
```
lib/
├── app/                    # App-level configuration
│   ├── routes/            # Navigation routes
│   └── theme/             # Theme and styling
├── core/                   # Core services and utilities
│   ├── bindings/          # Dependency injection
│   ├── firebase/          # Firebase setup
│   ├── services/          # Shared services (notifications, etc.)
│   ├── sync/              # Offline sync engine
│   └── utils/             # Utilities
├── data/                   # Data layer
│   ├── datasources/       # Local (SQLite) and Remote (Firebase)
│   ├── models/            # Data models with serialization
│   └── repositories/      # Repository implementations
├── domain/                 # Domain layer
│   ├── entities/          # Business entities
│   ├── repositories/      # Repository interfaces
│   ├── services/          # Domain services
│   └── usecases/          # Business logic use cases
└── presentation/          # Presentation layer
    ├── bloc/              # BLoC (for AI features)
    ├── controllers/       # GetX controllers
    ├── pages/             # Screen widgets
    └── widgets/           # Reusable widgets
```

---

## 2. Flutter Basics

### Q4: What is Flutter?
**A:** Flutter is Google's UI toolkit for building natively compiled applications for mobile, web, and desktop from a single codebase. It uses the Dart programming language and renders widgets directly to the canvas, giving it high performance and native-like feel.

### Q5: What is a Widget in Flutter?
**A:** In Flutter, everything is a widget. A widget is an immutable description of part of the user interface. Widgets are composed together to build complex UIs. There are two main types:
- **StatelessWidget**: Immutable, doesn't change over time (e.g., Text, Icon)
- **StatefulWidget**: Has mutable state that can change (e.g., TextField, Checkbox)

**Example:**
```dart
class MyWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Text('Hello, Flutter!');
  }
}
```

### Q6: What is the build method?
**A:** The `build()` method is called by Flutter to render the widget. It takes a `BuildContext` and returns a `Widget`. The framework calls this method whenever the widget needs to be rebuilt (e.g., when state changes or parent widget rebuilds).

### Q7: What is the widget tree?
**A:** The widget tree is the hierarchical structure of widgets that make up your app. When you compose widgets together, you're building a tree. Flutter's framework efficiently rebuilds only the parts of the tree that need updating.

---

## 3. Architecture - Clean Architecture

### Q8: What is Clean Architecture?
**A:** Clean Architecture is a software design pattern that separates concerns into distinct layers. In OmniLife, we have three main layers:

1. **Domain Layer**: Contains business logic and entities. Framework-agnostic.
2. **Data Layer**: Handles data persistence (SQLite, Firebase) and data models.
3. **Presentation Layer**: UI components, state management, and user interaction.

**Key Principle:** Dependencies point inward - Presentation depends on Domain, Data depends on Domain. Domain has no dependencies on other layers.

### Q9: Why use Clean Architecture?
**A:**
- **Separation of Concerns**: Each layer has a single responsibility
- **Testability**: Business logic can be tested without UI or database
- **Maintainability**: Changes to one layer don't affect others
- **Flexibility**: Can swap implementations (e.g., change database without changing business logic)

### Q10: How does data flow in Clean Architecture?
**A:**
```
User Action (Presentation)
    ↓
Controller calls Use Case (Domain)
    ↓
Use Case calls Repository Interface (Domain)
    ↓
Repository Implementation (Data) calls DataSource
    ↓
DataSource returns Data Model
    ↓
Repository converts to Entity (Domain)
    ↓
Use Case returns Entity to Controller
    ↓
Controller updates UI (Presentation)
```

---

## 4. GetX State Management

### Q11: What is GetX?
**A:** GetX is a lightweight Flutter framework that provides:
- **State Management**: Reactive state with observables
- **Navigation**: Simplified routing and navigation
- **Dependency Injection**: Service locator pattern
- **Utils**: Internationalization, storage, etc.

### Q12: How does GetX state management work?
**A:** GetX uses reactive programming with observables (`.obs`). When an observable changes, all widgets listening to it (via `Obx`) automatically rebuild.

**Example:**
```dart
// Controller
class TaskController extends GetxController {
  final tasks = <Task>[].obs;  // Observable list
  
  void addTask(Task task) {
    tasks.add(task);  // UI updates automatically
  }
}

// UI
Obx(() => Text('${controller.tasks.length} tasks'))
```

### Q13: What is GetXController?
**A:** `GetXController` is a base class for managing state. It has lifecycle methods:
- `onInit()`: Called when controller is created
- `onReady()`: Called after onInit
- `onClose()`: Called when controller is disposed

**Example:**
```dart
class TaskController extends GetxController {
  @override
  void onInit() {
    super.onInit();
    loadTasks();  // Load data when controller initializes
  }
}
```

### Q14: What is the difference between Get.put, Get.lazyPut, and Get.find?
**A:**
- **Get.put()**: Creates instance immediately and keeps it alive
- **Get.lazyPut()**: Creates instance only when first requested
- **Get.find()**: Retrieves an already registered instance

**Example:**
```dart
// In InitialBinding
Get.put(TaskController(), permanent: true);  // Created now, never disposed
Get.lazyPut<TaskRepository>(() => TaskRepositoryImpl());  // Created when first needed

// In controller
TaskRepository repo = Get.find<TaskRepository>();  // Get the instance
```

### Q15: What does permanent: true mean?
**A:** `permanent: true` means the controller will never be disposed, even when navigating away. This is used for controllers that need to persist across the app (e.g., AuthController, TaskController).

### Q16: What does fenix: true mean?
**A:** `fenix: true` means the instance can be recreated after being disposed. Like a phoenix rising from ashes, it can be brought back to life when needed again.

### Q17: How does GetX navigation work?
**A:** GetX provides named routes for navigation:

```dart
// Define routes
abstract final class AppRoutes {
  static const tasks = '/tasks';
  static const calendar = '/calendar';
}

// Navigate
Get.toNamed(AppRoutes.tasks);        // Push new route
Get.offNamed(AppRoutes.calendar);    // Replace current route
Get.offAllNamed(AppRoutes.login);    // Clear stack and navigate
Get.back();                          // Go back
```

### Q18: What is BindingsBuilder?
**A:** `BindingsBuilder` is used to register dependencies for a specific route. When navigating to that route, the bindings are executed to set up the required controllers and repositories.

**Example:**
```dart
GetPage(
  name: AppRoutes.tasks,
  page: () => const TasksPage(),
  binding: BindingsBuilder(() {
    Get.lazyPut<TaskRepository>(() => TaskRepositoryImpl());
    Get.put(TaskController());
  }),
)
```

---

## 5. BLoC (Business Logic Component)

### Q19: What is BLoC?
**A:** BLoC (Business Logic Component) is a state management pattern that uses streams to manage state. It follows an event-driven architecture:
- **Events**: User actions (e.g., "LoadTasks", "AddTask")
- **States**: App states (e.g., "Loading", "Loaded", "Error")
- **Streams**: Unidirectional data flow

### Q20: Why does OmniLife use both GetX and BLoC?
**A:** OmniLife uses a hybrid approach:
- **GetX**: For simple, straightforward state (tasks, calendar, auth) - less boilerplate, faster development
- **BLoC**: Reserved for complex, streaming state (AI Copilot) - better for event-driven workflows, more testable

### Q21: How does BLoC work?
**A:**
```dart
// 1. Define Events
abstract class TaskEvent {}
class LoadTasks extends TaskEvent {}
class AddTask extends TaskEvent { final Task task; }

// 2. Define States
abstract class TaskState {}
class TaskLoading extends TaskState {}
class TaskLoaded extends TaskState { final List<Task> tasks; }

// 3. Create BLoC
class TaskBloc extends Bloc<TaskEvent, TaskState> {
  TaskBloc() : super(TaskLoading()) {
    on<LoadTasks>((event, emit) async {
      emit(TaskLoading());
      final tasks = await repository.getTasks();
      emit(TaskLoaded(tasks));
    });
  }
}

// 4. Use in UI
BlocBuilder<TaskBloc, TaskState>(
  builder: (context, state) {
    if (state is TaskLoading) return CircularProgressIndicator();
    if (state is TaskLoaded) return ListView(children: state.tasks);
  },
)
```

### Q22: What is BlocObserver?
**A:** `BlocObserver` is used to observe all BLoC instances for logging, analytics, or error handling. OmniLife has `AppBlocObserver` that logs errors for debugging.

**Code:**
```dart
class AppBlocObserver extends BlocObserver {
  @override
  void onError(BlocBase<dynamic> bloc, Object error, StackTrace stackTrace) {
    debugPrint('${bloc.runtimeType} error: $error');
    super.onError(bloc, error, stackTrace);
  }
}
```

---

## 6. Domain Layer

### Q23: What is an Entity?
**A:** An entity is a business object that represents a core concept in the domain. Entities are framework-agnostic - they don't know about SQLite, Firebase, or Flutter. They contain only business logic and data.

**Example:**
```dart
class Task {
  const Task({
    required this.id,
    required this.userId,
    required this.title,
    this.description,
    this.completed = false,
    this.priority = TaskPriority.medium,
    required this.createdAt,
    required this.updatedAt,
    this.dueDate,
  });

  final String id;
  final String userId;
  final String title;
  final String? description;
  final bool completed;
  final TaskPriority priority;
  final DateTime createdAt;
  final DateTime updatedAt;
  final DateTime? dueDate;
}
```

### Q24: Why are entities immutable?
**A:** Entities are immutable (all fields are `final`) to prevent unexpected changes. To modify an entity, you create a copy with the new values using `copyWith()`:

```dart
Task updatedTask = task.copyWith(
  completed: true,
  updatedAt: DateTime.now(),
);
```

### Q25: What is a Repository Interface?
**A:** A repository interface defines the contract for data operations without specifying implementation. It lives in the domain layer and is implemented by the data layer.

**Example:**
```dart
abstract class TaskRepository {
  Future<List<Task>> getTasks(String userId);
  Future<Task?> getTask(String id);
  Future<Task> createTask(Task task);
  Future<Task> updateTask(Task task);
  Future<void> deleteTask(String id);
  Future<Task> completeTask(String id);
}
```

### Q26: What is a Use Case?
**A:** A use case (or interactor) encapsulates a single business rule or use case. It coordinates repositories and entities to perform a specific task. Use cases make business logic reusable and testable.

**Example:**
```dart
class GetAgendaForRange {
  const GetAgendaForRange();

  List<AgendaItem> call({
    required List<CalendarEvent> events,
    required List<Task> tasks,
    required DateTime start,
    required DateTime end,
  }) {
    // Business logic to merge events and tasks into agenda
  }
}
```

---

## 7. Data Layer

### Q27: What is a Data Model?
**A:** A data model is similar to an entity but includes serialization logic for storage (SQLite, Firebase). It extends the entity and adds methods like `toMap()`, `fromMap()`, `toFirestoreMap()`.

**Example:**
```dart
class TaskModel extends Task {
  const TaskModel({
    required super.id,
    required super.userId,
    required super.title,
    // ... other fields
  });

  factory TaskModel.fromMap(Map<String, Object?> map) {
    return TaskModel(
      id: map['id'] as String,
      userId: map['user_id'] as String,
      title: map['title'] as String,
      // ... parse other fields
    );
  }

  Map<String, Object?> toMap() {
    return {
      'id': id,
      'user_id': userId,
      'title': title,
      'completed': completed ? 1 : 0,  // SQLite uses 0/1 for bool
      'due_date': dueDate?.millisecondsSinceEpoch,
      // ... other fields
    };
  }

  Map<String, dynamic> toFirestoreMap() {
    return {
      'userId': userId,
      'title': title,
      'completed': completed,
      'dueDate': dueDate?.toIso8601String(),  // Firestore uses ISO strings
      // ... other fields
    };
  }
}
```

### Q28: What is a DataSource?
**A:** A data source is responsible for raw data access - talking to SQLite, Firebase, or APIs. It has no business logic - just CRUD operations.

**Example (SQLite DataSource):**
```dart
class LocalTaskDataSource {
  Future<void> insertTask(TaskModel task) async {
    final db = await database;
    await db.insert('tasks', task.toMap());
  }

  Future<List<TaskModel>> getAllTasks(String userId) async {
    final db = await database;
    final rows = await db.query(
      'tasks',
      where: 'user_id = ?',
      whereArgs: [userId],
    );
    return rows.map(TaskModel.fromMap).toList();
  }
}
```

### Q29: What is a Repository Implementation?
**A:** A repository implementation implements the repository interface from the domain layer. It coordinates data sources, handles caching, and implements business rules for data operations.

**Example:**
```dart
class TaskRepositoryImpl implements TaskRepository {
  final LocalTaskDataSource? localDataSource;
  final UserScopedFirestoreDataSource? remoteDataSource;
  final Map<String, Task> _memoryCache = {};

  @override
  Future<List<Task>> getTasks(String userId) async {
    // 1. Try memory cache
    if (_memoryCache.isNotEmpty) {
      return _memoryCache.values.where((t) => t.userId == userId).toList();
    }
    
    // 2. Load from SQLite
    final localTasks = await localDataSource.getAllTasks(userId);
    
    // 3. Cache in memory
    for (final t in localTasks) {
      _memoryCache[t.id] = t;
    }
    
    // 4. Background sync from Firestore
    _syncFromRemote(userId);
    
    return localTasks;
  }

  @override
  Future<Task> createTask(Task task) async {
    final model = TaskModel.fromEntity(task);
    
    // Optimistic update
    _memoryCache[model.id] = model;
    
    // Save to SQLite
    await localDataSource.insertTask(model);
    
    // Non-blocking sync to Firestore
    _syncTaskToRemote(task.userId, model);
    
    return model;
  }
}
```

### Q30: What is the difference between DataSource and Repository?
**A:**
- **DataSource**: Raw data access (SQL queries, API calls). No business logic.
- **Repository**: Coordinates data sources, implements caching, handles business rules, provides domain entities to the rest of the app.

---

## 8. Presentation Layer

### Q31: What is a Controller?
**A:** A controller (in GetX) manages the state for a feature or screen. It contains business logic, calls repositories, and exposes reactive observables for the UI to listen to.

**Example:**
```dart
class TaskController extends GetxController {
  final tasks = <Task>[].obs;
  final isLoading = false.obs;
  final errorMessage = RxnString();

  Future<void> loadTasks() async {
    isLoading.value = true;
    try {
      final loaded = await repository.getTasks(userId);
      tasks.assignAll(loaded);
    } catch (e) {
      errorMessage.value = 'Failed to load tasks';
    } finally {
      isLoading.value = false;
    }
  }
}
```

### Q32: What is Obx?
**A:** `Obx` is a GetX widget that rebuilds when any observable it references changes. It's the main way to make UI reactive.

**Example:**
```dart
Obx(() {
  if (controller.isLoading.value) {
    return CircularProgressIndicator();
  }
  return ListView.builder(
    itemCount: controller.tasks.length,
    itemBuilder: (context, index) => TaskCard(controller.tasks[index]),
  );
})
```

### Q33: What is a Page in Flutter?
**A:** A page is a full-screen widget. In OmniLife, pages are in `lib/presentation/pages/` and represent screens like TasksPage, CalendarPage, etc.

### Q34: What is a Widget in the presentation layer?
**A:** Reusable UI components that are shared across pages. In OmniLife, widgets are in `lib/presentation/widgets/` and include things like TaskCard, EventBlock, ModuleTile, etc.

---

## 9. Firebase Integration

### Q35: How is Firebase initialized in OmniLife?
**A:** Firebase is initialized in `main.dart` before the app starts:

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  final firebaseReady = await initializeFirebase();
  await initializeTelemetry(firebaseReady: firebaseReady);
  Bloc.observer = AppBlocObserver();
  runApp(const OmniLifeApp());
}
```

**Firebase Bootstrap:**
```dart
Future<bool> initializeFirebase() async {
  try {
    await Firebase.initializeApp(
      options: DefaultFirebaseOptions.currentPlatform,
    );
    return true;
  } catch (error) {
    if (kDebugMode) debugPrint('Firebase initialization failed: $error');
    return false;
  }
}
```

### Q36: What Firebase services are used?
**A:**
- **Firebase Auth**: Email/password and Google Sign-In authentication
- **Cloud Firestore**: NoSQL database for cloud sync
- **Firebase Storage**: File storage (receipts, images)
- **FCM (Firebase Cloud Messaging)**: Push notifications
- **Firebase Analytics**: Usage analytics
- **Firebase Crashlytics**: Crash reporting

### Q37: How does Firebase Auth work?
**A:** OmniLife uses the `AuthRepository` to interface with Firebase Auth:

```dart
class AuthRepositoryImpl implements AuthRepository {
  final FirebaseAuth _firebaseAuth = FirebaseAuth.instance;
  final GoogleSignIn _googleSignIn = GoogleSignIn();

  @override
  Future<void> register({required String email, required String password}) async {
    await _firebaseAuth.createUserWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  @override
  Future<void> login({required String email, required String password}) async {
    await _firebaseAuth.signInWithEmailAndPassword(
      email: email,
      password: password,
    );
  }

  @override
  Future<void> signInWithGoogle() async {
    final GoogleSignInAccount? googleUser = await _googleSignIn.signIn();
    final GoogleSignInAuthentication? googleAuth = await googleUser?.authentication;
    final credential = GoogleAuthProvider.credential(
      accessToken: googleAuth?.accessToken,
      idToken: googleAuth?.idToken,
    );
    await _firebaseAuth.signInWithCredential(credential);
  }

  @override
  Stream<AppUser?> authStateChanges() {
    return _firebaseAuth.authStateChanges().map((user) {
      if (user == null) return null;
      return AppUser(
        uid: user.uid,
        email: user.email,
        displayName: user.displayName,
        photoUrl: user.photoURL,
      );
    });
  }
}
```

### Q38: How does Firestore data structure work?
**A:** OmniLife uses a user-scoped Firestore structure:

```
users/{uid}
  ├── tasks/{taskId}
  │     └── { title, description, completed, priority, dueDate, ... }
  ├── events/{eventId}
  │     └── { title, startAt, endAt, colorTag, ... }
  ├── notes/{noteId}
  │     └── { title, content, category, tags, ... }
  └── habits/{habitId}
        └── { title, frequency, targetDays, ... }
```

### Q39: What is UserScopedFirestoreDataSource?
**A:** It's a generic data source that handles CRUD operations for any user-scoped Firestore collection. It abstracts away the Firestore complexity:

```dart
class UserScopedFirestoreDataSource {
  Future<List<Map<String, dynamic>>> list(String uid, String collection) async {
    final col = firestore.collection('users/$uid/$collection');
    final snapshot = await col.get().timeout(Duration(milliseconds: 1500));
    return snapshot.docs.map((doc) => {'id': doc.id, ...doc.data()}).toList();
  }

  Future<void> set(String uid, String collection, String docId, Map<String, dynamic> data) {
    final col = firestore.collection('users/$uid/$collection');
    return col.doc(docId).set(data, SetOptions(merge: true)).timeout(Duration(milliseconds: 1500));
  }
}
```

---

## 10. SQLite Database

### Q40: What is SQLite?
**A:** SQLite is an embedded SQL database engine. It's stored as a single file on the device and requires no separate server process. OmniLife uses SQLite for offline data storage.

### Q41: How is the database initialized?
**A:** The `AppDatabase` singleton manages the SQLite database:

```dart
class AppDatabase {
  static const schemaVersion = 7;
  static const tasksTable = 'tasks';
  static const eventsTable = 'calendar_events';
  // ... other tables

  Database? _database;

  Future<Database> get database async {
    return _database ??= await open();
  }

  Future<Database> open() async {
    final databasesPath = await getDatabasesPath();
    final path = p.join(databasesPath, 'omnilife.db');
    return openDatabase(
      path,
      version: schemaVersion,
      onCreate: (db, version) async {
        await db.execute(_createTasksTableSql);
        await db.execute(_createEventsTableSql);
        // ... create all tables
      },
      onUpgrade: (db, oldVersion, newVersion) async {
        if (oldVersion < 2) {
          await db.execute(_createEventsTableSql);
        }
        // ... handle migrations
      },
    );
  }
}
```

### Q42: What tables exist in the database?
**A:**
- `tasks`: Task data
- `calendar_events`: Calendar events
- `notes`: Notes and markdown content
- `habits`: Habit definitions
- `habit_logs`: Habit completion logs
- `finance`: Financial transactions
- `wellness_logs`: Wellness tracking data

### Q43: How does data serialization work with SQLite?
**A:** SQLite doesn't have native support for Dart objects, so we convert:
- **DateTime** → `int` (milliseconds since epoch)
- **bool** → `int` (0 or 1)
- **enum** → `String` (enum name)
- **List** → JSON string

**Example:**
```dart
Map<String, Object?> toMap() {
  return {
    'id': id,
    'completed': completed ? 1 : 0,  // bool to int
    'priority': priority.name,  // enum to string
    'due_date': dueDate?.millisecondsSinceEpoch,  // DateTime to int
    'created_at': createdAt.millisecondsSinceEpoch,
  };
}

factory TaskModel.fromMap(Map<String, Object?> map) {
  return TaskModel(
    id: map['id'] as String,
    completed: (map['completed'] as int) == 1,  // int to bool
    priority: TaskPriority.values.byName(map['priority'] as String),  // string to enum
    dueDate: map['due_date'] != null 
        ? DateTime.fromMillisecondsSinceEpoch(map['due_date'] as int) 
        : null,
  );
}
```

---

## 11. Theme System

### Q44: How does the theme system work?
**A:** OmniLife uses Material 3 design system with custom themes. The theme is defined in `AppTheme` class with light and dark variants:

```dart
abstract final class AppTheme {
  static ThemeData get light => _build(
    brightness: Brightness.light,
    background: AppColors.lightBackground,
    surface: AppColors.lightSurface,
    // ... other colors
  );

  static ThemeData get dark => _build(
    brightness: Brightness.dark,
    background: AppColors.darkBackground,
    surface: AppColors.darkSurface,
    // ... other colors
  );
}
```

### Q45: What is the design philosophy?
**A:** OmniLife follows an "anti-slop" aesthetic:
- No purple-to-pink gradients
- Glassmorphism only on auth screens
- 1px hairline borders instead of thick borders
- Solid surfaces with subtle shadows
- Tabular figures for numbers and dates

### Q46: How does theme switching work?
**A:** The `AppController` manages the theme mode:

```dart
class AppController extends GetxController {
  final themeMode = ThemeMode.system.obs;

  void toggleTheme() {
    themeMode.value = themeMode.value == ThemeMode.dark
        ? ThemeMode.light
        : ThemeMode.dark;
  }
}
```

The app uses `Obx` to react to theme changes:
```dart
Obx(
  () => GetMaterialApp(
    theme: AppTheme.light,
    darkTheme: AppTheme.dark,
    themeMode: appController.themeMode.value,
  ),
)
```

### Q47: What are semantic colors?
**A:** Semantic colors are named colors that have meaning in the app context, separate from the Material color scheme. They're defined in `AppSemanticColors` and used for specific features:

```dart
class AppSemanticColors extends ThemeExtension<AppSemanticColors> {
  final Color moduleTasks;
  final Color moduleCalendar;
  final Color moduleNotes;
  final Color moduleHabits;
  final Color habits;
  // ... other semantic colors
}
```

---

## 12. Tasks Feature

### Q48: How does the Tasks feature work?
**A:** The Tasks feature allows users to create, complete, and manage tasks. It consists of:
- **Task Entity**: Represents a task with title, description, priority, due date
- **TaskController**: Manages task state and operations
- **TasksPage**: UI for viewing and managing tasks
- **TaskCard**: Widget for displaying a single task
- **CreateTaskSheet**: Bottom sheet for creating/editing tasks

### Q49: How is a task created?
**A:**
1. User taps "New Task" button
2. `CreateTaskSheet.show()` opens the bottom sheet
3. User fills form and taps "Create Task"
4. `_submit()` validates form
5. Calls `TaskController.createTask()`
6. Controller adds task to list optimistically
7. Saves to SQLite via `LocalTaskDataSource`
8. Syncs to Firestore in background
9. Shows SnackBar confirmation
10. Closes sheet

**Code Flow:**
```dart
// 1. UI calls controller
final created = await controller.createTask(
  title: title,
  description: desc,
  priority: _priority,
  dueDate: _dueDate,
);

// 2. Controller (optimistic update)
tasks.insert(0, newTask);
final created = await _taskRepository.createTask(newTask);
tasks[index] = created;  // Update with server response

// 3. Repository
await _localDataSource.insertTask(model);
_syncTaskToRemote(task.userId, model);  // Background sync
```

### Q50: What is optimistic UI update?
**A:** Optimistic UI update means the UI updates immediately assuming the operation will succeed, before waiting for the actual save to complete. If the save fails, the UI reverts to the previous state.

**Example:**
```dart
// Add to UI immediately
tasks.insert(0, newTask);

try {
  // Actually save
  final created = await repository.createTask(newTask);
  tasks[index] = created;  // Update with real data
} catch (e) {
  // Revert if failed
  tasks.removeWhere((t) => t.id == newTask.id);
}
```

### Q51: How do task filters work?
**A:** The `TaskController` has a `filteredTasks` getter that applies filters:

```dart
List<Task> get filteredTasks {
  return tasks.where((t) {
    // Priority filter
    if (priority != null && t.priority != priority) return false;
    
    // Search query filter
    if (query.isNotEmpty) {
      final titleMatch = t.title.toLowerCase().contains(query);
      final descMatch = t.description?.toLowerCase().contains(query) ?? false;
      if (!titleMatch && !descMatch) return false;
    }
    
    // Tab filter (All/Today/Upcoming/Completed)
    switch (selectedFilter.value) {
      case TaskFilter.today:
        return isToday(t.dueDate) && !t.completed;
      case TaskFilter.completed:
        return t.completed;
      // ... other filters
    }
  }).toList();
}
```

### Q52: How does TaskCard widget work?
**A:** `TaskCard` displays a single task with:
- Circular checkbox (animated)
- Title and description
- Priority indicator (colored dot)
- Due date with smart formatting ("Today", "Tomorrow", "Jan 15")
- Delete button

**Key Features:**
```dart
// Animated checkbox
AnimatedContainer(
  duration: Duration(milliseconds: 180),
  decoration: BoxDecoration(
    color: task.completed ? theme.colorScheme.primary : Colors.transparent,
    shape: BoxShape.circle,
  ),
  child: task.completed ? Icon(Icons.check) : null,
)

// Smart date formatting
String _formatDueDate(DateTime due) {
  final diff = taskDay.difference(today).inDays;
  if (diff == 0) return 'Today';
  if (diff == 1) return 'Tomorrow';
  if (diff == -1) return 'Yesterday';
  return '${months[due.month - 1]} ${due.day}';
}
```

---

## 13. Calendar Feature

### Q53: How does the Calendar feature work?
**A:** The Calendar feature provides multi-view scheduling:
- **Month View**: Grid of days with event dots
- **Week View**: 7-day timeline with event blocks
- **Day View**: Single day detailed timeline
- **Agenda View**: List of events and tasks for selected day

### Q54: How is the calendar grid built?
**A:** The calendar grid is built by calculating the days needed to fill the month:
1. Get first day of month
2. Calculate which weekday it falls on
3. Add padding days from previous month
4. Add all days of current month
5. Add padding days from next month to complete grid

### Q55: How do calendar events work?
**A:** Calendar events are similar to tasks but with time ranges:
- **CalendarEvent Entity**: title, startAt, endAt, isAllDay, colorTag, type
- **CalendarController**: Manages event state
- **CalendarPage**: UI with month/week/day views
- **EventBlock**: Widget for displaying events in timeline
- **CreateEventSheet**: Bottom sheet for creating events

### Q56: What is the difference between events and focus blocks?
**A:**
- **Events**: Regular calendar events (meetings, appointments)
- **Focus Blocks**: Dedicated time blocks for deep work, displayed with a bolt icon

### Q57: How does the timeline view work?
**A:** The timeline view uses a positioning algorithm to place events side-by-side when they overlap:

```dart
List<PositionedEventLayout> overlapLayoutForDay(DateTime day) {
  return _agendaUseCase.packDayEvents(events, day);
}
```

The algorithm:
1. Sort events by start time
2. Group overlapping events into columns
3. Calculate position (left, width, top, height) for each event
4. Render using `Positioned` widgets

### Q58: How does the agenda view merge events and tasks?
**A:** The `GetAgendaForRange` use case merges events and tasks with due dates into a single timeline:

```dart
List<AgendaItem> call({
  required List<CalendarEvent> events,
  required List<Task> tasks,
  required DateTime start,
  required DateTime end,
}) {
  final items = <AgendaItem>[];
  
  // Add events
  for (final event in events) {
    if (event.startAt.isAfter(start) && event.endAt.isBefore(end)) {
      items.add(AgendaItem.event(event));
    }
  }
  
  // Add tasks with due dates
  for (final task in tasks) {
    if (task.dueDate != null && 
        task.dueDate!.isAfter(start) && 
        task.dueDate!.isBefore(end)) {
      items.add(AgendaItem.task(task));
    }
  }
  
  // Sort by time
  items.sort((a, b) => a.time.compareTo(b.time));
  return items;
}
```

---

## 14. Notes Feature

### Q59: How does the Notes feature work?
**A:** The Notes feature provides markdown-enabled note-taking:
- **Note Entity**: title, content (markdown), category, tags, isPinned, isArchived
- **NotesController**: Manages notes state, search, filtering
- **NotesPage**: UI for viewing and managing notes
- **NoteCard**: Widget for displaying a note
- **MarkdownNoteView**: Widget for rendering markdown content

### Q60: How does markdown rendering work?
**A:** OmniLife uses `flutter_markdown_plus` to render markdown content. Custom styling is applied via `MarkdownStyleSheet`:

```dart
static MarkdownStyleSheet styleSheetFor(BuildContext context) {
  final theme = Theme.of(context);
  final base = MarkdownStyleSheet.fromTheme(theme);
  return base.copyWith(
    p: theme.textTheme.bodyMedium,
    h1: theme.textTheme.titleLarge?.copyWith(fontWeight: FontWeight.w600),
    code: theme.textTheme.bodySmall?.copyWith(
      fontFamily: 'monospace',
      backgroundColor: semanticColors.surfaceRaised,
    ),
    blockquoteDecoration: BoxDecoration(
      border: Border(left: BorderSide(color: semanticColors.hairline, width: 3)),
    ),
  );
}
```

### Q61: How do checklists work in notes?
**A:** Notes support task lists using markdown syntax:
```markdown
- [ ] Buy groceries
- [x] Call mom
- [ ] Finish report
```

The `ChecklistParser` parses these lines and the `MarkdownNoteView` renders them as interactive checkboxes:

```dart
// Parse markdown into segments
for (final seg in ChecklistParser.segments(content)) {
  if (seg.item != null) {
    children.add(_ChecklistRow(
      item: seg.item,
      onTap: () => onToggleChecklist!(item.lineIndex),
    ));
  } else {
    children.add(MarkdownBody(data: seg.text!));
  }
}
```

### Q62: How does note search work?
**A:** NotesController has a debounced search with ranking:

```dart
void setSearchInput(String value) {
  searchInput.value = value;
  _debounceTimer?.cancel();
  _debounceTimer = Timer(searchDebounce, () {
    activeQuery.value = value.trim();  // Actual search query
  });
}

List<Note> get visibleNotes {
  Iterable<Note> pool = _pool;
  
  // Apply filters
  if (category != null) pool = pool.where((n) => n.category == category);
  if (tag != null) pool = pool.where((n) => n.tags.contains(tag));
  
  // Apply ranked search
  if (query.isEmpty) return pool.toList();
  return NoteSearchRanker.rank(pool, query).map((r) => r.note).toList();
}
```

The `NoteSearchRanker` ranks results by:
- Title matches (higher priority)
- Content matches
- Tag matches
- Relevance score

### Q63: How does converting checklist to tasks work?
**A:** Users can convert unchecked checklist items into tasks:

```dart
Future<ConvertChecklistResult> convertChecklistToTasks(Note note) async {
  final result = await _convert(note);
  if (result.created > 0) {
    await Get.find<TaskController>().loadTasks();  // Refresh tasks
  }
  return result;
}
```

The `ConvertChecklistToTasks` use case:
1. Parses checklist items from note
2. Filters unchecked items
3. Deduplicates against existing tasks
4. Creates new tasks for each unique item
5. Returns count of created tasks

---

## 15. Habits Feature

### Q64: How does the Habits feature work?
**A:** The Habits feature tracks recurring activities:
- **Habit Entity**: title, frequency (daily/weekly/custom), targetDays, streaks
- **HabitLog Entity**: completion records for specific dates
- **HabitsController**: Manages habits and logs
- **HabitsPage**: UI for viewing habits
- **HabitHeatmap**: GitHub-style heatmap showing completion history
- **HabitCard**: Widget for displaying a habit

### Q65: How are habit streaks calculated?
**A:** The `HabitStreaks` use case calculates streaks based on:
- Frequency (daily, weekly, custom)
- Target days per week
- Specific days (e.g., Mon, Wed, Fri)
- Completion history

**Algorithm:**
```dart
static StreakResult compute({
  required Set<String> completedDates,
  required HabitFrequency frequency,
  required List<int> specificDays,
  required int targetDaysPerWeek,
  required DateTime today,
}) {
  // Sort completed dates
  final sorted = completedDates.toList()..sort();
  
  // Calculate current streak
  int current = 0;
  for (int i = sorted.length - 1; i >= 0; i--) {
    if (isConsecutive(sorted[i], sorted[i + 1], frequency)) {
      current++;
    } else {
      break;
    }
  }
  
  // Calculate longest streak
  int longest = 0;
  int temp = 0;
  for (int i = 0; i < sorted.length; i++) {
    if (isConsecutive(sorted[i], sorted[i - 1], frequency)) {
      temp++;
    } else {
      longest = max(longest, temp);
      temp = 1;
    }
  }
  
  return StreakResult(current: current, longest: longest);
}
```

### Q66: How does the habit heatmap work?
**A:** The `HabitHeatmap` widget displays a GitHub-style contribution graph:
- 365-day grid (one column per week, 7 rows)
- Monday-first columns
- Month labels on top
- Day labels on left
- Color intensity based on completion

**Key Implementation:**
```dart
// Build grid
final grid = HeatmapGrid.build(
  completedDates: widget.completedDates,
  today: widget.today,
);

// Render cells
for (final week in grid.weeks) {
  for (final cell in week) {
    _CellView(
      cell: cell,
      doneColor: doneColor,
      selected: cell?.key == _selectedKey,
      onTap: () => setState(() => _selectedKey = cell?.key),
    );
  }
}
```

### Q67: How does habit completion work?
**A:** Toggling a habit completion:
1. User taps a day in the heatmap
2. `HabitsController.toggleHabitLog()` is called
3. Optimistic update: add/remove date from logs
4. Recalculate streaks in memory
5. Save to database
6. If save fails, revert optimistic update

**Code:**
```dart
Future<bool?> toggleHabitLog(String habitId, DateTime date) async {
  final dateStr = HabitStreaks.dateKey(date);
  final logs = habitLogs[habitId];
  final wasCompleted = logs.contains(dateStr);
  
  // Optimistic update
  if (wasCompleted) {
    logs.remove(dateStr);
  } else {
    logs.add(dateStr);
  }
  _recalculateInMemory(habitId);
  
  try {
    await _habitRepository.toggleHabitLog(habitId, dateStr);
    await _persistStreaks(habitId);
    return !wasCompleted;
  } catch (e) {
    // Revert
    if (wasCompleted) logs.add(dateStr);
    else logs.remove(dateStr);
    _recalculateInMemory(habitId);
    return null;
  }
}
```

---

## 16. Offline-First Architecture

### Q68: What is offline-first architecture?
**A:** Offline-first means the app works without an internet connection. All data is stored locally (SQLite) first, then synced to the cloud (Firebase) when connectivity is available.

### Q69: How does OmniLife implement offline-first?
**A:**
1. **All writes go to SQLite first** - data is saved locally immediately
2. **UI updates optimistically** - user sees instant feedback
3. **Background sync to Firebase** - data synced when network available
4. **Read from local cache** - always fast, no network dependency
5. **Sync queue** - failed syncs are retried

**Example Flow:**
```dart
Future<Task> createTask(Task task) async {
  // 1. Save to SQLite immediately
  await localDataSource.insertTask(model);
  
  // 2. Update UI (optimistic)
  tasks.add(task);
  
  // 3. Sync to Firebase in background (non-blocking)
  _syncTaskToRemote(task.userId, model);
  
  return task;
}

void _syncTaskToRemote(String userId, TaskModel model) async {
  try {
    await remoteDataSource.set(userId, 'tasks', model.id, model.toFirestoreMap())
        .timeout(Duration(milliseconds: 1500));
  } catch (_) {
    // Safe to ignore - local state is saved
  }
}
```

### Q70: What is the sync engine?
**A:** The sync engine (`SyncEngine`) manages the synchronization between local SQLite and remote Firebase. It:
- Monitors connectivity status
- Queues sync operations
- Retries failed operations
- Handles conflicts

**Key Components:**
- `ConnectivityMonitor`: Detects network changes
- `SyncOutbox`: Queue of pending sync operations
- `RemoteWriter`: Writes to Firebase

### Q71: How does the sync queue work?
**A:** The sync queue (SyncOutbox) stores operations that need to be synced:

```dart
class SyncOperation {
  final String id;
  final String entityType;  // 'task', 'event', etc.
  final String entityId;
  final String operation;  // 'create', 'update', 'delete'
  final Map<String, dynamic> payload;
  final DateTime createdAt;
  final int retryCount;
}
```

When network is available, the sync engine processes the queue and removes successful operations.

---

## 17. Notifications

### Q72: How do notifications work in OmniLife?
**A:** OmniLife uses two notification systems:
1. **Local Notifications**: `flutter_local_notifications` for scheduled reminders
2. **Push Notifications**: Firebase Cloud Messaging (FCM) for remote notifications

### Q73: How are local notifications scheduled?
**A:** When a task is created with a due date, a local notification is scheduled:

```dart
await notifService.scheduleReminder(
  id: title.hashCode & 0x7FFFFFFF,
  title: 'Reminder: $title',
  body: 'Task is due now',
  scheduledDate: _dueDate!,
  payload: 'task:${created.id}',
);
```

The notification appears at the scheduled time with "Mark done" and "Snooze" actions.

### Q74: How do notification actions work?
**A:** Notifications can have actions that trigger background code:

```dart
NotificationDetails buildReminderDetails({
  bool withActions = true,
  bool markDone = true,
}) {
  return NotificationDetails(
    android: AndroidNotificationDetails(
      channelId,
      channelName,
      actions: withActions
          ? [
              if (markDone)
                AndroidNotificationAction(
                  NotificationActionIds.markDone,
                  'Mark done',
                  cancelNotification: true,
                ),
              AndroidNotificationAction(
                NotificationActionIds.snooze,
                'Snooze 10 min',
                cancelNotification: true,
              ),
            ]
          : null,
    ),
  );
}
```

When the user taps "Mark done", the `notificationBackgroundHandler` runs in a background isolate and completes the task.

### Q75: What is the in-app notification banner?
**A:** For demo visibility, OmniLife shows an in-app banner (Snackbar) when notifications arrive:

```dart
void showInAppNotification(AppNotification item) {
  Get.rawSnackbar(
    snackPosition: SnackPosition.TOP,
    icon: Container(
      padding: EdgeInsets.all(8),
      decoration: BoxDecoration(
        color: accentColor.withValues(alpha: 0.15),
        shape: BoxShape.circle,
      ),
      child: Icon(icon, color: accentColor),
    ),
    titleText: Text(item.title),
    messageText: Text(item.body),
  );
}
```

---

## 18. Widget Deep Dives

### Q76: How does ModuleTile work?
**A:** `ModuleTile` is a reusable widget for the dashboard module grid:

```dart
class ModuleTile extends StatelessWidget {
  const ModuleTile({
    required this.name,
    required this.icon,
    required this.accentColor,
    this.countLabel,
    this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    return Material(
      child: InkWell(
        onTap: onTap,
        child: Container(
          padding: EdgeInsets.all(AppSpacing.md),
          decoration: BoxDecoration(
            color: surface,
            borderRadius: AppRadius.cardRadius,
            border: Border.all(color: hairline, width: 1),
          ),
          child: Column(
            children: [
              // Icon with colored background
              Container(
                decoration: BoxDecoration(
                  color: accentColor.withValues(alpha: 0.12),
                  borderRadius: BorderRadius.circular(10),
                ),
                child: Icon(icon, color: accentColor),
              ),
              // Name and chevron
              Text(name),
              Icon(Icons.chevron_right),
            ],
          ),
        ),
      ),
    );
  }
}
```

**Key Features:**
- Colored icon background (12% opacity)
- Item count label (optional)
- Hairline border
- InkWell for tap feedback

### Q77: How does ProgressRing work?
**A:** `ProgressRing` displays a circular progress indicator with percentage:

```dart
class ProgressRing extends StatelessWidget {
  const ProgressRing({
    required this.progress,
    required this.size,
    required this.strokeWidth,
    this.centerText,
  });

  @override
  Widget build(BuildContext context) {
    return SizedBox(
      width: size,
      height: size,
      child: CustomPaint(
        painter: _ProgressRingPainter(
          progress: progress,
          strokeWidth: strokeWidth,
          color: theme.colorScheme.primary,
        ),
        child: Center(
          child: Text(
            centerText ?? '${(progress * 100).toInt()}%',
            style: theme.textTheme.labelLarge,
          ),
        ),
      ),
    );
  }
}
```

The `_ProgressRingPainter` draws:
1. Background circle (gray)
2. Progress arc (colored, based on progress value)
3. Optional center text

### Q78: How does PulseQuickLog work?
**A:** `PulseQuickLog` provides one-tap logging for wellness data:

```dart
class PulseQuickLog extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Obx(() {
      return Wrap(
        spacing: AppSpacing.sm,
        children: [
          // Sleep/Wake button
          if (asleep == null)
            _LogButton(icon: Icons.bedtime, label: 'Sleep', onTap: () => c.startSleep())
          else
            _LogButton(icon: Icons.wb_sunny, label: 'Wake', emphasised: true),
          
          // Quick log buttons
          _LogButton(icon: Icons.water_drop, label: 'Water', onTap: () => c.log(LifeEventType.water)),
          _LogButton(icon: Icons.restaurant, label: 'Meal', onTap: () => c.log(LifeEventType.meal)),
          
          // Choice buttons (popup menus)
          _ChoiceButton(
            icon: Icons.sentiment_satisfied,
            label: 'Mood',
            choices: {'Good': 4, 'Okay': 3, 'Low': 2},
            onChoose: (score) => c.log(LifeEventType.mood, metadata: {'score': score}),
          ),
        ],
      );
    });
  }
}
```

**Key Features:**
- No forms - instant logging
- Undo confirmation after each log
- Sleep tracking (tracks duration between sleep/wake)
- Choice buttons for Mood and Energy

### Q79: How does StatTile work?
**A:** `StatTile` displays a quick statistic with icon and value:

```dart
class StatTile extends StatelessWidget {
  const StatTile({
    required this.icon,
    required this.label,
    required this.value,
    this.color,
  });

  @override
  Widget build(BuildContext context) {
    return Container(
      padding: EdgeInsets.all(AppSpacing.md),
      decoration: BoxDecoration(
        color: theme.colorScheme.surface,
        borderRadius: AppRadius.cardRadius,
        border: Border.all(color: hairline, width: 1),
      ),
      child: Row(
        children: [
          Icon(icon, color: color ?? theme.colorScheme.primary),
          SizedBox(width: AppSpacing.md),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(value, style: theme.textTheme.titleLarge),
                Text(label, style: theme.textTheme.bodySmall),
              ],
            ),
          ),
        ],
      ),
    );
  }
}
```

---

## Summary for Review

### Key Talking Points:

1. **Architecture**: Clean Architecture with Domain, Data, and Presentation layers
2. **State Management**: GetX for simple state, BLoC reserved for AI features
3. **Offline-First**: SQLite primary, Firebase secondary, background sync
4. **Firebase**: Auth, Firestore, FCM, Analytics, Crashlytics
5. **Features**: Tasks, Calendar, Notes, Habits, Finance, Wellness, Pulse
6. **Design**: Material 3, anti-slop aesthetic, responsive layouts
7. **Widgets**: Reusable components (TaskCard, EventBlock, ModuleTile, etc.)
8. **Notifications**: Local scheduled reminders + FCM push
9. **Theme**: Light/dark mode with semantic color system
10. **Testing**: 97+ tests passing (unit, widget, model)

### Demo Flow:
1. Show login page with validation
2. Demonstrate task creation with confirmation
3. Show calendar month/week/day views
4. Display habit heatmap
5. Trigger notification demo
6. Show web version in Chrome

This Q&A covers all aspects of the OmniLife project for your review preparation!
