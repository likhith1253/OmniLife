
# OmniLife Project - Comprehensive Q&A Guide

Complete question-and-answer style explanation of the entire OmniLife project covering architecture, Flutter basics, GetX, BLoC, all features, widgets, and code implementation details.

## Table of Contents
1. [Project Overview](#1-project-overview)
2. [Flutter & Dart Basics](#2-flutter--dart-basics)
3. [Clean Architecture Pattern](#3-clean-architecture-pattern)
4. [GetX State Management](#4-getx-state-management)
5. [BLoC Pattern](#5-bloc-pattern)
6. [Firebase Integration](#6-firebase-integration)
7. [Offline-First Sync Engine](#7-offline-first-sync-engine)
8. [SQLite Database](#8-sqlite-database)
9. [Theme System](#9-theme-system)
10. [Authentication Feature](#10-authentication-feature)
11. [Tasks Feature](#11-tasks-feature)
12. [Calendar Feature](#12-calendar-feature)
13. [Notes Feature](#13-notes-feature)
14. [Habits & Goals Feature](#14-habits--goals-feature)
15. [Finance Feature](#15-finance-feature)
16. [Wellness Feature](#16-wellness-feature)
17. [Pulse (Life Log) Feature](#17-pulse-life-log-feature)
18. [Focus Mode Feature](#18-focus-mode-feature)
19. [Maps & Places Feature](#19-maps--places-feature)
20. [Sensors Feature](#20-sensors-feature)
21. [Reminders Feature](#21-reminders-feature)
22. [Assistant (AI) Feature](#22-assistant-ai-feature)
23. [Notifications Feature](#23-notifications-feature)
24. [Core Services Deep Dive](#24-core-services-deep-dive)
25. [Widget Library Overview](#25-widget-library-overview)

---

## 1. Project Overview

### Q1: What is OmniLife?
**A:** OmniLife is a comprehensive personal life organizer built with Flutter for the course **23CSE465 Mobile Application Development**. It integrates 13 core life management domains into a single application with offline-first architecture. The app allows users to manage tasks, calendar events, notes, habits, finances, wellness data, and more, with optional AI assistance powered by Groq.

### Q2: What are the key features of OmniLife?
**A:**
- **Authentication**: Email/password and Google Sign-In with session persistence
- **Tasks**: Create, edit, complete, and delete tasks with priority, due dates, search, and filters
- **Calendar**: Month, week, and day views with events, color tags, and task agenda integration
- **Notes**: Markdown-enabled notes with tags, categories, pin/archive, ranked search, and checklist-to-task conversion
- **Habits & Goals**: Daily/weekly habits with streak tracking, 365-day heatmap, and goals with milestones
- **Finance**: Income/expense logging with categories, budgets, charts, receipt uploads, and PDF reports
- **Wellness**: Daily tracking of water intake, sleep, workouts, and mood
- **Pulse**: One-tap life log (sleep, wake, water, meals) with timeline, day replay, momentum score, and pattern insights
- **Focus Mode**: Pomodoro timer (25/5 default) with ambient audio/video and session history
- **Maps & Places**: Map with saved places and foreground geofencing
- **Sensors**: Pedometer step counter with hourly/daily totals and flip detector
- **Reminders**: One-off, daily, and weekly reminders with local notifications, snooze, and mark-done actions
- **Assistant**: Chat and voice interface powered by Groq AI for creating tasks, events, notes, reminders, and expenses
- **Next Up**: Dashboard hints for upcoming tasks and events
- **Notifications**: In-app notification center, local notifications, and FCM push messaging

### Q3: What technologies are used in OmniLife?
**A:**
- **Framework**: Flutter (Android primary, web supported, Windows for development)
- **Language**: Dart SDK 3.13+
- **State Management**: GetX (routing, DI, controllers) + BLoC (authentication flow)
- **Backend**: Firebase (Auth, Firestore, Storage, FCM, Analytics, Crashlytics)
- **Local Storage**: SQLite (sqflite) on devices, in-memory/key-value storage on web
- **Architecture**: Clean Architecture (Domain, Data, Presentation layers)
- **AI**: Groq API for assistant (optional, requires API key)

### Q4: What is the project structure?
**A:**
```
lib/
├── main.dart                 # App bootstrap (Firebase, notifications, sync)
├── firebase_options.dart     # FlutterFire configuration
├── app/                      # App-level configuration
│   ├── app.dart             # GetMaterialApp setup
│   ├── routes/              # Navigation routes and auth guard
│   └── theme/               # Design system (colors, typography, spacing)
├── core/                     # Cross-cutting concerns
│   ├── bindings/            # GetX dependency injection per feature
│   ├── config/              # Environment values (dart-define)
│   ├── errors/              # Failures and user-facing messages
│   ├── firebase/            # Firebase initialization helpers
│   ├── services/            # Shared services (notifications, push, location, sensors, PDF, analytics)
│   ├── sync/                # Offline sync engine, outbox, connectivity monitor
│   └── utils/               # Validators and helpers
├── data/                     # Data layer
│   ├── assistant/           # Groq provider, chat history, memory store
│   ├── datasources/         # Local (SQLite) and Remote (Firebase) data sources
│   ├── focus/ insights/     # Focus and insight data helpers
│   ├── models/              # Data models with SQLite/Firestore serialization
│   └── repositories/        # Repository implementations
├── domain/                   # Domain layer (framework-agnostic)
│   ├── assistant/           # AI engine, tool definitions, reminder time rules
│   ├── entities/            # Business entities
│   ├── repositories/        # Repository interfaces
│   ├── usecases/            # Business logic use cases
│   ├── services/            # Domain services (streak maths, finance maths, Pulse analysis)
│   ├── focus/               # Focus timer and state management
│   └── insights/            # Linked advisor and next-up logic
└── presentation/            # Presentation layer
    ├── assistant/           # Assistant tools and voice service
    ├── bloc/                # Authentication BLoC
    ├── controllers/         # GetX controllers per feature
    ├── pages/               # Screen widgets (one folder per module)
    └── widgets/             # Shared and per-module reusable widgets
```

### Q5: What is the target platform?
**A:** Primary target is Android. Web is supported for development and demonstration. Windows desktop is used only for development, not deployment.

---

## 2. Flutter & Dart Basics

### Q6: What is Flutter?
**A:** Flutter is Google's UI toolkit for building natively compiled applications for mobile, web, and desktop from a single codebase. It uses the Dart programming language and renders widgets directly to the canvas, providing high performance and a native-like feel.

### Q7: What is a Widget in Flutter?
**A:** In Flutter, everything is a widget. A widget is an immutable description of part of the user interface. Widgets are composed together to build complex UIs. There are two main types:
- **StatelessWidget**: Immutable, doesn't change over time (e.g., Text, Icon)
- **StatefulWidget**: Has mutable state that can change (e.g., TextField, Checkbox)

### Q8: What is the build method?
**A:** The `build()` method is called by Flutter to render the widget. It takes a `BuildContext` and returns a `Widget`. The framework calls this method whenever the widget needs to be rebuilt (e.g., when state changes or parent widget rebuilds).

### Q9: What is the widget tree?
**A:** The widget tree is the hierarchical structure of widgets that make up your app. When you compose widgets together, you're building a tree. Flutter's framework efficiently rebuilds only the parts of the tree that need updating.

### Q10: What is Dart?
**A:** Dart is the programming language used by Flutter. It's an object-oriented, class-based language with C-style syntax. Dart supports ahead-of-time (AOT) compilation for native performance and just-in-time (JIT) compilation for fast development cycles.

---

## 3. Clean Architecture Pattern

### Q11: What is Clean Architecture?
**A:** Clean Architecture is a software design pattern that separates concerns into distinct layers. In OmniLife, we have four main layers:

1. **Presentation Layer**: UI components, state management (GetX controllers), and user interaction
2. **Domain Layer**: Business logic, entities, repository interfaces, use cases, and domain services
3. **Data Layer**: Data persistence (SQLite, Firebase), data models, data sources, and repository implementations
4. **Core Layer**: Cross-cutting concerns (sync engine, notifications, location services, bindings, config)

**Key Principle:** Dependencies point inward - Presentation depends on Domain, Data depends on Domain. Domain has no dependencies on other layers (framework-agnostic).

### Q12: Why use Clean Architecture?
**A:**
- **Separation of Concerns**: Each layer has a single responsibility
- **Testability**: Business logic can be tested without UI or database dependencies
- **Maintainability**: Changes to one layer don't affect others
- **Flexibility**: Can swap implementations (e.g., change database without changing business logic)
- **Scalability**: Easy to add new features without disrupting existing code

### Q13: How does data flow in Clean Architecture?
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

### Q14: What files implement each layer?
**A:**
- **Presentation**: <ref_file file="D:\OmniLife\lib\presentation\controllers\auth_controller.dart" />, <ref_file file="D:\OmniLife\lib\presentation\pages\tasks\tasks_page.dart" />, <ref_file file="D:\OmniLife\lib\presentation\widgets\task_card.dart" />
- **Domain**: <ref_file file="D:\OmniLife\lib\domain\entities\task.dart" />, <ref_file file="D:\OmniLife\lib\domain\repositories\task_repository.dart" />, <ref_file file="D:\OmniLife\lib\domain\usecases\habits\habit_streaks.dart" />
- **Data**: <ref_file file="D:\OmniLife\lib\data\models\task_model.dart" />, <ref_file file="D:\OmniLife\lib\data\datasources\local\local_task_data_source.dart" />, <ref_file file="D:\OmniLife\lib\data\repositories\task_repository_impl.dart" />
- **Core**: <ref_file file="D:\OmniLife\lib\core\sync\sync_engine.dart" />, <ref_file file="D:\OmniLife\lib\core\services\notification_service.dart" />, <ref_file file="D:\OmniLife\lib\core\bindings\initial_binding.dart" />

---

## 4. GetX State Management

### Q15: What is GetX?
**A:** GetX is a lightweight Flutter framework that provides:
- **State Management**: Reactive state with observables (.obs)
- **Navigation**: Simplified routing and navigation with named routes
- **Dependency Injection**: Service locator pattern for managing dependencies
- **Utils**: Internationalization, storage, and other utilities

### Q16: How does GetX state management work?
**A:** GetX uses reactive programming with observables (`.obs`). When an observable changes, all widgets listening to it (via `Obx`) automatically rebuild. This eliminates the need for `setState()` and boilerplate code.

### Q17: What is a GetXController?
**A:** `GetXController` is a base class for managing state. It has lifecycle methods:
- `onInit()`: Called when controller is created
- `onReady()`: Called after onInit
- `onClose()`: Called when controller is disposed

Controllers manage state for features and expose reactive observables for the UI.

### Q18: What is the difference between Get.put, Get.lazyPut, and Get.find?
**A:**
- **Get.put()**: Creates instance immediately and keeps it alive
- **Get.lazyPut()**: Creates instance only when first requested (lazy initialization)
- **Get.find()**: Retrieves an already registered instance

### Q19: What does permanent: true mean?
**A:** `permanent: true` means the controller will never be disposed, even when navigating away. This is used for controllers that need to persist across the app (e.g., AuthController, TaskController).

### Q20: What does fenix: true mean?
**A:** `fenix: true` means the instance can be recreated after being disposed. Like a phoenix rising from ashes, it can be brought back to life when needed again. This is useful for controllers that should persist but can be recreated if memory is needed.

### Q21: How does GetX navigation work?
**A:** GetX provides named routes for navigation:
- `Get.toNamed(AppRoutes.tasks)`: Push new route
- `Get.offNamed(AppRoutes.calendar)`: Replace current route
- `Get.offAllNamed(AppRoutes.login)`: Clear stack and navigate
- `Get.back()`: Go back

Routes are defined in <ref_file file="D:\OmniLife\lib\app\routes\app_routes.dart" /> and registered in <ref_file file="D:\OmniLife\lib\app\routes\app_pages.dart" />.

### Q22: What is BindingsBuilder?
**A:** `BindingsBuilder` is used to register dependencies for a specific route. When navigating to that route, the bindings are executed to set up the required controllers and repositories. This ensures dependencies are available when the page loads.

### Q23: Where are GetX bindings defined?
**A:** Bindings are defined in <ref_file file="D:\OmniLife\lib\core\bindings\initial_binding.dart" /> for app-wide dependencies, and in route-specific bindings in <ref_file file="D:\OmniLife\lib\app\routes\app_pages.dart" /> for per-route dependencies.

---

## 5. BLoC Pattern

### Q24: What is BLoC?
**A:** BLoC (Business Logic Component) is a state management pattern that uses streams to manage state. It follows an event-driven architecture:
- **Events**: User actions (e.g., "LoadTasks", "AddTask")
- **States**: App states (e.g., "Loading", "Loaded", "Error")
- **Streams**: Unidirectional data flow

### Q25: Why does OmniLife use both GetX and BLoC?
**A:** OmniLife uses a hybrid approach:
- **GetX**: For simple, straightforward state (tasks, calendar, habits, etc.) - less boilerplate, faster development
- **BLoC**: Reserved for authentication flow - better for event-driven workflows, more testable, standard pattern for auth

### Q26: How is BLoC used in OmniLife?
**A:** BLoC is used only for authentication in <ref_file file="D:\OmniLife\lib\presentation\bloc\app_bloc_observer.dart" />. The `AppBlocObserver` logs errors for debugging. Most state management uses GetX for simplicity.

### Q27: What is BlocObserver?
**A:** `BlocObserver` is used to observe all BLoC instances for logging, analytics, or error handling. OmniLife has `AppBlocObserver` that logs errors to the console for debugging.

---

## 6. Firebase Integration

### Q28: How is Firebase initialized in OmniLife?
**A:** Firebase is initialized in <ref_file file="D:\OmniLife\lib\main.dart" /> before the app starts:
1. `WidgetsFlutterBinding.ensureInitialized()` initializes Flutter bindings
2. `initializeFirebase()` calls `Firebase.initializeApp()` with platform-specific options from <ref_file file="D:\OmniLife\lib\firebase_options.dart" />
3. `initializeTelemetry()` sets up Analytics, Crashlytics, and FCM background handler
4. `Bloc.observer = AppBlocObserver()` sets up BLoC error logging
5. `runApp(OmniLifeApp())` starts the Flutter app

### Q29: What Firebase services are used?
**A:**
- **Firebase Auth**: Email/password and Google Sign-In authentication
- **Cloud Firestore**: NoSQL database for cloud sync
- **Firebase Storage**: File storage for receipts and images
- **FCM (Firebase Cloud Messaging)**: Push notifications
- **Firebase Analytics**: Usage analytics and event tracking
- **Firebase Crashlytics**: Crash reporting

### Q30: How does Firebase Auth work?
**A:** OmniLife uses the `AuthRepository` to interface with Firebase Auth. The repository is implemented in <ref_file file="D:\OmniLife\lib\data\datasources\remote\firebase_auth_datasource.dart" /> and wrapped by <ref_file file="D:\OmniLife\lib\data\repositories\auth_repository_impl.dart" />. It supports:
- Email/password registration and login
- Google Sign-In
- Password reset
- Session persistence
- Auth state changes stream

### Q31: How does Firestore data structure work?
**A:** OmniLife uses a user-scoped Firestore structure:
```
users/{uid}
  ├── tasks/{taskId}
  ├── events/{eventId}
  ├── notes/{noteId}
  ├── habits/{habitId}
  ├── habit_logs/{logId}
  ├── expenses/{expenseId}
  ├── wellness/{date}
  ├── focus_sessions/{sessionId}
  ├── locations/{locationId}
  ├── sensor_daily/{date}
  ├── reminders/{reminderId}
  ├── devices/{deviceId}
  ├── settings/{settingId}
  ├── life_events/{eventId}
  └── conversations/{conversationId}/messages/{messageId}
```

This ensures each user's data is isolated and secure.

### Q32: What is UserScopedFirestoreDataSource?
**A:** It's a generic data source in <ref_file file="D:\OmniLife\lib\data\datasources\remote\user_scoped_firestore_datasource.dart" /> that handles CRUD operations for any user-scoped Firestore collection. It abstracts away Firestore complexity and provides a simple interface for repositories.

### Q33: How are Firebase security rules configured?
**A:** Security rules are defined in <ref_file file="D:\OmniLife\firestore.rules" />. The rules follow an ownership model:
- Everything lives under `users/{uid}/...`
- Only the signed-in user whose uid matches can read/write their data
- Additional validation: userId/uid fields must match caller, documents must have ≤60 fields
- All other paths are denied by default

---

## 7. Offline-First Sync Engine

### Q34: What is offline-first architecture?
**A:** Offline-first means the app works without an internet connection. All data is stored locally (SQLite) first, then synced to the cloud (Firebase) when connectivity is available. Users can create, edit, and delete data offline, and changes will sync when they come back online.

### Q35: How does OmniLife implement offline-first?
**A:**
1. **All writes go to SQLite first** - data is saved locally immediately
2. **UI updates optimistically** - user sees instant feedback
3. **Background sync to Firebase** - data synced when network available
4. **Read from local cache** - always fast, no network dependency
5. **Sync queue** - failed syncs are retried with exponential backoff
6. **Conflict resolution** - last-writer-wins on `updatedAt` timestamp

### Q36: What is the sync engine?
**A:** The sync engine (<ref_file file="D:\OmniLife\lib\core\sync\sync_engine.dart" />) manages synchronization between local SQLite and remote Firebase. It:
- Monitors connectivity status via <ref_file file="D:\OmniLife\lib\core\sync\connectivity_monitor.dart" />
- Queues sync operations in the outbox
- Retries failed operations with exponential backoff
- Handles conflicts by last-writer-wins
- Coalesces repeated writes to the same document
- Processes operations in batches for efficiency

### Q37: How does the sync queue work?
**A:** The sync queue (SyncOutbox) stores operations that need to be synced. It's implemented in <ref_file file="D:\OmniLife\lib\core\sync\sqlite_sync_outbox.dart" /> for mobile and <ref_file file="D:\OmniLife\lib\core\sync\key_value_sync_outbox.dart" /> for web. Each operation contains:
- Entity type (task, event, note, etc.)
- Entity ID
- Operation type (create, update, delete)
- Payload (data)
- Created timestamp
- Retry count
- Next attempt timestamp

When network is available, the sync engine processes the queue and removes successful operations.

### Q38: How does conflict resolution work?
**A:** Conflicts are resolved by last-writer-wins on the `updatedAt` timestamp. When syncing, the engine compares the local `updatedAt` with the remote `updatedAt`:
- If local is newer: Write to remote
- If remote is newer: Skip the local change (it's outdated)
- If timestamps are equal: Use JSON comparison to detect actual differences

This ensures no data loss and the most recent change always wins.

### Q39: What is the SyncOperation?
**A:** SyncOperation (<ref_file file="D:\OmniLife\lib\core\sync\sync_operation.dart" />) represents a single sync operation. It contains:
- `id`: Composite key `collection/docId` (ensures coalescing)
- `uid`: User who owns the data
- `collection`: Firestore collection name
- `docId`: Document ID
- `type`: Operation type (set or delete)
- `data`: Document data (for set operations)
- `updatedAtMs`: Logical time of the change
- `createdAtMs`: When the operation was queued
- `attempts`: Retry count
- `nextAttemptAtMs`: When to retry
- `lastError`: Last error message
- `state`: pending or failed

### Q40: How does the remote writer work?
**A:** The remote writer (<ref_file file="D:\OmniLife\lib\core\sync\firestore_remote_writer.dart" />) writes operations to Firestore using transactions. It:
- Builds document references for `users/{uid}/{collection}/{docId}`
- Implements last-writer-wins by comparing `updatedAt` in a transaction
- Supports batch operations (multiple documents in one transaction)
- Handles timeout errors gracefully
- Returns results indicating success, skip (remote newer), or failure

---

## 8. SQLite Database

### Q41: What is SQLite?
**A:** SQLite is an embedded SQL database engine. It's stored as a single file on the device and requires no separate server process. OmniLife uses SQLite for offline data storage via the `sqflite` package.

### Q42: How is the database initialized?
**A:** The database is managed by <ref_file file="D:\OmniLife\lib\data\datasources\local\app_database.dart" />. It:
- Uses a singleton pattern
- Opens/creates the database file `omnilife.db`
- Creates tables on first launch
- Handles schema migrations with versioning
- Supports foreign key constraints

### Q43: What tables exist in the database?
**A:**
- `tasks`: Task data
- `calendar_events`: Calendar events with index on (user_id, start_at)
- `notes`: Notes with markdown content
- `habits`: Habit definitions
- `habit_logs`: Habit completion logs with foreign key to habits
- `finance`: Financial transactions
- `wellness_logs`: Wellness tracking data
- `sync_outbox`: Sync queue (managed separately)

### Q44: How does data serialization work with SQLite?
**A:** SQLite doesn't have native support for Dart objects, so we convert:
- **DateTime** → `int` (milliseconds since epoch)
- **bool** → `int` (0 or 1)
- **enum** → `String` (enum name)
- **List** → JSON string
- **Map** → JSON string

Data models handle this conversion in `toMap()` and `fromMap()` methods.

### Q45: What is the schema version?
**A:** The current schema version is 7. Each time the schema changes, the version is incremented and the `onUpgrade` callback handles the migration. This ensures users' databases are upgraded smoothly without data loss.

---

## 9. Theme System

### Q46: How does the theme system work?
**A:** OmniLife uses Material 3 design system with custom themes defined in <ref_file file="D:\OmniLife\lib\app\theme\app_theme.dart" />. There are light and dark variants. The theme is managed by <ref_file file="D:\OmniLife\lib\presentation\controllers\app_controller.dart" /> and users can toggle between light/dark mode.

### Q47: What is the design philosophy?
**A:** OmniLife follows an "anti-slop" aesthetic:
- No purple-to-pink gradients
- Glassmorphism only on auth screens
- 1px hairline borders instead of thick borders
- Solid surfaces with subtle shadows
- Tabular figures for numbers and dates
- Consistent spacing via <ref_file file="D:\OmniLife\lib\app\theme\app_spacing.dart" />
- Consistent border radius via <ref_file file="D:\OmniLife\lib\app\theme\app_radius.dart" />

### Q48: How does theme switching work?
**A:** The `AppController` manages the theme mode (system, light, dark). The app uses `Obx` to react to theme changes. When the user toggles the theme, the entire app rebuilds with the new theme.

### Q49: What are semantic colors?
**A:** Semantic colors are named colors that have meaning in the app context, separate from the Material color scheme. They're defined in <ref_file file="D:\OmniLife\lib\app\theme\app_semantic_colors.dart" /> and used for specific features (e.g., module colors, status colors). This allows theming to be consistent across the app.

### Q50: What color system is used?
**A:** OmniLife uses a custom color system defined in <ref_file file="D:\OmniLife\lib\app\theme\app_colors.dart" /> with:
- Primary, secondary, tertiary colors
- Surface colors (background, surface, surface variants)
- Error, warning, success colors
- Neutral colors for text and borders

---

## 10. Authentication Feature

### Q51: How does authentication work?
**A:** Authentication is managed by <ref_file file="D:\OmniLife\lib\presentation\controllers\auth_controller.dart" />. It:
- Observes Firebase Auth state changes
- Manages login, register, logout, and password reset
- Handles Google Sign-In
- Redirects users based on auth status (via <ref_file file="D:\OmniLife\lib\app\routes\auth_guard.dart" />)
- Persists session across app restarts

### Q52: What authentication methods are supported?
**A:**
- Email and password registration
- Email and password login
- Google Sign-In
- Password reset via email

### Q53: How does the auth guard work?
**A:** The auth guard (<ref_file file="D:\OmniLife\lib\app\routes\auth_guard.dart" />) protects routes:
- Signed-out users trying to access protected routes are redirected to login
- The pending route is remembered and restored after sign-in
- Signed-in users trying to access login/register are redirected to dashboard
- Auth state changes trigger automatic redirects

### Q54: How is the auth UI designed?
**A:** The auth screens (<ref_file file="D:\OmniLife\lib\presentation\pages\auth\login_page.dart" />, <ref_file file="D:\OmniLife\lib\presentation\pages\auth\register_page.dart" />) use a custom design with:
- Glassmorphism background
- Centered card with logo
- Social auth button for Google Sign-In
- Email/password fields with validation
- Forgot password link
- Clean, modern aesthetic

### Q55: How does session persistence work?
**A:** Firebase Auth automatically persists the session. When the app restarts, the auth state stream emits the current user (if still signed in). The `AuthController` observes this and redirects to the dashboard or login accordingly.

---

## 11. Tasks Feature

### Q56: How does the Tasks feature work?
**A:** The Tasks feature allows users to create, complete, and manage tasks. It consists of:
- **Task Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\task.dart" /> - represents a task with title, description, priority, due date
- **TaskController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\task_controller.dart" /> - manages task state and operations
- **TasksPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\tasks\tasks_page.dart" /> - UI for viewing and managing tasks
- **TaskCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\task_card.dart" /> - widget for displaying a single task
- **CreateTaskSheet**: <ref_file file="D:\OmniLife\lib\presentation\widgets\create_task_sheet.dart" /> - bottom sheet for creating/editing tasks

### Q57: What task features are available?
**A:**
- Create, edit, complete, and delete tasks
- Priority levels (low, medium, high)
- Due dates with smart formatting
- Search by title and description
- Filter by (all, today, upcoming, completed)
- Filter by priority
- Completion metrics and statistics
- Optimistic UI updates

### Q58: How is a task created?
**A:**
1. User taps "New Task" button
2. `CreateTaskSheet.show()` opens the bottom sheet
3. User fills form and taps "Create Task"
4. Form is validated
5. Calls `TaskController.createTask()`
6. Controller adds task to list optimistically
7. Saves to SQLite via `LocalTaskDataSource`
8. Syncs to Firestore in background via sync engine
9. Shows confirmation
10. Closes sheet

### Q59: What is optimistic UI update?
**A:** Optimistic UI update means the UI updates immediately assuming the operation will succeed, before waiting for the actual save to complete. If the save fails, the UI reverts to the previous state. This provides a fast, snappy feel.

### Q60: How do task filters work?
**A:** The `TaskController` has a `filteredTasks` getter that applies filters:
- Priority filter (high, medium, low, or all)
- Search query (matches title or description)
- Tab filter (all, today, upcoming, completed)

The filters are applied in sequence, and the result is sorted by creation date.

### Q61: How does TaskCard widget work?
**A:** `TaskCard` displays a single task with:
- Circular checkbox (animated on toggle)
- Title and description
- Priority indicator (colored dot)
- Due date with smart formatting ("Today", "Tomorrow", "Jan 15")
- Delete button
- Tap to edit

---

## 12. Calendar Feature

### Q62: How does the Calendar feature work?
**A:** The Calendar feature provides multi-view scheduling:
- **CalendarEvent Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\calendar_event.dart" /> - represents events with time ranges
- **CalendarController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\calendar_controller.dart" /> - manages calendar state
- **CalendarPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\calendar\calendar_page.dart" /> - UI with month/week/day views
- **EventBlock**: <ref_file file="D:\OmniLife\lib\presentation\widgets\calendar\event_block.dart" /> - widget for displaying events in timeline
- **CreateEventSheet**: <ref_file file="D:\OmniLife\lib\presentation\widgets\calendar\create_event_sheet.dart" /> - bottom sheet for creating events

### Q63: What calendar views are available?
**A:**
- **Month View**: Grid of days with event dots, month navigation
- **Week View**: 7-day timeline with event blocks, time gutter
- **Day View**: Single day detailed timeline
- **Agenda View**: List of events and tasks for selected day

### Q64: How is the calendar grid built?
**A:** The calendar grid is built by calculating the days needed to fill the month:
1. Get first day of month
2. Calculate which weekday it falls on
3. Add padding days from previous month
4. Add all days of current month
5. Add padding days from next month to complete grid
6. Render as a 7-column grid (Sunday-Saturday)

### Q65: How do calendar events work?
**A:** Calendar events are similar to tasks but with time ranges:
- Start and end times
- All-day flag
- Color tags for categorization
- Type (event or focus block)
- Linked task ID (optional)

### Q66: What is the difference between events and focus blocks?
**A:**
- **Events**: Regular calendar events (meetings, appointments)
- **Focus Blocks**: Dedicated time blocks for deep work, displayed with a bolt icon, integrated with Focus Mode

### Q67: How does the timeline view work?
**A:** The timeline view uses a positioning algorithm to place events side-by-side when they overlap. The algorithm:
1. Sorts events by start time
2. Groups overlapping events into columns
3. Calculates position (left, width, top, height) for each event
4. Renders using `Positioned` widgets

### Q68: How does the agenda view merge events and tasks?
**A:** The `GetAgendaForRange` use case (<ref_file file="D:\OmniLife\lib\domain\usecases\calendar\get_agenda_for_range.dart" />) merges events and tasks with due dates into a single timeline. It:
1. Filters events in the date range
2. Filters tasks with due dates in the range
3. Combines into a single list
4. Sorts by time
5. Returns as agenda items

---

## 13. Notes Feature

### Q69: How does the Notes feature work?
**A:** The Notes feature provides markdown-enabled note-taking:
- **Note Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\note.dart" /> - represents a note with markdown content
- **NotesController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\notes_controller.dart" /> - manages notes state
- **NotesPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\notes\notes_page.dart" /> - UI for viewing and managing notes
- **NoteCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\notes\note_card.dart" /> - widget for displaying a note
- **MarkdownNoteView**: <ref_file file="D:\OmniLife\lib\presentation\widgets\notes\markdown_note_view.dart" /> - widget for rendering markdown

### Q70: What note features are available?
**A:**
- Markdown content with live rendering
- Tags and categories
- Pin and archive
- Ranked search (title matches > content matches > tag matches)
- Checklist items that can be converted to tasks
- Create, edit, delete notes

### Q71: How does markdown rendering work?
**A:** OmniLife uses `flutter_markdown_plus` to render markdown content. Custom styling is applied via `MarkdownStyleSheet` to match the app's theme. The markdown parser supports:
- Headers, bold, italic
- Code blocks with syntax highlighting
- Blockquotes
- Lists (ordered and unordered)
- Checklists (interactive)

### Q72: How do checklists work in notes?
**A:** Notes support task lists using markdown syntax:
```markdown
- [ ] Buy groceries
- [x] Call mom
- [ ] Finish report
```

The `ChecklistParser` (<ref_file file="D:\OmniLife\lib\domain\usecases\notes\checklist_parser.dart" />) parses these lines and the `MarkdownNoteView` renders them as interactive checkboxes. Users can tap checkboxes to toggle completion.

### Q73: How does note search work?
**A:** NotesController has a debounced search with ranking. The `NoteSearchRanker` (<ref_file file="D:\OmniLife\lib\domain\usecases\notes\note_search_ranker.dart" />) ranks results by:
- Title matches (highest priority)
- Content matches
- Tag matches
- Relevance score

The search is debounced to avoid unnecessary queries while typing.

### Q74: How does converting checklist to tasks work?
**A:** Users can convert unchecked checklist items into tasks via the `ConvertChecklistToTasks` use case (<ref_file file="D:\OmniLife\lib\domain\usecases\notes\convert_checklist_to_tasks.dart" />). It:
1. Parses checklist items from note
2. Filters unchecked items
3. Deduplicates against existing tasks
4. Creates new tasks for each unique item
5. Returns count of created tasks

---

## 14. Habits & Goals Feature

### Q75: How does the Habits feature work?
**A:** The Habits feature tracks recurring activities:
- **Habit Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\habit.dart" /> - represents a habit
- **HabitLog Entity**: Completion records for specific dates
- **HabitsController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\habits_controller.dart" /> - manages habits state
- **HabitsPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\habits\habits_page.dart" /> - UI for viewing habits
- **HabitHeatmap**: <ref_file file="D:\OmniLife\lib\presentation\widgets\habits\habit_heatmap.dart" /> - GitHub-style heatmap
- **HabitCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\habits\habit_card.dart" /> - widget for displaying a habit

### Q76: What habit features are available?
**A:**
- Daily, weekly, or custom frequency habits
- Target days per week
- Specific days (e.g., Mon, Wed, Fri)
- Check-in with undo
- Current and longest streaks
- 365-day heatmap visualization
- Goals with milestones linked to habits

### Q77: How are habit streaks calculated?
**A:** The `HabitStreaks` use case (<ref_file file="D:\OmniLife\lib\domain\usecases\habits\habit_streaks.dart" />) calculates streaks based on:
- Frequency (daily, weekly, custom)
- Target days per week
- Specific days
- Completion history

The algorithm converts dates to day numbers (days since Unix epoch) to avoid DST issues and walks through the completion history to find consecutive runs.

### Q78: How does the habit heatmap work?
**A:** The `HabitHeatmap` widget displays a GitHub-style contribution graph:
- 365-day grid (one column per week, 7 rows)
- Monday-first columns
- Month labels on top
- Day labels on left
- Color intensity based on completion
- Tap any day to toggle completion

The `HeatmapGrid` (<ref_file file="D:\OmniLife\lib\domain\usecases\habits\heatmap_grid.dart" />) builds the grid structure.

### Q79: How does habit completion work?
**A:** Toggling a habit completion:
1. User taps a day in the heatmap
2. `HabitsController.toggleHabitLog()` is called
3. Optimistic update: add/remove date from logs
4. Recalculate streaks in memory
5. Save to database
6. If save fails, revert optimistic update

### Q80: How do goals work?
**A:** Goals are linked to habits and have milestones. Users can:
- Create goals with descriptions
- Link habits to goals
- Set milestones
- Track progress based on habit completion

The GoalsController (<ref_file file="D:\OmniLife\lib\presentation\controllers\goals_controller.dart" />) manages goal state.

---

## 15. Finance Feature

### Q81: How does the Finance feature work?
**A:** The Finance feature tracks income and expenses:
- **FinanceTransaction Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\finance_transaction.dart" /> - represents a transaction
- **FinanceController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\finance_controller.dart" /> - manages finance state
- **FinancePage**: <ref_file file="D:\OmniLife\lib\presentation\pages\finance\finance_page.dart" /> - UI for viewing finances
- **FinanceCharts**: <ref_file file="D:\OmniLife\lib\presentation\pages\finance\finance_charts.dart" /> - charts widget
- **FinanceMath**: <ref_file file="D:\OmniLife\lib\domain\services\finance_math.dart" /> - business logic for calculations

### Q82: What finance features are available?
**A:**
- Income and expense logging
- Categories for transactions
- Per-category budgets
- Charts (spending by category, six-month income vs spending)
- Receipt photo upload to Firebase Storage
- PDF monthly report generation
- Written suggestions based on spending patterns

### Q83: How are finance charts calculated?
**A:** The `FinanceMath` service provides:
- `spendByCategory()`: Groups expenses by category, calculates share of total, returns sorted list
- `monthlyTotals()`: Calculates income vs spending for N months, includes months with zero transactions
- `BudgetStatus`: Compares spending against budget, returns level (noBudget, onTrack, nearLimit, over)

### Q84: How does receipt upload work?
**A:** Users can upload receipt photos via the `ReceiptUploader` service (<ref_file file="D:\OmniLife\lib\core\services\receipt_uploader.dart" />). It:
1. Opens image picker
2. Compresses image
3. Uploads to Firebase Storage
4. Returns download URL
5. URL is stored with the transaction

### Q85: How are PDF reports generated?
**A:** The `FinanceReportPdf` service (<ref_file file="D:\OmniLife\lib\core\services\finance_report_pdf.dart" />) generates monthly reports using the `pdf` package. It:
1. Fetches transactions for the month
2. Calculates totals and breakdowns
3. Generates PDF with tables and charts
4. Allows sharing or saving

---

## 16. Wellness Feature

### Q86: How does the Wellness feature work?
**A:** The Wellness feature tracks daily health metrics:
- **WellnessLog Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\wellness_log.dart" /> - represents daily wellness data
- **WellnessController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\wellness_controller.dart" /> - manages wellness state
- **WellnessPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\wellness\wellness_page.dart" /> - UI for tracking wellness

### Q87: What wellness metrics are tracked?
**A:**
- Water intake (in ml)
- Sleep duration (in hours)
- Sleep quality (rating)
- Workout duration (in minutes)
- Workout type
- Mood score

### Q88: How is wellness data stored?
**A:** Wellness data is stored in the `wellness_logs` table with a unique constraint on (user_id, date). This ensures one record per day per user. The data is synced to Firestore under `users/{uid}/wellness/{date}`.

---

## 17. Pulse (Life Log) Feature

### Q89: How does the Pulse feature work?
**A:** Pulse is a one-tap life log that captures moments throughout the day:
- **LifeEvent Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\life_event.dart" /> - represents a logged moment
- **PulseController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\pulse_controller.dart" /> - manages pulse state
- **PulsePage**: <ref_file file="D:\OmniLife\lib\presentation\pages\pulse\pulse_page.dart" /> - UI for logging and viewing timeline
- **PulseTimeline**: <ref_file file="D:\OmniLife\lib\presentation\widgets\pulse\pulse_timeline.dart" /> - timeline widget
- **PulseQuickLog**: <ref_file file="D:\OmniLife\lib\presentation\widgets\pulse\pulse_quick_log.dart" /> - quick log buttons

### Q90: What can be logged in Pulse?
**A:**
- Sleep / Wake (tracks sleep duration)
- Water intake
- Meals
- Workouts
- Mood
- Energy level
- Custom notes

### Q91: How does the timeline work?
**A:** The `TimelineBuilder` (<ref_file file="D:\OmniLife\lib\domain\services\pulse\timeline_builder.dart" />) organizes events into a chronological timeline. It:
1. Fetches all events for a date range
2. Groups by hour
3. Renders as a vertical timeline with icons and labels

### Q92: How does day replay work?
**A:** The `DayReplay` use case (<ref_file file="D:\OmniLife\lib\domain\services\pulse\day_replay.dart" />) reconstructs a day's events in chronological order, showing a narrative of the day's activities.

### Q93: How is momentum calculated?
**A:** The `MomentumCalculator` (<ref_file file="D:\OmniLife\lib\domain\services\pulse\momentum_calculator.dart" />) calculates a momentum score based on:
- Number of logged events
- Consistency of logging
- Positive vs negative events
- Pattern of activities

### Q94: How does sleep tracking work?
**A:** Sleep tracking pairs sleep and wake events:
1. User logs "Sleep" at night
2. User logs "Wake" in the morning
3. The `SleepAnalyzer` (<ref_file file="D:\OmniLife\lib\domain\services\pulse\sleep_analyzer.dart" />) calculates duration
4. Sleep data is displayed on the Sleep page (<ref_file file="D:\OmniLife\lib\presentation\pages\pulse\sleep_page.dart" />)

### Q95: How do pattern insights work?
**A:** The `PatternDetector` (<ref_file file="D:\OmniLife\lib\domain\services\pulse\pattern_detector.dart" />) analyzes logged events to find patterns:
- Peak productivity hours
- Sleep patterns
- Activity frequency
- Correlations between different metrics

---

## 18. Focus Mode Feature

### Q96: How does Focus Mode work?
**A:** Focus Mode is a Pomodoro timer for deep work:
- **FocusSession Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\focus_session.dart" /> - represents a focus session
- **FocusController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\focus_controller.dart" /> - manages focus state
- **FocusPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\focus\focus_page.dart" /> - UI for focus timer
- **FocusTimer**: <ref_file file="D:\OmniLife\lib\domain\focus\focus_timer.dart" /> - timer logic

### Q97: What focus features are available?
**A:**
- Pomodoro timer (25/5 default, customizable)
- Survives app restarts (persists state)
- Ambient audio playback (rain, white noise, etc.)
- Ambient video playback
- Session history
- Statistics (total focus time, sessions completed)

### Q98: How does the timer work?
**A:** The `FocusTimer` uses a periodic timer that:
1. Counts down from the duration
2. Updates the UI every second
3. Plays notification sound when complete
4. Saves state to SharedPreferences for persistence
5. Handles app lifecycle events (pause/resume)

### Q99: How does ambient audio work?
**A:** The `AmbientAudioPlayer` (<ref_file file="D:\OmniLife\lib\core\services\ambient_audio_player.dart" />) uses the `just_audio` package to play:
- Rain sounds
- White noise
- Brown noise
- Custom audio files

Audio plays during focus sessions to help with concentration.

### Q100: How does ambient video work?
**A:** The `FocusVideoPlayer` (<ref_file file="D:\OmniLife\lib\core\services\focus_video_player.dart" />) uses the `video_player` package to play:
- Looping ambient videos
- Nature scenes
- Abstract animations

Videos play during focus sessions for visual focus.

---

## 19. Maps & Places Feature

### Q101: How does the Maps feature work?
**A:** The Maps feature displays saved locations and geofencing:
- **SavedLocation Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\saved_location.dart" /> - represents a saved place
- **PlacesController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\places_controller.dart" /> - manages places state
- **MapPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\map\map_page.dart" /> - UI for map
- **LocationService**: <ref_file file="D:\OmniLife\lib\core\services\location\location_service.dart" /> - location abstraction
- **GeofenceService**: <ref_file file="D:\OmniLife\lib\core\services\location\geofence_service.dart" /> - geofencing logic

### Q102: What map features are available?
**A:**
- Interactive map using `flutter_map`
- Save places with names and notes
- Foreground geofencing (notifies when entering/leaving)
- Current location display
- Distance calculations

### Q103: How does geofencing work?
**A:** The `GeofenceService`:
1. Uses the `geolocator` package for location
2. Monitors user's current location
3. Checks if user enters/leaves saved places
4. Triggers notifications when crossing geofence boundaries
5. Requires GPS permissions

### Q104: What are the limitations?
**A:**
- Requires a physical phone with GPS
- Not available on web or desktop
- Requires location permissions
- Geofencing is foreground-only (not background due to platform limitations)

---

## 20. Sensors Feature

### Q105: How does the Sensors feature work?
**A:** The Sensors feature uses device hardware:
- **DailySteps Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\daily_steps.dart" /> - represents step counts
- **SensorsController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\sensors_controller.dart" /> - manages sensors state
- **SensorsPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\sensors\sensors_page.dart" /> - UI for sensor data
- **StepAggregator**: <ref_file file="D:\OmniLife\lib\core\services\sensors\step_aggregator.dart" /> - step counting logic
- **FlipDetector**: <ref_file file="D:\OmniLife\lib\core\services\sensors\flip_detector.dart" /> - device orientation detection

### Q106: What sensor features are available?
**A:**
- Pedometer step counter
- Hourly and daily step totals
- Flip detector (detects when phone is flipped)
- Device orientation monitoring

### Q107: How does step counting work?
**A:** The `StepAggregator` uses the `pedometer` package to:
1. Listen to step count updates
2. Aggregate steps by hour
3. Store daily totals
4. Display statistics

### Q108: How does the flip detector work?
**A:** The `FlipDetector` uses the `sensors_plus` package to:
1. Monitor accelerometer data
2. Detect orientation changes
3. Trigger actions when phone is flipped
4. Can be used for gesture-based interactions

### Q109: What are the limitations?
**A:**
- Requires a physical phone
- Not available on web or desktop
- Requires sensor permissions
- Accuracy varies by device

---

## 21. Reminders Feature

### Q110: How does the Reminders feature work?
**A:** The Reminders feature manages scheduled notifications:
- **Reminder Entity**: <ref_file file="D:\OmniLife\lib\domain\entities\reminder.dart" /> - represents a reminder
- **RemindersController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\reminders_controller.dart" /> - manages reminders state
- **RemindersPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\reminders\reminders_page.dart" /> - UI for reminders
- **ReminderScheduler**: <ref_file file="D:\OmniLife\lib\core\services\reminder_scheduler.dart" /> - scheduling logic
- **ReminderService**: <ref_file file="D:\OmniLife\lib\domain\usecases\reminders\reminder_service_impl.dart" /> - business logic

### Q111: What reminder features are available?
**A:**
- One-off reminders (fire once at specific time)
- Daily reminders (repeat every day at same time)
- Weekly reminders (repeat every week on same day)
- Snooze action (reschedule for 10 minutes later)
- Mark done action
- Enable/disable reminders
- Delete with undo
- Synced between devices
- Re-scheduled on sign-in

### Q112: How are reminders scheduled?
**A:** Reminders are scheduled using `flutter_local_notifications`:
1. User creates reminder with time and repeat pattern
2. Controller saves to SQLite
3. Syncs to Firestore
4. Schedules local notification via `NotificationService`
5. Notification fires at scheduled time
6. User can snooze or mark done via notification actions

### Q113: How does the snooze action work?
**A:** When user taps "Snooze 10 min":
1. The `notificationBackgroundHandler` runs in background
2. Calls `scheduleReminderWithPlugin` to reschedule
3. Sets new time 10 minutes in the future
4. Shows new notification
5. Updates reminder state

### Q114: How does the mark done action work?
**A:** When user taps "Mark done":
1. The `notificationBackgroundHandler` runs in background
2. Calls the appropriate repository method (e.g., `completeTask`)
3. Cancels the notification
4. Updates reminder state

### Q115: How are reminders synced?
**A:** Reminders are synced via the sync engine:
1. Created reminders are queued for sync
2. Sync engine uploads to Firestore when online
3. When user signs in on another device, reminders are downloaded
4. Local notifications are re-scheduled based on Firestore data

---

## 22. Assistant (AI) Feature

### Q116: How does the Assistant feature work?
**A:** The Assistant is an AI-powered life copilot:
- **AssistantEngine**: <ref_file file="D:\OmniLife\lib\domain\assistant\assistant_engine.dart" /> - AI orchestration
- **AiProvider**: <ref_file file="D:\OmniLife\lib\domain\assistant\ai_provider.dart" /> - Groq API abstraction
- **AssistantController**: <ref_file file="D:\OmniLife\lib\presentation\controllers\assistant_controller.dart" /> - manages assistant state
- **AssistantPage**: <ref_file file="D:\OmniLife\lib\presentation\pages\assistant\assistant_page.dart" /> - UI for chat
- **VoiceService**: <ref_file file="D:\OmniLife\lib\presentation\assistant\voice_service.dart" /> - speech-to-text and text-to-speech

### Q117: What can the Assistant do?
**A:**
- Create tasks from natural language
- Create calendar events
- Create notes
- Create reminders
- Log expenses
- Log multiple Pulse facts from one message
- Toggle habits
- Answer questions about spending and sleep
- Chat via text or voice

### Q118: How does the AI engine work?
**A:** The `AssistantEngine`:
1. Builds a system prompt with current time and timezone
2. Sends user message to Groq API
3. Receives tool calls (functions to execute)
4. Executes tools (create task, log water, etc.)
5. Composes reply from tool results
6. Returns result to UI

### Q119: What are assistant tools?
**A:** Tools are functions the AI can call, defined in <ref_file file="D:\OmniLife\lib\domain\assistant\assistant_tool.dart" />:
- `createTask`: Create a task
- `createEvent`: Create a calendar event
- `createNote`: Create a note
- `createReminder`: Create a reminder
- `addTransaction`: Log income or expense
- `logPulse`: Log wellness facts
- `toggleHabit`: Toggle habit completion
- `getSpending`: Query spending data
- `getSleep`: Query sleep data

### Q120: How does voice input work?
**A:** Voice input uses:
- `speech_to_text` package for speech-to-text
- Microphone permission handling
- Real-time transcription
- Text sent to assistant engine

### Q121: How does voice output work?
**A:** Voice output uses:
- `flutter_tts` package for text-to-speech
- Speaks assistant replies
- Configurable voice and speed

### Q122: What is the fallback mechanism?
**A:** If the AI API is unavailable, the assistant uses regex-based fallbacks to detect:
- Income: "received 2500", "earned 500"
- Expense: "spent 500", "paid 100"
This ensures basic functionality even without AI.

### Q123: How is chat history managed?
**A:** Chat history is stored in `ChatHistoryStore` (<ref_file file="D:\OmniLife\lib\data\assistant\chat_history_store.dart" />):
- Persists conversation locally
- Limited to recent messages (configurable)
- Used for context in AI requests

### Q124: What is the memory store?
**A:** The `MemoryStore` (<ref_file file="D:\OmniLife\lib\domain\assistant\memory_store.dart" />) stores user preferences and facts:
- Remembers user's typical wake time
- Remembers common categories
- Improves AI responses over time
- Can be remote (optional URL) or local

---

## 23. Notifications Feature

### Q125: How do notifications work in OmniLife?
**A:** OmniLife uses two notification systems:
1. **Local Notifications**: `flutter_local_notifications` for scheduled reminders
2. **Push Notifications**: Firebase Cloud Messaging (FCM) for remote notifications

### Q126: How are local notifications scheduled?
**A:** When a task has a due date or a reminder is created:
1. Controller calls `NotificationService.scheduleReminder()`
2. Service uses `flutter_local_notifications` to schedule
3. Notification appears at scheduled time
4. Has "Mark done" and "Snooze" actions
5. Actions work even when app is in background

### Q127: How do notification actions work?
**A:** Notifications can have actions that trigger background code:
- **Mark done**: Completes the task/reminder
- **Snooze**: Reschedules for 10 minutes later

Actions are handled by `notificationBackgroundHandler` which runs in a background isolate.

### Q128: What is the in-app notification banner?
**A:** For demo visibility, OmniLife shows an in-app banner (Snackbar) when notifications arrive. This ensures notifications are visible even on web where system notifications aren't available.

### Q129: How does FCM work?
**A:** Firebase Cloud Messaging:
1. Device registers for FCM tokens via `PushRegistrationManager` (<ref_file file="D:\OmniLife\lib\core\services\push_registration_manager.dart" />)
2. Token is stored in Firestore under `users/{uid}/devices/{deviceId}`
3. Server can send push messages to specific devices or topics
4. `PushMessagingService` (<ref_file file="D:\OmniLife\lib\core\services\push_messaging_service.dart" />) handles incoming messages
5. Messages are displayed as in-app notifications

### Q130: What is the notification center?
**A:** The notification center (<ref_file file="D:\OmniLife\lib\presentation\widgets\notifications\notification_sheet.dart" />) displays:
- Recent notifications
- Unread count badge
- Tap to view details
- Mark as read

---

## 24. Core Services Deep Dive

### Q131: What core services exist?
**A:**
- **NotificationService**: <ref_file file="D:\OmniLife\lib\core\services\notification_service.dart" /> - local notifications
- **PushMessagingService**: <ref_file file="D:\OmniLife\lib\core\services\push_messaging_service.dart" /> - FCM push messages
- **AnalyticsService**: <ref_file file="D:\OmniLife\lib\core\services\analytics_service.dart" /> - Firebase Analytics
- **CrashReporter**: <ref_file file="D:\OmniLife\lib\core\services\crash_reporter.dart" /> - Firebase Crashlytics
- **LocationService**: <ref_file file="D:\OmniLife\lib\core\services\location\location_service.dart" /> - location abstraction
- **GeofenceService**: <ref_file file="D:\OmniLife\lib\core\services\location\geofence_service.dart" /> - geofencing
- **SensorService**: <ref_file file="D:\OmniLife\lib\core\services\sensors\sensor_service.dart" /> - sensors abstraction
- **ReceiptUploader**: <ref_file file="D:\OmniLife\lib\core\services\receipt_uploader.dart" /> - image upload
- **FinanceReportPdf**: <ref_file file="D:\OmniLife\lib\core\services\finance_report_pdf.dart" /> - PDF generation
- **UserSessionCoordinator**: <ref_file file="D:\OmniLife\lib\core\services\user_session_coordinator.dart" /> - session management

### Q132: How does the analytics service work?
**A:** The `AnalyticsService` wraps Firebase Analytics:
- Logs events (e.g., task created, task completed)
- Logs user properties
- Disabled in debug mode (configurable)
- Events are defined in telemetry config

### Q133: How does the crash reporter work?
**A:** The `CrashReporter` wraps Firebase Crashlytics:
- Catches Flutter errors
- Catches platform errors
- Records custom keys
- Disabled in debug mode (configurable)
- Provides stack traces for debugging

### Q134: How does the location service work?
**A:** The `LocationService` provides an abstraction over location:
- Get current location
- Get last known location
- Stream location updates
- Permission handling
- Implemented by `GeolocatorLocationService` using `geolocator` package

### Q135: How does the sensor service work?
**A:** The `SensorService` provides an abstraction over sensors:
- Stream accelerometer data
- Stream gyroscope data
- Permission handling
- Implemented by `DeviceSensorService` using `sensors_plus` package

---

## 25. Widget Library Overview

### Q136: What shared widgets exist?
**A:**
- **ModuleTile**: <ref_file file="D:\OmniLife\lib\presentation\widgets\module_tile.dart" /> - dashboard module card
- **ProgressRing**: <ref_file file="D:\OmniLife\lib\presentation\widgets\progress_ring.dart" /> - circular progress indicator
- **StatTile**: <ref_file file="D:\OmniLife\lib\presentation\widgets\stat_tile.dart" /> - statistic display
- **AppEmptyView**: <ref_file file="D:\OmniLife\lib\presentation\widgets\app_empty_view.dart" /> - empty state
- **AppLoadingView**: <ref_file file="D:\OmniLife\lib\presentation\widgets\app_loading_view.dart" /> - loading indicator
- **PrimaryButton**: <ref_file file="D:\OmniLife\lib\presentation\widgets\primary_button.dart" /> - primary action button
- **SecondaryButton**: <ref_file file="D:\OmniLife\lib\presentation\widgets\secondary_button.dart" /> - secondary action button
- **AppChip**: <ref_file file="D:\OmniLife\lib\presentation\widgets\app_chip.dart" /> - selectable chip
- **AppSectionContainer**: <ref_file file="D:\OmniLife\lib\presentation\widgets\app_section_container.dart" /> - section container
- **SampleDataPrompt**: <ref_file file="D:\OmniLife\lib\presentation\widgets\sample_data_prompt.dart" /> - demo data prompt

### Q137: What feature-specific widgets exist?
**A:**
- **TaskCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\task_card.dart" /> - task display
- **EventBlock**: <ref_file file="D:\OmniLife\lib\presentation\widgets\calendar\event_block.dart" /> - calendar event
- **NoteCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\notes\note_card.dart" /> - note display
- **HabitCard**: <ref_file file="D:\OmniLife\lib\presentation\widgets\habits\habit_card.dart" /> - habit display
- **HabitHeatmap**: <ref_file file="D:\OmniLife\lib\presentation\widgets\habits\habit_heatmap.dart" /> - 365-day heatmap
- **PulseQuickLog**: <ref_file file="D:\OmniLife\lib\presentation\widgets\pulse\pulse_quick_log.dart" /> - quick log buttons
- **PulseTimeline**: <ref_file file="D:\OmniLife\lib\presentation\widgets\pulse\pulse_timeline.dart" /> - timeline view
- **AssistantMicButton**: <ref_file file="D:\OmniLife\lib\presentation\widgets\assistant\assistant_mic_button.dart" /> - voice input button
- **NextUpSection**: <ref_file file="D:\OmniLife\lib\presentation\widgets\next_up\next_up_section.dart" /> - upcoming items

### Q138: What are the design principles for widgets?
**A:**
- Reusable across the app
- Consistent styling via theme
- Responsive design
- Accessible (proper labels, sizes)
- Optimized for performance (const constructors where possible)
- Clear separation of UI and logic

---

## Summary

### Key Architectural Points:
1. **Clean Architecture**: Separation of concerns with Domain, Data, Presentation, and Core layers
2. **Offline-First**: SQLite primary, Firebase secondary, background sync with retry logic
3. **State Management**: GetX for most features, BLoC for authentication
4. **Firebase Integration**: Auth, Firestore, Storage, FCM, Analytics, Crashlytics
5. **Security**: Ownership-based rules, field validation, user isolation

### Feature Coverage:
- 13 major features all covered with equal depth
- Each feature has entity, controller, page, and widgets
- Consistent patterns across features
- High-level overview with file references

### Demo-Ready Features:
- Authentication (login/register with validation)
- Tasks (create, complete, delete with filters)
- Calendar (month/week/day views)
- Notifications (local and push)
- Dashboard (progress, next up, quick actions)

This comprehensive Q&A guide provides a complete overview of the OmniLife project for review preparation and understanding.
