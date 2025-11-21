---
tags:
  - feature
  - workout-plan
name: Workout Plan
status: "[[Statuses/Done]]"
---

# Workout Plan Feature

> **Status**: Approved for MVP (January 1, 2025)
> **Last Updated**: 2025-11-16
> **Navigation**: See [00-app-overview.md](00-app-overview.md) for complete navigation flows

---

## Overview

### Purpose
The Workout Plan feature enables users to follow structured, multi-day training programs with automatic progression tracking, adherence monitoring, and cycle-based progression. Plans can be created via AI recommendation, auto-detection from workout history, or manual creation.

### User Problem
**Target User**: Intermediate lifters who want structured training

**Problems Solved**:
1. **Decision fatigue**: "What should I do today?" → Structured plan tells them
2. **Inconsistent training**: Random workouts → Consistent routine with progression
3. **No progress tracking**: Can't compare Week 1 vs Week 2 → Automatic cycle tracking
4. **Manual plan setup tedium**: 20-30 min to set up plan in other apps → AI does it or auto-detects it

### Competitive Advantage
- ✅ **Only app with 3 plan sources**: AI-recommended + Auto-detected + Manual
- ✅ **Only app with pattern detection**: Auto-generates plan from workout history
- ✅ **Anytime editing**: Edit plans before and after activation (other apps lock plans)
- ✅ **Draft auto-save**: Resume plan creation where you left off (other apps lose progress)
- ✅ **Cycle comparison**: Compare Week 1 vs Week 2 performance automatically

---

## Related Areas

*   **[[1-onboarding]]**: Initial source of AI-Recommended plans (Path A).
*   **[[3-dashboard]]**: Primary entry point for viewing active plans and creating new ones.
*   **[[2-workout_process]]**: Execution context where plans are performed.
*   **[[AI-01-pattern-recognition]]**: Logic for auto-detecting plans from history.
*   **[[4-authentication]]**: User data sync for plans across devices.

---

## Plan Structure (Backend-Aligned)

### Data Models

Based on backend `CreatePlanRequest` structure from `draft.md`:

```kotlin
data class WorkoutPlan(
    val id: String,
    val userId: String,
    val name: String,                    // Required, e.g., "PPL Strength"
    val description: String?,            // Optional
    val days: List<WorkoutDay>,          // 1-30 days (dayIndex 0-29)
    val status: PlanStatus,              // ACTIVE, INACTIVE, ARCHIVED
    val createdAt: Timestamp,
    val source: PlanSource,              // AI_RECOMMENDED, AUTO_DETECTED, MANUAL

    // Progression tracking
    val currentDayIndex: Int = 0,        // Which day user is on (0-29)
    val currentCycle: Int = 1,           // Week 1, Week 2, etc.
    val totalWorkoutsCompleted: Int = 0,
    val lastWorkoutDate: Timestamp?
)

data class WorkoutDay(
    val dayIndex: Int,                   // 0-29 (max 30-day plans)
    val name: String?,                   // Optional, e.g., "Upper Body"
    val type: DayType,                   // WORKOUT or REST
    val exercises: List<PlannedExercise>,// Empty for REST days
    val notes: String?                   // Optional
)

data class PlannedExercise(
    val exerciseId: String,              // Reference to exercise library
    val orderIndex: Int,                 // Order within day (0, 1, 2...)
    val targetSets: Int,                 // Required, e.g., 3
    val targetReps: Int?,                // Optional, e.g., 10
    val targetWeight: Double?,           // Optional, e.g., 80.0 (kg)
    val notes: String?                   // Optional
)

enum class PlanStatus {
    ACTIVE,      // Currently following this plan
    INACTIVE,    // Saved but not active (can preview, switch to, or delete)
    ARCHIVED     // Completed or abandoned (hidden from main list)
}

enum class PlanSource {
    AI_RECOMMENDED,  // From Path A onboarding recommendations
    AUTO_DETECTED,   // From Pattern Recognition AI
    MANUAL           // User created from scratch
}

enum class DayType {
    WORKOUT,  // Regular training day with exercises
    REST      // Rest/recovery day (no exercises)
}
```

### Plan Draft (Auto-Save for Manual Creation)

