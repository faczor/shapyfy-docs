---
tags:
  - feature
  - workout
  - version/mvp
  - version/v1.1
name: Workout Process
status: "[[Statuses/Done]]"
---

# Workout Process Feature

> **Navigation**: See [00-app-overview.md](00-app-overview.md) for complete navigation flows and integration points

## Overview

### Purpose
The Workout Process feature enables users to log their strength training workouts in real-time while in the gym. Users can select exercises, manage their workout queue, log sets with weight and reps data, and track their progress compared to previous workouts.

### User Problem
Users in the gym need to quickly log their workout activity while potentially dealing with:
- Limited time between sets (1-3 minutes rest)
- Poor internet connectivity in gyms
- Sweaty hands and interruptions
- Need to remember previous performance
- Equipment unavailability or changes mid-workout
- Following workout plans while maintaining flexibility

### Responsibilities
- Exercise search and filtering
- Custom exercise creation (one-field, AI-categorized)
- Exercise queue management
- Set logging with pre-fill from historical data
- Exercise state management (queued, in-progress, completed, skipped)
- Skip exercise functionality with confirmations
- Plan accountability (check logged sets vs planned sets)
- Volume tracking and progress visualization
- Workout completion and auto-save
- Offline-first data persistence

---

## Screens Overview

**Core Workout Flow:**
1. **Exercise Picker** → See [2.01-exercise-picker.md](../designer/2.01-exercise-picker.md)
2. **Active Workout Hub** → See [2.02-active-workout.md](../designer/2.02-active-workout.md)
3. **Exercise Detail (Set Logger)** → See [2.03-exercise-detail.md](../designer/2.03-exercise-detail.md)
4. **Workout Summary** → See [2.04-workout-summary.md](../designer/2.04-workout-summary.md)

---

## Key Design Decisions (2025-01-05 Review)

### Issue #2: Hide "Finish Workout" Button When 0 Exercises
**Problem:** Confusing to show "Finish Workout" when nothing has been started.

**Solution:**
- Button hidden when `exercises.isEmpty()`
- "Add Exercise" becomes primary (solid green) when alone
- Button appears after first exercise added
- Clear causality: Add something → Then finish it

**See:** [2.02-active-workout.md](../designer/2.02-active-workout.md) for implementation details

---

### Issue #3: Pre-fill Logic - Plan as Reference
**Problem:** Unclear how to balance workout plan guidance with user autonomy.

**Client Decision:**
- Plans are adaptive (adjust over time)
- User autonomy prioritized over plan rigidity
- Variable psychology (different users, different needs)

