# OmniLife - 30 Minute Presentation Script

**Course:** 23CSE465 Mobile Application Development  
**Project:** OmniLife - AI Personal Life Operating System  
**Total Time:** 30 minutes  
**Presenters:** [Add team member names]

---

## **[0:00-0:05] Introduction**

**Speaker 1:**
"Good morning/afternoon everyone. Today we're presenting OmniLife, a personal life management system built with Flutter. OmniLife is designed as a placement-level project that demonstrates mastery of the complete Mobile Application Development syllabus."

"OmniLife integrates 12 core life management domains into a single, connected ecosystem - including tasks, calendar, notes, habits, finance, wellness, and even an AI copilot. Our goal was to build not just a collection of separate features, but a truly integrated personal operating system."

---

## **[0:05-0:10] Technology Stack & Architecture**

**Speaker 1:**
"Let me start with our technology stack. We're using Flutter as our framework with Dart as the programming language. Flutter allows us to build for Android, web, and desktop from a single codebase."

"For state management, we're using a hybrid approach - GetX for most features because it's lightweight and fast to develop, and BLoC reserved for our future AI features where we need more complex event-driven state management."

"Our backend is Firebase - we're using Auth for authentication, Firestore for cloud sync, Storage for files, FCM for push notifications, Analytics, and Crashlytics. For local data persistence, we use SQLite, following an offline-first architecture where all data is saved locally first, then synced to the cloud."

"Architecturally, we follow Clean Architecture with three layers: the Domain layer for business logic, the Data layer for persistence, and the Presentation layer for UI. This separation makes our code testable, maintainable, and flexible."

---

## **[0:10-0:15] Project Structure**

**Speaker 2:**
"Let me walk you through our project structure. In the `lib` folder, we have `app` for configuration, `core` for shared services, `data` for our database and repositories, `domain` for business entities and use cases, and `presentation` for our UI."

"The `data` layer contains our data models with serialization logic for both SQLite and Firebase. The `domain` layer has our entities - these are framework-agnostic business objects like Task, CalendarEvent, Note, and Habit. The `presentation` layer has our GetX controllers, pages, and reusable widgets."

"We use GetX for dependency injection through bindings. When the app starts, `InitialBinding` registers all our controllers and repositories. Controllers marked as `permanent: true` survive navigation, while repositories are created lazily when first needed."

---

## **[0:15-0:20] Key Features Overview**

**Speaker 2:**
"OmniLife has 12 core modules. Let me highlight the ones we've implemented: Tasks & Projects for managing to-dos, Calendar & Schedule for time management, Notes & Knowledge Base with markdown support, Habits & Goal Tracking with streaks, Personal Finance for budgeting, and Wellness & Mood tracking."

"We also have additional modules for Maps & Geofencing, Sensors & Hardware integration, Media & Focus Mode for productivity, and Notifications. The AI Life Copilot is planned for future implementation using BLoC for complex streaming state."

"Each module follows the same architectural pattern - a domain entity, a repository interface, a repository implementation with local SQLite and remote Firebase datasources, a GetX controller for state management, and UI pages with reusable widgets."

---

## **[0:20-0:25] Demo - Authentication & Tasks**

**Speaker 3:**
"Now let me show you the application. I'll start with authentication. [Switch to app] Our login page supports both email/password and Google Sign-In. We have input validation for email format and password strength. Clear error messages appear if login fails. Once logged in, the user session persists across app restarts using Firebase Auth."

"After login, we redirect to the dashboard. Let me show you the Tasks feature. [Navigate to Tasks] This is our Tasks page. You can see filter chips at the top - All, Today, Upcoming, and Completed. We also have priority filtering. Let me create a new task. [Tap New Task]"

"I'll enter 'Complete project documentation' with high priority and set a due date for today. [Fill form and tap Create Task] You'll see a SnackBar confirmation immediately - that's our optimistic UI update. The task appears in the list instantly. The TaskCard widget shows the priority indicator with a colored dot, smart date formatting - it says 'Today' instead of the full date - and an animated checkbox."

"Let me mark this task as complete. [Tap checkbox] The checkbox animates, and the task is struck through. This updates both our local SQLite database and syncs to Firebase in the background. If we delete a task [Tap delete], it's removed from the list with immediate UI feedback."

---

## **[0:25-0:30] Demo - Calendar, Notes, Habits & Conclusion**