```kotlin
data class PlanDraft(
    val id: String,
    val userId: String,
    val createdAt: Timestamp,
    val lastModifiedAt: Timestamp,
    val status: PlanDraftStatus,

    // Partial plan data (might be incomplete during creation)
    val name: String?,
    val description: String?,
    val days: List<WorkoutDay>           // Can be empty or partial
)

enum class PlanDraftStatus {
    IN_PROGRESS,  // User still creating (auto-saved locally)
    COMPLETED,    // User finished and activated plan (draft deleted)
    ABANDONED     // Draft older than 30 days or user discarded
}
```

### Plan Progress Tracking

```kotlin
data class PlanProgress(
    val planId: String,
    val currentDayIndex: Int,            // User is on Day 3 (next workout)
    val currentCycle: Int,               // Week 2
    val totalWorkoutsCompleted: Int,     // 10 workouts completed total
    val lastWorkoutDate: Timestamp?,
    val cycleAdherence: List<CycleAdherence> // Per-cycle stats
)

data class CycleAdherence(
    val cycleNumber: Int,                // Week 1, Week 2, etc.
    val completedDays: Int,              // 6 of 7 days completed
    val totalDays: Int,                  // 7 days in plan
    val adherencePercentage: Float,      // 85.7%
    val averageExerciseCompletion: Float // 92% (avg exercises completed per workout)
)
```

### Workout Log Plan Reference

Extension to existing `Workout` model:

```kotlin
data class Workout(
    // ... existing fields

    // NEW: Plan tracking
    val planId: String?,           // Which plan was this from? (null for custom workouts)
    val planDayIndex: Int?,        // Which day? (0-29, null for custom)
    val planCycleNumber: Int?,     // Week 1, Week 2, etc. (null for custom)
    val planAdherence: PlanDayAdherence? // Did user follow plan?
)

data class PlanDayAdherence(
    val plannedExercises: Int,     // 8 exercises in plan
    val completedExercises: Int,   // 7 exercises logged
    val skippedExercises: Int,     // 1 exercise skipped
    val adherencePercentage: Float // 87.5%
)
```

---

## Plan Sources (3 Ways to Get a Plan)

### Source 1: AI-Recommended Plans

**Entry Point**: Onboarding Path A (Personalization Questions)

**Flow**:
1. User answers personalization questions (1.05)
2. AI recommends 3 workout plans (1.06)
3. User selects one and previews it (1.07)
4. User can edit plan before accepting (modify exercises, targets)
5. User accepts plan → Plan status: ACTIVE
6. Dashboard transitions to Type 3 (shows active plan)

**Screens**:
- ✅ Already designed: 1.05, 1.06, 1.07
- ⚠️ Need to extend 1.07: Add edit mode

**Source Type**: `PlanSource.AI_RECOMMENDED`

---

### Source 2: Auto-Detected Plans (Pattern Recognition)

**Entry Point**: Pattern Recognition AI (after 2-4 weeks of custom workouts)

**Flow**:
1. User logs custom workouts for 2-4 weeks
2. Backend detects pattern (naive check → AI analysis)
3. Dashboard shows detection banner: "AI detected your plan!"
4. User taps "View Plan" → Plan Preview
5. User can edit detected plan (AI might miss exercises)
6. User accepts plan → Plan status: ACTIVE
7. Dashboard transitions to Type 3

**Screens**:
- 🆕 Detection Banner (Dashboard component)
- ✅ Plan Preview (reuse 1.07 with edit mode + AI badges)

**Source Type**: `PlanSource.AUTO_DETECTED`

**Reference**: [[AI-01-pattern-recognition]]

---

### Source 3: Manual Plan Creation (Plan Builder)

**Entry Point**: Dashboard "Create Plan" button (Type 2 users without active plan) or Plan Library "New Plan" button.

**Core Requirements**:
1.  **Zero-Loss Drafts**: If the user closes the app, the draft MUST be saved.
2.  **Non-Linear Editing**: User can jump between naming, adding days, and editing exercises.
3.  **Smart Defaults**: When adding a day, default to the next logical day type (e.g., after 3 workouts, suggest Rest).

**Detailed Flow**:

1.  **Initialization**:
    *   User taps "Create Plan".
    *   System creates a `PlanDraft` (ID generated, Status: IN_PROGRESS).
    *   **Screen**: `5.01-plan-builder-setup` (Name & Description).