**Solution:**
- **History pre-fills input fields** (user's actual capability)
- **Plan shown as reference only** in Exercise Info Card
- Format: "📋 Today's Plan: 3×10 @ 60kg"
- Non-prescriptive, informational only
- Backend tracks adherence for future adaptation
- Plan deviation dialog when `logged_sets < planned_sets` (gentle accountability)

**See:** [2.03-exercise-detail.md](../designer/2.03-exercise-detail.md) for implementation details

---

### Issue #5: Exercise Ordering (No Auto-Reordering)
**Problem:** Auto-reordering by status broke user's mental model mid-workout.

**Solution:**
- **Exercises stay in user's addition order** (NEVER reorder by status)
- Visual states show progress (opacity, borders, checkmarks)
- Position determined by `orderIndex` field
- Only ONE exercise "In Progress" at a time (clear focus)
- Previous "In Progress" auto-transitions to Completed/Queued

**Example:**
```
User adds: Squat → Bench → Deadlift

User completes Squat, skips Bench:
1. Squat (Completed ✓)     ← Position 1
2. Bench (Skipped)          ← Position 2
3. Deadlift (In Progress)   ← Position 3

Order NEVER changes based on status.
```

**See:** [2.02-active-workout.md](../designer/2.02-active-workout.md) for implementation details

---

### Issue #6: Skip Exercise Functionality
**Problem:** No way to skip exercise when equipment unavailable or injury occurs.

**Solution:**
- **Skip Exercise button** when 0 sets logged (gray, neutral)
- Skip confirmation dialog (prevents accidental taps)
- Marks exercise as **SKIPPED** (new status)
- Exercise remains in workout (non-destructive)
- Can reopen later to log sets (un-skip functionality)
- Skipped exercises shown in Workout Summary

**See:** [2.03-exercise-detail.md](../designer/2.03-exercise-detail.md) for skip functionality
**See:** [2.02-active-workout.md](../designer/2.02-active-workout.md) for skipped card state
**See:** [2.04-workout-summary.md](../designer/2.04-workout-summary.md) for summary display

---

## State Machine

### Exercise States

```
QUEUED ──[tap card]──> IN_PROGRESS ──[log sets + finish]──> COMPLETED
  │                          │
  │                          └──[skip (0 sets)]──> SKIPPED
  │                                                    │
  └──────────────[tap skipped card]───────────────────┘
```

**Rules:**
- Only ONE exercise "In Progress" at a time
- Previous "In Progress" auto-transitions:
  - If has logged sets → COMPLETED
  - If 0 sets → QUEUED
- Skipped exercises can be reopened (un-skip)
- Completed exercises can be reopened to add more sets

---

## Exercise Ordering Rules

### Primary Rule
**Exercises display in the exact order the user added them. NEVER auto-reorder by status.**

### Adding New Exercises
- New exercises append to bottom of list
- `orderIndex` assigned: `max(existingIndexes) + 1`

### Deleting Exercises
- Remaining exercises maintain their relative order
- `orderIndex` values NOT re-indexed (preserve order)

### Visual States (Not Order)
States indicate progress through visual treatment:
- **Queued:** 80% opacity, "Start" button
- **In Progress:** Green border, "Continue" button
- **Completed:** 100% opacity, checkmark ✓
- **Skipped:** 60% opacity, italic "Skipped" text

---

## Single "In Progress" Exercise Rule

### Logic
Only ONE exercise can have green border (In Progress) at a time.

### When User Taps Different Exercise:
1. Previous "In Progress" exercise auto-transitions:
   - If has logged sets → Becomes "Completed" ✓
   - If 0 sets logged → Reverts to "Queued"
2. New exercise becomes "In Progress" (green border appears)
3. User navigates to Exercise Detail screen

**Purpose:** Clear visual focus, matches physical reality (doing one exercise at a time)

---

## Pre-fill Logic

### Priority Order (MVP):
```kotlin
1. If current workout has logged sets: Use last logged set
2. Else if history exists for this exercise: Use history Set N
3. Else: Empty (0)
```

**Plan values are NOT used for pre-fill** ← Key decision

### When Exercise is From Workout Plan:
- Plan values shown in Exercise Info Card: "📋 Today's Plan: 3×10 @ 60kg"
- Input fields still pre-fill from history
- User can reference plan while adjusting
- Backend tracks adherence for plan adaptation

---

## Data Models

### WorkoutExercise
```kotlin
data class WorkoutExercise(
    val id: String,
    val exerciseId: String,
    val exerciseName: String,
    val status: ExerciseStatus,  // QUEUED, IN_PROGRESS, COMPLETED, SKIPPED
    val sets: List<ExerciseSet>,
    val orderIndex: Int,  // User's addition order (never changes)

    // Plan context (nullable - only if from plan)
    val plannedSets: Int? = null,
    val plannedReps: Int? = null,
    val plannedWeight: Double? = null,
    val isFromPlan: Boolean = false
)
```

### ExerciseStatus
```kotlin
enum class ExerciseStatus {
    QUEUED,      // Not started, dimmed 80%
    IN_PROGRESS, // Sets being logged, green border
    COMPLETED,   // Finished with sets, checkmark
    SKIPPED      // Skipped with 0 sets, dimmed 60%, italic
}
```

### ExerciseSet
```kotlin
data class ExerciseSet(
    val setNumber: Int,
    val weight: Double,  // kg
    val reps: Int,
    val timestamp: Timestamp
)
```

---

## Business Logic

### Auto-save Draft
- On app exit/background: Save current workout state as "Draft"
- On crash/force quit: Workout data already saved incrementally
- Draft detection: Check for workouts with `status = Draft` on app launch

### Finish Workout
1. **Check for QUEUED exercises**: If any exercises are still QUEUED (untouched), show Incomplete Workout Confirmation Dialog
   - User can "Continue Workout" (go back) or "Finish Anyway" (proceed)
   - If "Finish Anyway": Mark all QUEUED exercises as SKIPPED
2. Mark all "In Progress" exercises as "Completed" (if they have logged sets)
3. Skipped exercises remain as "Skipped"
4. Set `endTime = now`
5. Calculate duration (`endTime - startTime`)
6. Update status to "Completed"
7. Sync to server in background (including skipped exercise data)
8. Clear draft status
9. Navigate to Workout Summary (shows completed AND skipped exercises)

**See:** [2.02-active-workout.md](../designer/2.02-active-workout.md) for Incomplete Workout Confirmation Dialog specification

### Redo Last Workout
1. Query last completed workout
2. Copy exercise list and set structure
3. Create new workout session with exercises in "Queued" state
4. Pre-populate expected sets based on last workout
5. User can still add/remove exercises or modify sets

---

## Post-MVP Enhancements

### Helper Tips for "Train on My Own" Path
**Priority:** Next iteration after MVP testing

**Context:** Users who choose "I'll train on my own" in onboarding bypass the guided path. They're confident but may not know app-specific UX patterns.

**Issue:**
- "Train on my own" ≠ "I know your app's UX patterns"
- First real workout post-onboarding might still cause confusion
- Current design: No helper tips (assumes confidence)
- Risk: Users struggle with basic interactions

**Recommendation:**
- Show **dismissible** tip on **first real workout only**
- Context-aware micro-guidance (e.g., "Tap any card to start logging")
- One-time only (dismiss = never show again)
- Non-blocking (can dismiss immediately)

**Implementation:**
- Flag: `hasSeenFirstWorkoutTips: Boolean`
- Show when: `first_time_completed = true` AND `workout_count = 1` AND flag `= false`
- Examples:
  - Exercise Picker: "Tap + to create custom exercises"
  - Active Workout: "Tap any exercise card to start logging sets"
  - Exercise Detail: "Your last workout data pre-fills to save time"

**Why Post-MVP:**
- Test base usability without training wheels first
- Gather data on actual confusion points
- Design targeted, data-driven tips
- Avoid over-engineering before knowing real pain points

**Success Metric:**
- User testing reveals specific friction points → Tips address exact points
- Tips shown to <10% of users (only truly confused ones need them)

---

### UX Enhancements (2025-11-25 Review)
**Priority:** Post-MVP, based on user feedback

**Exercise Detail Screen Enhancements:**
| Feature | Description | Rationale |
|---------|-------------|-----------|
| Rest Timer | Countdown timer after logging set (1-3 min configurable) | Expected feature in workout apps, currently users use phone timer |
| Quick Weight Adjust | [-2.5] [weight] [+2.5] buttons for micro-adjustments | Faster than keyboard input for small changes |
| Historical Progression | Show trend over time, not just "last workout" | Users want to see progress |
| Plate Calculator | Show which plates to load on bar | Quality of life for barbell exercises |

**Active Workout Screen Enhancements:**
| Feature | Description | Rationale |
|---------|-------------|-----------|
| Last Set Preview | Show "Last: 80kg × 10" on exercise cards | Quick reference without tapping into detail |
| Manual Reorder | Drag-to-reorder exercises after adding | Flexibility if user adds in wrong order |
| Superset/Circuit Support | Group exercises for alternating sets | Common training method |

**Exercise Detail → Next Exercise Flow:**
| Feature | Description | Rationale |
|---------|-------------|-----------|
| "Continue to Next" Button | After finishing exercise, quick nav to next | Reduces round-trips to Active Workout hub |
| Next Exercise Preview | Show "Next up: Bench Press" at bottom of screen | Context without leaving screen |

**Workout Summary Screen Enhancements:**
| Feature | Description | Rationale |
|---------|-------------|-----------|
| Workout Rating | "How was this workout?" with emoji scale | Subjective data for plan adaptation |
| Notes Field | Free text for context ("felt weak", "new gym") | User context for future reference |
| "Redo This Workout" | Quick action to start same workout again | Common for consistent routines |

**Why Post-MVP:**
- MVP flow is solid, no critical UX gaps
- Gather real user feedback before adding complexity
- Prioritize based on actual pain points, not assumptions

---

## Technical Considerations

### Offline-First Architecture
- All workout logging operations work offline (local database)
- Exercise library cached locally (SQLDelight recommended)
- Background sync when connection available (exponential backoff retry)
- Show subtle "Offline" indicator (non-blocking)
- Queue sync operations for later

### Performance
- Exercise list: Use LazyColumn for large lists
- Set logging: Debounce rapid taps to prevent duplicate saves
- Animations: Use compose animations for smooth state transitions
- Image loading: If exercise images added, use lazy loading

### Color Reference
All colors sourced from: `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`

---

## Screen References

| Step | Screen | File Reference |
|------|--------|----------------|
| 1 | Exercise Picker | [2.01-exercise-picker.md](../designer/2.01-exercise-picker.md) |
| 2 | Active Workout Hub | [2.02-active-workout.md](../designer/2.02-active-workout.md) |
| 3 | Exercise Detail (Set Logger) | [2.03-exercise-detail.md](../designer/2.03-exercise-detail.md) |
| 4 | Workout Summary | [2.04-workout-summary.md](../designer/2.04-workout-summary.md) |

> **Complete navigation flows**: See [00-app-overview.md](00-app-overview.md)

---

## Revision History

### 2025-11-25: UX Review & Post-MVP Planning
- ✅ **Reviewed:** Complete workout flow (Exercise Picker → Active Workout → Exercise Detail → Summary)
- ✅ **Confirmed:** MVP specs are solid, no critical UX gaps
- ✅ **Documented:** 10+ post-MVP enhancement opportunities (rest timer, supersets, workout rating, etc.)
- ✅ **Deferred:** Rest timer, manual reorder, workout notes/rating - all post-MVP
- ✅ **Decision:** Skipped exercise opacity stays at 60% (no change needed)

### 2025-01-05: Workflow Review & Critical UX Fixes
- ✅ **Fixed:** Hide "Finish Workout" when 0 exercises (Issue #2)
- ✅ **Fixed:** Exercise ordering - no auto-reordering, preserve user's addition order (Issue #5)
- ✅ **Fixed:** Single "In Progress" exercise at a time with smart state transitions
- ✅ **Added:** Skip Exercise functionality with confirmation dialogs (Issue #6)
- ✅ **Added:** SKIPPED exercise status (new state)
- ✅ **Added:** Plan deviation dialog (gentle accountability when sets < planned)
- ✅ **Added:** Skipped exercises in Workout Summary
- ✅ **Updated:** Pre-fill logic - workout plan shown as reference only (Issue #3)
- ✅ **Updated:** Empty state with "Add Exercise" as primary action
- ✅ **Updated:** Exercise state machine with un-skip functionality
- ✅ **Restructured:** Documentation into main + sub-files (following onboarding convention)

### 2025-01-11: UX Review & Redesign
- ❌ **Removed:** Exercise Action Sheet modal (eliminated friction)
- ❌ **Removed:** Helper tip banner (no hand-holding for "train on my own" path)
- ✅ **Added:** FAB with animated hide/show for custom exercise creation
- ✅ **Added:** Context-aware toast system (teaches once, then simplifies)
- ✅ **Added:** Visual feedback (checkmarks) on added exercises
- ✅ **Added:** Hybrid search + category filtering
- ✅ **Added:** Bottom sheet for custom exercise creation (one field only)
- ✅ **Added:** Exercise deletion with confirmation dialog (× icon on cards)
- ✅ **Updated:** Active Workout bottom bar (stacked buttons, not side-by-side)
- ✅ **Updated:** Back arrow behavior (auto-save draft)
