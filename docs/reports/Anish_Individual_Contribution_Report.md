# Individual Contribution Report: OmniLife
**Course:** 23CSE465 - Mobile Application Development  
**Academic Year:** 2026-2027 | **Evaluation Window:** 13-10-2026 to 23-10-2026  
**Total Evaluation Marks:** 25 Marks  

---

### Student Information
- **Student Name:** Anish
- **GitHub Username:** `anish435`
- **Application Name:** OmniLife (AI Personal Life Operating System)
- **Primary Assigned Modules:** Calendar & Scheduling System, Notification & Reminder Service, Firebase Auth & Google Sign-In, and AI Voice Assistant Integration

---

### 1. Modules & Architecture Developed

My primary focus on the OmniLife project encompassed calendar scheduling, local and cloud push notification engines, authentication pipeline security, and multi-modal AI voice assistant integration.

1. **Calendar & Multi-View Scheduling Suite (`lib/presentation/pages/calendar/`):**
   - Engineered the comprehensive calendar interface supporting Month, Week, and Day timeline modes (`1196b57`).
   - Implemented an event packing and collision layout algorithm for Day and Week views, allowing overlapping appointments to display side-by-side with distinct colored left indicator bars.
   - Built the unified Agenda view (`GetAgendaForRange` use case), synthesizing both calendar events and due tasks into an ordered daily itinerary.

2. **Booking Confirmations & Notification Engine (`lib/core/services/` & `docs/DEMO_CHECKLIST.md`):**
   - Implemented local push notifications and schedule triggers using `flutter_local_notifications` (`15ea72e`).
   - Built the scheduling workflow providing immediate on-device confirmation dialogs and system notification tray reminders for tasks, reminders, and scheduled events (addressing Rubric Section D).
   - Configured Firebase Cloud Messaging (FCM) background message handlers and notification channels.

3. **Authentication & Session Persistence (`lib/presentation/controllers/auth_controller.dart`):**
   - Implemented Firebase Authentication supporting both standard email/password registration and Google OAuth sign-in.
   - Configured cross-platform OAuth client IDs (`serverClientId` and `default_web_client_id`) aligning mobile and web environments (`25a13df`).
   - Ensured secure session restoration on app restart and built clean logout cascades that flush active memory controllers and route users back to login.

4. **AI Life Copilot & Voice Assistant (`lib/domain/assistant/` & `presentation/assistant/`):**
   - Connected Groq LLM API integrations enabling natural language task creation, expense recording, and calendar scheduling via typed or spoken prompts.
   - Integrated Speech-To-Text (STT) and Text-To-Speech (TTS) pipelines (`e8e3279`), allowing hands-free voice commands.
   - Implemented compound command parsing allowing multiple actions (e.g. logging a meal and scheduling a reminder) from a single spoken phrase.

---

### 2. Concrete Features & Code Contributed

- **`lib/presentation/pages/calendar/calendar_page.dart`**: Implemented Month, Week, and Day visual calendar viewports with quick "Today" navigation and date selection pickers.
- **`lib/core/services/notification_service.dart`**: Created the local notification scheduling service, handling exact alarm permissions, notification channels, snooze actions, and dismissal callbacks.
- **`lib/domain/assistant/assistant_engine.dart`**: Built tool-calling logic that maps conversational user intents directly into domain entity creation operations.
- **`lib/presentation/controllers/calendar_controller.dart`**: Managed reactive calendar state, event creation bottom sheets, and optimistic event mutation broadcasts.
- **`lib/core/services/voice_service.dart`**: Managed audio recording streams, microphone permission handling, and spoken feedback synthesis.

---

### 3. Key Git Commits & Version Control Traceability

All commits were recorded under my GitHub profile `anish435`:

- `1196b57` - *feat(calendar): implement Module 2 — Calendar & Schedule with Month, Week, and Day views*
- `15ea72e` - *feat: add booking and reminder notifications with UI improvements*
- `e8e3279` - *feat(insights,assistant): dynamic testing mode exit, compound voice logging, and reminder notifications*
- `25a13df` - *fix(auth): align Google Sign-In serverClientId and Firebase android options with omnilife-a78bb project*
- `9734b4d` - *fix(android,assistant): fix Gradle build imports, disable incremental caching, and improve web voice error handling*
- `c937b45` - *fix(di): fix CalendarController and TaskController registration, route bindings, and task creation submission*
- `27f9716` - *fix(tasks): make Firestore sync non-blocking and guard loading states in task/calendar creation*
- `a46b850` - *fix: resolve linter warnings and offline firestore handling*
- `a9bb93a` - *docs: add project plan*

---

### 4. Tools, Frameworks & Packages Leveraged

| Tool / Package | Version / Type | Purpose in Project |
|---|---|---|
| **flutter_local_notifications** | Notification Plugin | Triggering local timed reminders and confirmation banners |
| **firebase_auth / google_sign_in** | Identity Provider | Handling authentication, session storage, and OAuth credentials |
| **speech_to_text / flutter_tts** | Audio / Voice APIs | Real-time speech recognition and text-to-speech audio synthesis |
| **http** | Networking | Communicating with Groq's high-speed inference endpoints |
| **firebase_messaging** | Cloud Push | Delivering device-targeted remote notifications |

---

### 5. Technical Challenges Faced & Solutions

#### Challenge 1: Android 13+ Exact Alarm and Notification Permission Restrictions
- **Problem:** On newer Android versions (API 33+), scheduled notifications for calendar reminders failed silently unless runtime `POST_NOTIFICATIONS` and `SCHEDULE_EXACT_ALARM` permissions were explicitly granted.
- **Solution:** Implemented proactive permission checks and an educational prompt in `NotificationService` (`15ea72e`). The app requests exact alarm permissions upon scheduling the first reminder and gracefully falls back to non-exact delivery windows if permissions are restricted by battery optimization settings.

#### Challenge 2: Google Sign-In Client ID Misalignment Between Web and Android
- **Problem:** Google Sign-In threw `ApiException 10` during Android device testing while working properly on web, caused by mismatched client ID mappings between the Firebase console and the local Google Services JSON.
- **Solution:** Audited and resolved OAuth client configurations in `25a13df`. Configured the correct `serverClientId` within `GoogleSignIn` initialization in Dart, allowing the backend to exchange Google auth tokens seamlessly across both platforms.

---

### 6. Testing & Quality Assurance Performed

- **Authentication & Security Rules Testing:**
  - Validated automated test cases `AUTH-01` through `AUTH-12` in `test/presentation/controllers/auth_controller_test.dart` and `auth_guard_test.dart`, ensuring signed-out users cannot access restricted routes.
- **Notification Scheduling Verification:**
  - Performed manual hardware testing on physical Android devices, setting 1-minute and 5-minute timed event reminders and verifying that banners trigger accurately even when the app is in the background.
- **Calendar Overlap Packing Tests:**
  - Ran unit tests on calendar time block layout algorithms to ensure multi-event days never hide or clip concurrent schedule slots.