2.  **Day Management (The "Skeleton")**:
    *   User sees a list of days (initially empty).
    *   Action: "Add Day".
    *   User selects type: "Workout" or "Rest".
    *   **Screen**: `5.02-plan-builder-days`.

3.  **Day Configuration (The "Meat")**:
    *   User taps a "Workout" day.
    *   User adds exercises via `2.01-exercise-picker`.
    *   User configures targets (Sets, Reps, Weight) via `5.03-target-config`.
    *   *Constraint*: Must have at least 1 exercise to be valid.

4.  **Finalization**:
    *   User taps "Save & Activate" or "Save for Later".
    *   System validates (Name exists? >0 days?).
    *   System converts `PlanDraft` to `WorkoutPlan`.
    *   Draft is deleted.

**Screens**:
- 🆕 `5.02-plan-builder-setup`: Name, Description, Cover Image (optional).
- 🆕 `5.03-plan-builder-days`: List of days, reorder drag-and-drop, swipe to delete.
- 🆕 `5.04-target-config`: Bottom sheet for setting sets/reps/weight per exercise.
- ✅ `2.01-exercise-picker`: Reuse existing.
- ✅ `1.07-workout-plan-preview`: Reuse for final review.

**Source Type**: `PlanSource.MANUAL`

**Auto-Save Strategy**:
- Trigger: `onPause`, `onStop`, and every 5 seconds of inactivity.
- Storage: Local SQLite `plan_drafts` table.
- Restoration: When opening Plan Builder, check for existing drafts. If found, prompt: "Resume 'My Plan'?"

---

## Plan Execution (Following a Plan)

### Daily Workout Flow

**User on Dashboard Type 3:**

**Dashboard shows**:
- Program Card: "Your Program: PPL Strength" + "Week 2" + "Day 3: Legs" + Progress bar (9/14)
- **Action**: "Manage Plan" (small text button) -> Opens Plan Library with active plan selected.
- Primary Action Button: "Start Day 3: Legs Workout"

**User WITHOUT Active Plan (Type 2):**
- **Plan Hub Card** (replaces Program Card):
    - **State B1 (Empty Library)**:
        - Title: "Need structure?"
        - Action 1 (Primary): "Get AI Recommendations"
        - Action 2 (Secondary): "Build from Scratch"
    - **State B2 (Populated Library)**:
        - Title: "Workout Plans"
        - Action 1 (Primary): "Open Library" (Access drafts/saved plans)
        - Action 2 (Secondary): "Create New"
- Primary Action Button: "Start New Workout" (Custom)

**User taps "Start Day 3"**:
1. System fetches plan Day 3 exercises
2. Creates Active Workout with exercises pre-loaded (Queued state)
3. Navigates to Active Workout (2.02)

**Active Workout**:
- 6 exercise cards (from plan Day 3)
- User taps exercise → Exercise Detail (2.03)

**Exercise Detail (Plan Mode)**:
- Exercise Info Card shows:
  - **"📋 Plan: 3 sets × 10 reps @ 100kg"** (from plan target)
  - **"Last Week: 102.5kg × 10 reps"** (from Week 1, Day 3 history)
- Pre-fill inputs: 102.5kg (from history, not plan target)
- User logs sets

**Workout Summary**:
- Shows plan adherence: "5/6 exercises completed (83%)"
- System saves workout with plan reference:
  - `planId`, `planDayIndex: 2`, `planCycleNumber: 2`

**Dashboard updates**:
- Progress bar: 10/14
- Next day: "Start Day 4: Push Workout"

---

### REST Day Flow

**User on Dashboard Type 3:**

### REST Day Flow

**User on Dashboard Type 3:**

**Dashboard shows**:
- REST Day Card (instead of Program Card):
- **Visuals**: Calming colors (blue/purple gradients), "Rest & Recover" imagery.
- **Copy**: "Muscles grow during rest, not just training."

**User Actions**:
1.  **Do Nothing (Ideal)**: User opens app, sees it's rest day, closes app.
    *   *Logic*: System auto-completes the day at midnight? OR System just waits for user to "Confirm Rest"?
    *   *Decision*: **Auto-progression**. If today is a Rest Day, and the user does *not* log a workout, the system automatically advances the plan to the next day at 4:00 AM next morning.