**Speaker 3:**
"Now let me show you the Calendar. [Navigate to Calendar] We have three views - Month, Week, and Day. The month view shows a grid of days with event dots. Let me create an event. [Tap + button] I'll create a 'Project Review' event for 2 PM with a blue color tag. The event appears in the timeline."

"The Week view shows a 7-day timeline with event blocks positioned side-by-side when they overlap. We also have an agenda view that merges both calendar events and tasks with due dates into a single timeline."

"Let me quickly show you Notes. [Navigate to Notes] Notes support markdown rendering. I can create checklists using markdown syntax like '- [ ] Buy groceries'. These render as interactive checkboxes that I can tap to complete. There's also a debounced search that ranks results by relevance."

"And finally, Habits. [Navigate to Habits] We track habits with a GitHub-style heatmap showing 365 days of completion history. This heatmap calculates streaks based on the habit's frequency - daily, weekly, or custom days. Tapping a day marks it complete, and the streaks update immediately."

**Speaker 1:**
"To summarize, OmniLife demonstrates clean architecture, offline-first data persistence, reactive state management with GetX, and a comprehensive feature set covering multiple domains. All features work without an internet connection thanks to our SQLite database, and sync seamlessly to Firebase when connectivity is available."

"We have 97+ tests passing, including unit tests, widget tests, and model tests. The app follows Material 3 design guidelines with a custom anti-slop aesthetic - no gradients, clean surfaces with hairline borders, and consistent theming."

"Thank you for your attention. We're happy to answer any questions about the implementation, architecture, or specific features."

---

## **Demo Checklist**

**Before the presentation, ensure:**
- [ ] Flutter app is running on device/emulator
- [ ] Firebase is properly configured
- [ ] Test account credentials are ready
- [ ] Network connection is available (for Firebase sync demo)
- [ ] Sample data exists for demo (a few tasks, events, notes, habits)

**During the presentation:**
- [ ] Show login with validation error (enter invalid email)
- [ ] Show successful login and auto-redirect
- [ ] Create a task with due date today
- [ ] Show SnackBar confirmation
- [ ] Mark task complete (show animation)
- [ ] Delete a task
- [ ] Create a calendar event
- [ ] Switch between Month/Week/Day views
- [ ] Show note with markdown checklist
- [ ] Tap checklist item to complete
- [ ] Show habit heatmap
- [ ] Tap a day to mark habit complete
- [ ] If time permits, show notification demo

---

## **Team Member Roles**

**Speaker 1 (0:00-0:10):** Introduction, Technology Stack, Architecture, Project Structure  
**Speaker 2 (0:10-0:20):** Feature Overview, Modules, Architecture Details  
**Speaker 3 (0:20-0:30):** Live Demo, Calendar, Notes, Habits, Conclusion  

*Adjust roles based on your team size and expertise.*

---

## **Key Talking Points for Q&A**

**If asked about Clean Architecture:**
"We separated our code into Domain, Data, and Presentation layers. The Domain layer has business logic and entities that don't depend on frameworks. The Data layer handles SQLite and Firebase. The Presentation layer has our UI. This makes testing easier and allows us to swap implementations without changing business logic."

**If asked about GetX vs BLoC:**
"We use GetX for simple state like tasks and calendar because it's lightweight with less boilerplate. BLoC is reserved for our AI Copilot feature where we need complex event-driven workflows. This hybrid approach gives us the best of both - speed for common features, reliability for complex ones."

**If asked about Offline-First:**
"All data is saved to SQLite immediately with optimistic UI updates. The user sees instant feedback. Then, in the background, we sync to Firebase. This means the app works without internet, and data is never lost even if the sync fails."

**If asked about Testing:**
"We have 97+ tests passing. These include unit tests for business logic, widget tests for UI components, and model tests for data serialization. Our tests ensure that changes don't break existing functionality."

**If asked about Design:**
"We follow Material 3 design guidelines with a custom 'anti-slop' aesthetic - no gradients, glassmorphism only on auth screens, clean surfaces with 1px hairline borders. We have both light and dark themes with semantic colors for different modules."

---

## **Time Management Tips**

- **Keep to time** - Practice the full script with a timer
- **Demo first** - Ensure the app is running and data is ready before starting
- **Transitions** - Have clear signals for when to switch speakers
- **Backup plan** - If Firebase sync fails, focus on SQLite and UI features
- **Emphasis** - Spend more time on the unique aspects (architecture, offline-first, clean design)
