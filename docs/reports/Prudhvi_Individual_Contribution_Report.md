# Individual Contribution Report: OmniLife
**Course:** 23CSE465 - Mobile Application Development  
**Academic Year:** 2026-2027 | **Evaluation Window:** 13-10-2026 to 23-10-2026  
**Total Evaluation Marks:** 25 Marks  

---

### Student Information
- **Student Name:** Prudhvi
- **GitHub Username:** `prudhvimanvith`
- **Application Name:** OmniLife (AI Personal Life Operating System)
- **Primary Assigned Modules:** Habits & Goal Tracking System, Personal Finance & Budgeting Module, Notes Knowledge Base, and Mobile Sensor Integration

---

### 1. Modules & Architecture Developed

In OmniLife, my development responsibilities focused on delivering complex personal productivity and financial tracking modules, implementing rich data visualization algorithms, and integrating mobile hardware sensors.

1. **Habits & Goal Tracking Engine (`lib/presentation/pages/habits/` & `domain/usecases/habit_streaks.dart`):**
   - Engineered the habits management subsystem supporting daily, weekly, and custom target schedules.
   - Built the 365-day interactive activity heatmap component visualizing historical completion frequency using color-intensity mapping.
   - Designed mathematical algorithms in `HabitStreaks` to calculate consecutive active streaks and all-time record streaks with instant rollback/undo protection.

2. **Personal Finance & Budgeting Suite (`lib/presentation/pages/finance/`):**
   - Developed the financial transaction logger supporting income and expense categorization.
   - Built analytical spending breakdowns and monthly budget ceiling comparisons.
   - Implemented receipt image attachment handling integrated with camera and local file caching.
   - Created PDF export functionality allowing users to compile and download monthly financial statement summaries.

3. **Notes & Knowledge Base (`lib/presentation/pages/notes/`):**
   - Implemented markdown note-taking with real-time markdown parsing and checklist rendering.
   - Built interactive checklist items that toggle dynamically and persist status changes directly to the database.
   - Implemented tag filtering, category organization, note pinning, and ranked search filtering (`NoteSearchRanker`).

4. **Hardware Sensors & Physical Gesture Integration (`lib/core/services/`):**
   - Integrated mobile device accelerometer and gyroscope sensors via `sensors_plus`.
   - Developed the quick-access "Double Shake" gesture detection (`8ba7750`), allowing users to trigger the OmniLife voice assistant overlay simply by shaking their physical handset.

---

### 2. Concrete Features & Code Contributed

- **`lib/domain/usecases/habit_streaks.dart`**: Penned pure business logic algorithms computing current streaks and longest streaks across custom weekly milestone intervals.
- **`lib/presentation/pages/habits/widgets/habit_heatmap.dart`**: Implemented the GitHub-style annual contribution grid with touchable day cells and dynamic month headers.
- **`lib/presentation/pages/finance/finance_page.dart`**: Built expense categorization charts, transaction history feeds, and budget warning indicators.
- **`lib/core/services/shake_detector.dart`**: Programmed raw sensor threshold filters to detect intentional double-shake gestures while rejecting random walking vibrations.
- **`lib/presentation/pages/notes/widgets/note_card.dart`**: Built interactive note previews with pinned badges, category tags, and checkbox toggle handlers.

---

### 3. Key Git Commits & Version Control Traceability

All commits were recorded under my GitHub profile `prudhvimanvith`:

- `f6e24fb` - *feat: Habits and Goal Tracking*
- `2ab5589` - *feat: complete Finance and Budgeting module*
- `e51116c` - *feat: Notes & Knowledge Base*
- `d1808e8` - *feat: implement Wellness and Sleep tracking module*
- `8ba7750` - *feat: add double-shake gesture to summon voice assistant popup on mobile*
- `9eddc83` - *build(android): configure dedicated release signing and key.properties loading*
- `b6aa4a9` - *fix: preserve default_web_client_id and configure serverClientId for mobile Google Sign-In*
- `ad386e9` - *fix: resolve string interpolation and layout overflow bugs in finance page*
- `b079181` - *fix: resolve habit module bugs and dependencies*
- `f70c437` - *fix: adjust voice assistant timeout, update test assertions, and update gitignore*

---

### 4. Tools, Frameworks & Packages Leveraged

| Tool / Package | Version / Type | Purpose in Project |
|---|---|---|
| **sensors_plus** | Hardware Plugin | Accessing phone accelerometer data for gesture recognition |
| **fl_chart** | Charting Library | Rendering financial category spending breakdowns |
| **flutter_markdown** | Markdown Renderer | Parsing markdown formatting and checklist items inside notes |
| **pdf / printing** | Document Generation | Exporting structured monthly finance statement reports |
| **image_picker** | Media Picker | Attaching receipt photos to expense records |

---

### 5. Technical Challenges Faced & Solutions

#### Challenge 1: Accurately Computing Broken Habit Streaks Across Variable Schedules
- **Problem:** When users track weekly habits (e.g., "3 days per week") rather than simple consecutive daily habits, standard calendar day-counter algorithms falsely marked streaks as broken on rest days.
- **Solution:** Formulated a window-based tracking algorithm in `HabitStreaks`. The algorithm groups completion timestamps by calendar week ISO standards, checking whether weekly frequency targets are fulfilled before resetting or incrementing streak counters.

#### Challenge 2: Accidental False Triggers with Accelerometer Shake Gestures
- **Problem:** Normal phone movement—such as placing the handset onto a desk or walking with the phone in a pocket—was frequently triggering the voice assistant overlay by accident.
- **Solution:** Implemented a two-stage peak-detection threshold algorithm with a sliding time window (350ms to 800ms) in `shake_detector.dart` (`8ba7750`). The listener requires two distinct acceleration spikes exceeding 12.5 m/s² within the time bracket, eliminating false positives during routine physical movement.

---

### 6. Testing & Quality Assurance Performed

- **Unit Testing of Streak Calculation Logic:**
  - Wrote comprehensive unit tests (`test/domain/usecases/habit_streaks_test.dart`) validating edge cases: leap years, month boundaries, timezone offsets, and streak restoration upon unchecking accidental completions.
- **Finance Module Calculations:**
  - Verified math precision in financial aggregations, ensuring category expense sums match overall wallet deductions down to the cent without floating-point rounding errors.
- **Sensor Calibration on Live Hardware:**
  - Tested the shake gesture on physical Android hardware across different orientations to ensure smooth triggering for the live evaluation presentation.