2.  **Log Active Recovery**: User does yoga/stretching.
    *   Action: "Log Activity" -> Select "Yoga".
    *   Result: Day marked "Completed with Activity".
3.  **Skip Rest (Not Recommended)**:
    *   Action: "Skip to Next Workout".
    *   Result: Dashboard immediately shows Day X+1.
    *   *Warning*: "Skipping rest can lead to overtraining."

**Progression Logic**:
- **Standard**: Time-based. If Day = REST, wait 24h, then advance.
- **Manual Advance**: User taps "I'm rested, let's train" -> Advances immediately.
- **Missed Workout**: If Day = WORKOUT, and user does nothing, plan does *not* advance. It waits.


---

### Cycle Progression

**Scenario: 7-day PPL plan**

Days: Push / Pull / Legs / Push / Pull / Legs / REST

**User completes Day 7 (last day of cycle)**:
- System updates: `currentDayIndex = 0`, `currentCycle = 2`
- Dashboard shows: "Start Day 1: Push Workout (Week 2)"

**Infinite repeat**:
- After Week 2 → Week 3
- After Week 3 → Week 4
- Continues until user switches plans or deactivates

**No end date** (for MVP):
- User manually switches plans when ready
- Future: Add "Plan Duration" feature (4-week programs)

---

## Plan Management

### Multiple Plans (One Active at a Time)

**User can have**:
- Multiple saved plans (ACTIVE: 1, INACTIVE: 0-N, ARCHIVED: 0-N)
- Only ONE active plan at a time

**Example**:
```
Plans:
  - "PPL Strength" (ACTIVE) ← User is following this
  - "Upper/Lower Hypertrophy" (INACTIVE) ← Saved for later
  - "Full Body Beginner" (INACTIVE) ← Saved for later
```

**User can**:
- Preview any plan (tap → Plan Preview screen)
- Edit any plan (active or inactive, anytime)
- Delete inactive plans
- Switch active plan (deactivate current, activate another)

### Plan Library (My Plans)

**Entry Point**: Dashboard → "My Plans" (or Settings)

**Core Requirements**:
1.  **Visual Hierarchy**: The **Active Plan** must be visually distinct (e.g., top card, highlighted border, "Active" badge).
2.  **Filtering/Sorting**:
    *   *Default*: Active plan first, then most recently modified.
    *   *Tabs*: "All", "Created", "Downloaded/AI".
3.  **Quick Actions**:
    *   Swipe to delete (Inactive plans only).
    *   Long-press to rename.
    *   Tap to Preview (and then Activate).

**Detailed Flow - Switching Plans**:
1.  User taps an Inactive Plan (e.g., "Upper/Lower").
2.  **Screen**: `1.07-workout-plan-preview` (Mode: Inactive).
3.  User taps "Start this Plan".
4.  **Critical Confirmation**:
    *   "Activate 'Upper/Lower'?"
    *   "Current plan 'PPL' will be paused."
    *   "Progress for 'Upper/Lower' starts at Day 1."
5.  User confirms.
6.  System updates `activePlanId` in User Profile.
7.  Dashboard refreshes to show new plan.

**Screens**:
- 🆕 `5.01-plan-library`: List view of all plans.

---

### Plan Switching Logic

**Scenario**: User wants to switch from "PPL Strength" to "Upper/Lower Hypertrophy"

**Rules**:
1.  **One Active Plan**: Only one plan can be `ACTIVE` at a time.
2.  **Progress Reset**: Switching to a plan *always* resumes it from where it was left off (or Day 1 if never started). It does *not* carry over progress from the previous plan.
3.  **History Preservation**: The old plan (`PPL Strength`) becomes `INACTIVE`. Its history (logs) is preserved. If the user switches back to PPL later, they can choose to "Resume" or "Restart".

**Edge Case - "Restarting" a Plan**:
- User finishes a 12-week program.
- User wants to do it again.
- Action: "Restart Plan" button.
- Result: `currentCycle` increments (or resets to 1 if desired), `currentDayIndex` = 0.

---

### Plan Editing (Anytime)

**All plans are editable** (before and after activation):

**User can edit**:
- Plan name, description
- Add/remove days
- Change day type (WORKOUT ↔ REST)
- Add/remove exercises
- Change exercise targets (sets, reps, weight)
- Reorder exercises

**Editing active plan**:
- Changes apply to **future workouts only**
- Historical workout logs preserve old plan structure (non-destructive)

**Editing inactive plan**:
- Just saves changes
- If user activates later, uses updated structure

**Example**:
```
User on Week 2, Day 3 of "PPL Strength"
  ↓
Edits plan: Adds Face Pulls to Pull day (Day 2)
  ↓
Week 3, Day 2: Face Pulls appears in workout (change applied)
  ↓
Historical Week 1 & 2 workouts: No Face Pulls (unchanged)
```

---

## Plan Adherence Tracking

### Completion Rules

**Question**: What counts as "completing" a plan day?

**Answer**: **Any progress counts** (Option A from requirements)

**Rule**:
- User logs at least 1 set → Day marked complete
- Progression advances to next day

**Example**:
```
Day 1 has 8 exercises
User logs:
  - 6 exercises completed (all sets)
  - 1 exercise skipped (0 sets)
  - 1 exercise not attempted (user forgot)
  ↓
Adherence: 6/8 exercises = 75%
  ↓
Day 1 still marked COMPLETE (because user logged something)
  ↓
Next workout: Day 2
```

**Strict adherence** (future enhancement):
- Track adherence percentage
- Show stats: "Week 1: 85% adherence"
- But don't block progression

---

### Pre-Fill Logic (Plan vs History)

**Option C (from requirements): Show both plan target + history**

**Scenario**: User on Week 2, Day 1 (Bench Press)

**Plan target**: 3 sets × 10 reps @ 80kg
**Last performance** (Week 1, Day 1): 3 sets × 10 reps @ 82.5kg (user progressed!)

**Exercise Detail screen shows**:
- **Info Card**:
  - "📋 Plan: 3 sets × 10 reps @ 80kg"
  - "Last Week: 82.5kg × 10 reps"
- **Pre-fill inputs**: Weight: 82.5kg, Reps: 10 (from history, not plan)

**User can reference both**:
- Plan tells them original target
- History shows their actual progression
- They decide whether to increase weight further

**Implementation**:
```kotlin
fun getExercisePreFill(
    planExercise: PlannedExercise,
    lastPerformance: ExerciseLog?
): ExercisePreFill {
    return ExercisePreFill(
        // Pre-fill inputs with history (or plan if no history)
        weight = lastPerformance?.averageWeight ?: planExercise.targetWeight,
        reps = lastPerformance?.averageReps ?: planExercise.targetReps,
        sets = planExercise.targetSets, // Always use plan sets

        // Display both in info card
        planTarget = PlanTarget(
            sets = planExercise.targetSets,
            reps = planExercise.targetReps,
            weight = planExercise.targetWeight
        ),
        lastPerformance = lastPerformance
    )
}
```

---

## Out-of-Order Day Execution

**Question**: Can user do Day 3 before Day 2?

**Answer**: **Allowed but not recommended** (Option from requirements)

**Scenario**: User on Day 2 (Pull), but gym pull equipment is taken

**User wants to do Day 3 (Legs) instead**

**Flow**:
1. User goes to Plan Library → Taps active plan
2. Plan Preview shows all days (Day 1, Day 2, Day 3...)
3. Each day card has "Start This Day" button
4. User taps "Start This Day" on Day 3 (even though they're on Day 2)
5. System shows warning:
   - "You're about to start Day 3: Legs"
   - "Current progression is Day 2: Pull"
   - "Starting out of order may affect your training balance."
   - [Cancel] [Start Anyway]
6. User taps "Start Anyway"
7. Active Workout loads Day 3 exercises
8. User completes Day 3
9. System marks: Day 3 complete, Day 2 skipped
10. Dashboard updates: "Start Day 4: Push" (progression follows what user did)

**Progression logic**: Follows what user actually did (not strict order)

---

## Integration Points

### Dashboard Feature Integration

**Type 2 → Type 3 Transition** (User activates first plan):

**Before** (Type 2):
- Program Card: "No active plan" with "Get Recommendations" CTA
- Primary Action Button: "Start New Workout"

**After** (Type 3):
- Program Card: "Your Program: PPL Strength" + progress bar
- Primary Action Button: "Start Day 1: Push Workout"

**Components affected**:
- Program Card Slot (content changes)
- Primary Action Button (text + behavior changes)
- Recent Workouts (shows plan badges 📋)

**Reference**: [3-dashboard.md](3-dashboard.md)

---

### Workout Process Integration

**Active Workout Screen** (2.02):
- **No changes to screen design**
- Receives pre-loaded exercises (from plan day instead of manual selection)
- User can still add/remove exercises during workout (plan editing)

**Exercise Detail Screen** (2.03):
- **New**: Exercise Info Card shows plan target + last performance
- **Modified**: Pre-fill logic uses history (not plan target)
- Otherwise identical to custom workouts

**Workout Summary Screen** (2.04):
- **New**: Shows plan adherence indicator: "7/8 exercises completed (87%)"
- Otherwise identical to custom workouts

**Reference**: [2-workout_process.md](dump/ux-designer/2-workout_process.md)

---

### Pattern Recognition Integration

**Detection Banner** (Dashboard):
- NEW component on Dashboard Type 2
- Shows when `DetectedPlan.status == PENDING`
- User taps "View Plan" → Plan Preview (auto-detected mode)

**Plan Preview** (auto-detected):
- Shows AI badges: "🤖 Auto-Detected Plan"
- Shows confidence: "85% match"
- Shows source: "Based on 9 workouts from Oct 1-21"
- User can edit before accepting

**After acceptance**:
- DetectedPlan converted to WorkoutPlan
- Dashboard transitions to Type 3

**Reference**: [AI-01-pattern-recognition.md](AI-01-pattern-recognition.md)

---

## Screen Inventory

### Already Designed

| Screen | File | Status | Notes |
|--------|------|--------|-------|
| Plan Preview (AI Recommendations) | 1.07-workout-plan-preview.md | ✅ Complete | Need to extend with edit mode |
| Dashboard Type 3 | 3-dashboard.md | ✅ Complete | Shows active plan |
| Active Workout | 2.02-active-workout.md | ✅ Complete | Receives pre-loaded exercises |
| Exercise Detail | 2.03-exercise-detail.md | ✅ Complete | Need to add plan info card |
| Workout Summary | 2.04-workout-summary.md | ✅ Complete | Need to add adherence indicator |

---

### Need to Design (NEW Screens)

| Screen | Priority | Purpose | Complexity |
|--------|----------|---------|------------|
| **Plan Builder (Step 1)** | HIGH | Manual plan creation entry (name, description) | Medium |
| **Day List Editor (Step 2)** | HIGH | Add/remove/reorder days | Medium |
| **Day Editor** | HIGH | Configure day (type, name, exercises) | Medium |
| **Exercise Target Config** | HIGH | Bottom sheet to set targets per exercise | Low |
| **Plan Library** | MEDIUM | View all saved plans (active, inactive, archived) | Medium |
| **REST Day Card** | MEDIUM | Dashboard component for rest days | Low |
| **Detection Banner** | HIGH | Dashboard component for Pattern Recognition | Low |

---

### Screen Extensions (Modify Existing)

| Screen | Modification | Priority |
|--------|--------------|----------|
| **Plan Preview (1.07)** | Add edit mode + AI badges | HIGH |
| **Dashboard (3.dashboard.md)** | Add detection banner, REST day card | HIGH |
| **Exercise Detail (2.03)** | Add plan info card (target + history) | HIGH |
| **Workout Summary (2.04)** | Add adherence indicator | MEDIUM |

---

## Alternative States Summary

### REST Day States

**State 1: REST day in sequence**
- Dashboard shows calm REST day card
- User can take rest, skip ahead, or start custom workout
- Progression continues (REST day counted)

**State 2: Multiple consecutive REST days**
- Day 3: REST → "Skip to Day 4"
- Day 4: REST → "Skip to Day 5"
- Not "Skip to Day 5" directly (respects each day)

---

### Plan Creation Draft States

**State 1: Creating plan, exits mid-creation**
- Auto-save creates draft (status: IN_PROGRESS)
- User returns → "Resume creating 'Plan Name'?" banner

**State 2: Multiple drafts**
- User has 2 drafts in progress
- Plan Builder shows: "Resume 'PPL'?" + "Resume 'Upper/Lower'?" + "Start New"

**State 3: Abandoned drafts**
- Drafts older than 30 days auto-deleted
- Or user manually deletes: "Discard Draft" button

---

### Plan Switching States

**State 1: Switch plans mid-cycle**
- User on Week 2, Day 3 of "PPL Strength"
- Switches to "Upper/Lower"
- Progress resets to Day 1, Cycle 1
- Old plan history preserved

**State 2: No active plan (user deactivated)**
- User deactivates plan without activating another
- Dashboard transitions to Type 2 (no active plan)

---

### Edge Cases

**Edge 1: Empty plan (0 days)**
- Plan Builder: "Preview Plan" button disabled until at least 1 day added

**Edge 2: Plan with only REST days**
- Allowed (e.g., deload week)
- Dashboard shows REST day card every day

**Edge 3: Exercise deleted from library**
- Plan references exercise that no longer exists
- Show placeholder: "Exercise unavailable" + "Replace Exercise" button

**Edge 4: User skips entire plan cycle**
- User doesn't train for 2 weeks (vacation)
- Returns, still on same day (no auto-advancement)
- Continues where they left off

---

## Implementation Notes

### Auto-Save Logic (Plan Creation)

```kotlin
class PlanBuilderViewModel {
    private val draftRepository: PlanDraftRepository
    private var currentDraft: PlanDraft? = null

    init {
        // On Plan Builder open, check for existing drafts
        val drafts = draftRepository.getInProgressDrafts()
        if (drafts.isNotEmpty()) {
            // Show resume prompt or auto-resume most recent
            currentDraft = drafts.maxBy { it.lastModifiedAt }
        } else {
            // Create new draft
            currentDraft = PlanDraft(
                id = generateId(),
                status = PlanDraftStatus.IN_PROGRESS,
                name = null,
                days = emptyList()
            )
            draftRepository.save(currentDraft)
        }
    }

    fun updatePlanName(name: String) {
        currentDraft = currentDraft?.copy(
            name = name,
            lastModifiedAt = now()
        )
        autoSave()
    }

    fun addDay(day: WorkoutDay) {
        currentDraft = currentDraft?.copy(
            days = currentDraft.days + day,
            lastModifiedAt = now()
        )
        autoSave()
    }

    private fun autoSave() {
        // Debounced save (max once per 2 seconds)
        viewModelScope.launch {
            delay(2000)
            currentDraft?.let { draftRepository.save(it) }
        }
    }

    fun finishPlan() {
        // Convert draft to real plan
        val plan = currentDraft?.toWorkoutPlan()
        plan?.let {
            planRepository.save(it)
            draftRepository.delete(currentDraft.id) // Delete draft
        }
    }
}
```

---

### Progression Tracking

```kotlin
class PlanProgressService {

    suspend fun completeWorkout(workout: Workout) {
        // If workout is from a plan, update progression
        workout.planId?.let { planId ->
            val plan = planRepository.get(planId)
            val progress = progressRepository.get(planId)

            // Calculate next day
            val nextDayIndex = if (workout.planDayIndex == plan.days.lastIndex) {
                // Completed last day → start new cycle
                0
            } else {
                // Move to next day
                workout.planDayIndex + 1
            }

            // Calculate next cycle
            val nextCycle = if (nextDayIndex == 0) {
                progress.currentCycle + 1
            } else {
                progress.currentCycle
            }

            // Update progress
            progressRepository.update(
                planId = planId,
                currentDayIndex = nextDayIndex,
                currentCycle = nextCycle,
                totalWorkoutsCompleted = progress.totalWorkoutsCompleted + 1,
                lastWorkoutDate = now()
            )

            // Update plan
            planRepository.update(
                planId = planId,
                currentDayIndex = nextDayIndex,
                currentCycle = nextCycle
            )
        }
    }
}
```

---

### Plan Adherence Calculation

```kotlin
fun calculatePlanAdherence(
    workout: Workout,
    planDay: WorkoutDay
): PlanDayAdherence {
    val plannedExercises = planDay.exercises.size
    val completedExercises = workout.exercises.count { it.sets.isNotEmpty() }
    val skippedExercises = workout.exercises.count { it.status == ExerciseStatus.SKIPPED }

    val adherencePercentage = if (plannedExercises > 0) {
        (completedExercises.toFloat() / plannedExercises) * 100
    } else {
        100f // No exercises planned = 100% adherence
    }

    return PlanDayAdherence(
        plannedExercises = plannedExercises,
        completedExercises = completedExercises,
        skippedExercises = skippedExercises,
        adherencePercentage = adherencePercentage
    )
}
```

---

## Color Reference

All colors sourced from: `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`

**Used in this feature**:
- `LightSurface` (#FFFFFF) - Card backgrounds
- `LightBackground` (#F3F3F8) - Screen backgrounds
- `LimeGreenDark` (#A8E6CF) - Primary buttons, plan badges
- `LimeGreenBright` (#CBFFC9) - Success indicators
- `TextOnLight` (#000000) - Primary text
- `TextOnLightSecondary` (#2D2D2D) - Secondary text
- `TextOnLightTertiary` (#666666) - Tertiary text

---

## Success Metrics

### Feature Adoption
- % of users who activate at least one plan (any source)
- % breakdown by source (AI / Auto-Detected / Manual)
- Average time to first plan activation (from app install)
- Plan creation abandonment rate (drafts not completed)

### User Engagement
- Average plan cycle duration (how long users follow a plan)
- Plan switching frequency (how often users change plans)
- Plan editing frequency (how often users modify plans)
- Average adherence percentage (exercises completed vs planned)

### Business Impact
- User retention (plan users vs non-plan users)
- Workout frequency (plan users vs non-plan users)
- Feature satisfaction (NPS for plan feature)

---

## Future Enhancements (Post-MVP)

### Phase 2 Features
- **Plan templates library**: Browse community-created plans
- **Plan sharing**: Share plans with friends
- **Plan duplication**: Duplicate existing plan to modify
- **Defined plan duration**: Plans with end dates (4-week programs)
- **Deload weeks**: Auto-reduce intensity every 4th week
- **Exercise substitution suggestions**: "Can't do bench? Try dumbbell press"

### Phase 3 Features
- **Progressive overload automation**: Auto-increase weight/reps based on performance
- **Plan analytics dashboard**: Compare cycles, visualize progression
- **Multi-phase programs**: Strength phase → Hypertrophy phase → Deload
- **Trainer-created plans**: Trainers can create plans for clients

---

## Design Status

| Component | Status | Designer | Developer | Notes |
|-----------|--------|----------|-----------|-------|
| Plan Preview (edit mode) | 🔴 Not Started | - | - | Extend 1.07 |
| Plan Builder (Step 1) | 🔴 Not Started | - | - | New screen |
| Day List Editor (Step 2) | 🔴 Not Started | - | - | New screen |
| Day Editor | 🔴 Not Started | - | - | New screen |
| Exercise Target Config | 🔴 Not Started | - | - | Bottom sheet |
| Plan Library | 🔴 Not Started | - | - | New screen |
| REST Day Card | 🔴 Not Started | - | - | Dashboard component |
| Detection Banner | 🔴 Not Started | - | - | Dashboard component |
| Dashboard Extensions | 🔴 Not Started | - | - | Modify existing |
| Exercise Detail Extensions | 🔴 Not Started | - | - | Add plan info card |
| Backend API | 🔴 Not Started | - | - | Plan CRUD + progression |
| Auto-save logic | 🔴 Not Started | - | - | Draft management |

---

## Related Documents

- [00-app-overview.md](00-app-overview.md) - App structure and navigation
- [3-dashboard.md](3-dashboard.md) - Dashboard Type 2 & Type 3 states
- [2-workout_process.md](2-workout_process.md) - Active Workout integration
- [AI-01-pattern-recognition.md](AI-01-pattern-recognition.md) - Auto-detection feature
- [1.07-workout-plan-preview.md](../designer/1.07-workout-plan-preview.md) - Existing preview design


---

**Last Updated**: 2025-11-16
**Next Steps**:
1. Design Plan Builder screens (manual creation flow)
2. Extend Plan Preview with edit mode
3. Design Plan Library screen
4. Implement backend API (plan CRUD, progression tracking)
