---
tags:
  - feature
  - overview
  - version/mvp
  - version/v1.1
name: App Overview
status: "[[Statuses/Done]]"
---

# Shapyfy App Overview & Navigation

**Purpose**: Single source of truth for project vision, product scope, app structure, feature relationships, and navigation flows.

---

## Project Vision & Context

### Project Name
**Shapyfy** - Strength training tracker with offline-first architecture

### Target Audience

#### Short-term Target (MVP)
People who are into strength training and want to track their progress.

**User Characteristics**:
- Gym-goers with limited time between sets (1-3 minutes rest)
- Users in gyms with poor internet connectivity
- People who want quick, intuitive workout logging
- Need to remember previous performance and track progress

#### Long-term Vision (Future)
**Trainers & Coaching Platform**:
- List of clients
- Training schedule management
- Client workout visibility (if client uses app)
- Program creation and assignment
- Progress monitoring across clients

---

## Tech Principles

### Offline-First Architecture
**Critical Requirement**: The app MUST work offline.

**Why?**
- Gyms often have poor internet connection
- Users may not have data plans
- Workout logging cannot be blocked by connectivity
- Gym environment demands instant responsiveness

### Implementation Strategy
- All workout logging works offline (local database)
- Background sync when connection available
- Queue pending operations (exponential backoff retry)
- Show subtle offline indicator (non-blocking)

### Potential Versioning
- **Basic Version**: Offline-first, only local DB
- **Advanced Version**: Online-first with additional features (social, sharing, etc.)

---

## MVP Scope & Features

### ✅ Implemented Features

#### 1. Onboarding
**Status**: Complete

- Interactive demo (log first set in ~30 seconds)
- Path A: Guided program recommendations
- Path B: Custom workout builder
- Personalization questions (equipment, goal, experience, frequency)
- See: [1-onboarding.md](1-onboarding.md)

#### 2. Workout Logging
**Status**: Complete

**Core Features**:
- Exercise picker with search and category filters
- Custom exercise creation (single field, AI categorization)
- Active workout hub (queued/in-progress/completed exercises)
- Set logging with weight and reps
- Pre-fill from historical data
- Volume tracking (weight × reps)
- Exercise deletion with confirmation
- Auto-save as draft (non-destructive exit)

**Workout Structure**:
- List of exercises
- Each exercise contains:
  - Name (from predefined list + custom)
  - Sets
  - Each set contains:
    - Reps
    - Weight used
    - Notes (optional - future)
- Date and time (captured automatically)

**See**: [2-workout_process.md](2-workout_process.md)

#### 3. Dashboard
**Status**: Complete (Redesigned 2025-11-07)

- User journey-based states (Type 1: Empty, Type 2: Custom, Type 3: Program)
- This Week Hero Card (animated volume progress indicator)
- Visual comparison indicators (↑↗️➡️↘↓)
- Weekly volume goal (10 tonnes)
- Progress-focused motivation
- Resume draft banner (for incomplete workouts)
- See: [3-dashboard.md](3-dashboard.md)

#### 4. User Authentication
**Status**: Complete (Designed 2025-01-12)

- Progressive optional account creation (4 touchpoints)
- "Early Access" framing for community building
- OAuth (Google, Apple) + Email/Password
- Cloud backup and multi-device sync
- Email onboarding strategy (feedback & surveys)
- Offline-first compatible (accounts are optional)
- See: [4-authentication.md](4-authentication.md)

---

### 🚧 Planned Features (Not Yet Implemented)

#### Exercise Definition & Library
**Priority**: Medium

**Current State**: Basic predefined list + custom creation

**Future Enhancements**:
- Exercise categorization (chest, back, legs, arms, shoulders, core)
- Exercise descriptions (how to perform)
- Tools and tips
- Exercise photos/videos
- Equipment tags

#### Progress Tracking & Analytics
**Priority**: Medium

- Exercise-specific charts (weight lifted over time)
- Overall stats (total weight lifted, total workouts completed)
- Goal setting (lift X weight for Y reps)
- Personal records tracking
- Strength progression analysis

#### Profile - Body Measurements
**Priority**: Low

- Log measurements (weight, body fat %, muscle mass)
- Progress charts over time
- Measurement reminders (e.g., monthly)
- Max lift tracking:
  - Select exercise from predefined list
  - Update max lift from measurements screen

#### Social & Sharing
**Priority**: Future

- Share workout summaries
- Follow friends
- Workout streaks and badges

---

## Current Implementation

### Features Map

| Feature | Purpose | Status | Reference |
|---------|---------|-------------------|
| **Onboarding** | Introduce new users to core functionality through interactive demo, then guide to appropriate path | Complete | [1-onboarding.md](1-onboarding.md) |
| **Dashboard** | Adaptive hub with user journey-based states, progress visualization, and motivation through self-competition | Complete (Redesigned) | [3-dashboard.md](3-dashboard.md) |
| **Workout Process** | Core feature: log workouts in real-time (exercise picker, active workout, set logging) | Complete | [2-workout_process.md](2-workout_process.md) |
| **Authentication** | Progressive optional account creation with "Early Access" framing, cloud backup, and email onboarding | Complete (Designed) | [4-authentication.md](4-authentication.md) |
| **Top Bar** | Shared navigation component with logo, back arrow, network badge | Component | [top_bar.md](top_bar.md) |

---

## App Structure

```
┌─────────────────────────────────────────────────────┐
│                   APP LAUNCH                         │
└──────────────────┬──────────────────────────────────┘
                   │
         ┌─────────┴─────────┐
         │                   │
    First-Time          Returning
      User                User
         │                   │
         │                   │
   ┌─────▼─────┐       ┌─────▼─────┐
   │ ONBOARDING│       │ DASHBOARD │
   │  Feature  │       │  Feature  │
   └─────┬─────┘       └─────┬─────┘
         │                   │
         │                   │
         └─────────┬─────────┘
                   │
           ┌───────▼────────┐
           │ WORKOUT PROCESS│
           │    Feature     │
           └───────┬────────┘
                   │
           ┌───────▼────────┐
           │   DASHBOARD    │
           │  (Updated)     │
           └────────────────┘
```

---

## Entry Points

### 1. First-Time User Entry
**Trigger**: App launched, no user data exists, `first_time_completed = false`

**Flow**:
```
App Launch
    ↓
Welcome Screen (1.01-welcome.md)
    ↓ [Let's Log Your First Set]
Demo Exercise Selection (1.02-demo-exercise-selection.md)
    ↓ [Select: Bench Press / Dumbbell Press / Squat]
Exercise Detail Demo (1.03-demo-exercise-detail.md)
    ↓ [Log 1 set - auto-advances]
Success Fork (1.04-success-fork.md)
    ↓ [User chooses path]
    ├─→ Path A: Guided Program
    └─→ Path B: Custom Workout
```

### 2. Returning User Entry
**Trigger**: App launched, user data exists, `first_time_completed = true`

**Flow**:
```
App Launch
    ↓
Dashboard (Adaptive State)
    ├─→ Type 1: No workouts, no plan → Fork screen (choose Path A or B)
    ├─→ Type 2: Has workouts, no plan → Progress-focused dashboard
    └─→ Type 3: Has active program → Program-guided dashboard

    (Draft banner overlays any type if draft exists)
```

---

## Feature Integration Points

### Onboarding → Workout Process

#### Path A: Guided Program
**From**: Workout Plan Preview (1.07)
**Action**: "Start Day 1 Now"
**To**: Active Workout Screen (2-workout_process.md)
**State**: Day 1 exercises pre-loaded in "Queued" state

#### Path A Alternative: Save for Later
**From**: Workout Plan Preview (1.07)
**Action**: "Save to Start Later"
**To**: Dashboard (dashboard.md)
**State**: Shows Draft Banner (1.08)

#### Path B: Custom Workout
**From**: Success Fork (1.04)
**Action**: "I'll Build My Own"
**To**: Exercise Picker (2-workout_process.md)
**State**: Empty workout session

---

### Dashboard → Workout Process

#### Type 1: Fork Selection
**From**: Dashboard Type 1 (empty state)
**Action**: "Get Workout Plan" or "Build My Own"
**To**: Personalization (Path A) or Exercise Picker (Path B)
**State**: User choosing path (like onboarding fork)

#### Type 2: Start New Workout
**From**: Dashboard Type 2 (custom routine)
**Action**: "Start New Workout" button
**To**: Exercise Picker (2-workout_process.md)
**State**: New workout session created

#### Type 2: Get Recommendations
**From**: Dashboard Type 2 (custom routine)
**Action**: "Get Recommendations" (Program Card CTA)
**To**: Personalization Questions
**State**: User opting into program (becomes Type 3)

#### Type 3: Start Program Day
**From**: Dashboard Type 3 (active program)
**Action**: "Start Day X" button (in Program Card)
**To**: Active Workout Screen (2-workout_process.md)
**State**: Day X exercises pre-loaded in "Queued" state

#### Resume Draft (Any Type)
**From**: Dashboard Draft Banner
**Action**: "Resume" button
**To**: Active Workout Screen (2-workout_process.md)
**State**: Exact draft state restored (queued/in-progress/completed)

---

### Workout Process → Dashboard

#### Workout Completed
**From**: Workout Summary (2-workout_process.md)
**Action**: "Done" button
**To**: Dashboard
**State**: Updated with new workout in history

#### Exit During Workout (Auto-save)
**From**: Active Workout Screen
**Action**: Back arrow tap
**To**: Dashboard
**State**: Workout saved as draft, shows resume banner on next launch

---

## Navigation Patterns

### 1. Non-Destructive Navigation
**Pattern**: Back arrow always auto-saves progress

**Examples**:
- Active Workout → Dashboard (saves as draft)
- Exercise Detail → Active Workout (saves logged sets)
- Exercise Picker → Active Workout (preserves added exercises)

**Purpose**: Respects user time, no data loss, gym-friendly UX

---

### 2. Feature Boundaries

#### Dashboard Feature Owns:
- Type 1: Empty state fork screen
- Type 2: Custom routine dashboard
- Type 3: Active program dashboard
- This Week Hero Card (progress visualization)
- Draft workout banner (overlays any type)
- Workout history display

#### Workout Process Feature Owns:
- Exercise Picker
- Active Workout hub
- Exercise Detail (set logging)
- Workout Summary

#### Onboarding Feature Owns:
- Welcome (1.01)
- Demo Exercise Selection (1.02)
- Success Fork (1.04)
- Personalization (1.05)
- Recommendations (1.06)
- Plan Preview (1.07)

**Note**: Exercise Detail (see 2.03-exercise-detail.md) is part of Workout Process but used by Onboarding with `isOnboardingDemo=true` flag

---

### 3. Shared Components

#### Top Bar Component
**Used by**:
- Exercise Picker
- Active Workout
- Exercise Detail
- Personalization
- Plan Preview

**Configurations**:
- Back arrow + Logo + Network badge
- Back arrow + Title (centered) + Network badge
- Logo only + Network badge

**Reference**: [top_bar.md](top_bar.md)

---

## State Transitions

### User Status States
```
New User (first_time_completed = false)
    ↓ [Complete onboarding demo]
Onboarded User (first_time_completed = true)
    ├─→ Type 1: 0 workouts, no plan (empty state)
    │   ↓ [Complete first workout]
    ├─→ Type 2: Has workouts, no plan (custom routine)
    │   ↓ [Get program recommendations]
    └─→ Type 3: Has active program (program follower)
```

### Workout Session States
```
None
    ↓ [Start new workout / Redo / Resume]
Draft (incomplete, auto-saved)
    ↓ [Resume]
In Progress (user actively logging)
    ↓ [Finish Workout]
Completed (saved to history)
```

### Exercise States (within Active Workout)
```
Queued (not started)
    ↓ [Tap card]
In Progress (sets being logged)
    ↓ [Finish Exercise]
Completed (all sets logged)
```

---

## Deep Links & Future Entry Points

**Future Considerations** (not yet implemented):

### External Entry Points
- Push notification → Specific workout
- Share link → Workout plan preview
- Widget → Quick start last workout

### Internal Shortcuts
- Quick action → Start workout
- Siri shortcut → Log set for exercise

---

## Navigation Rules & Guidelines

### Rule 1: Always Preserve User Data
- Auto-save on every navigation
- No confirmation dialogs for navigation (only for deletion)
- Draft state restores exactly as left

### Rule 2: Clear Entry/Exit Points
- Each feature has defined entry points
- Each feature has defined exit points
- No circular dependencies

### Rule 3: Context Preservation
- Pass necessary data between features (e.g., `isOnboardingDemo` flag)
- Use deep linking parameters for state restoration
- Maintain navigation history for back button

### Rule 4: Feature Independence
- Features should work independently when possible
- Cross-feature navigation defined in this file only
- Features communicate through well-defined interfaces

---

## Error Flows & Edge Cases

### Onboarding Abandonment
**Scenario**: User exits during onboarding before choosing path

**From**: Exercise Picker (during onboarding)
**Action**: Back arrow (no exercises added)
**Behavior**: Show exit confirmation dialog
**Options**:
- "Exit" → Close app or Dashboard (if available)
- "Pick Exercises" → Stay in picker

### Workout Abandonment
**Scenario**: User exits active workout mid-session

**From**: Active Workout
**Action**: Back arrow
**Behavior**: Auto-saves as draft (no confirmation)
**Result**: Dashboard shows resume banner

### Empty Workout
**Scenario**: User finishes workout with 0 exercises

**From**: Active Workout
**Action**: "Finish Workout" (with 0 exercises)
**Behavior**: Allow (trust user - might be time-only tracking)
**Result**: Empty workout saved to history

---

## Developer Implementation Notes

### Navigation Architecture
- Use **single activity architecture** (Jetpack Navigation or similar)
- Define navigation graph in code, not in individual features
- Features expose entry point composables only

### State Management
- Use ViewModel for feature-level state
- Use Repository pattern for data persistence
- Share state via StateFlow when needed

### Flag Management
```kotlin
// User status flags
val isFirstTimeUser: Boolean = !userPrefs.firstTimeCompleted
val hasDraftWorkout: Boolean = workoutRepo.getDraftWorkout() != null
val hasWorkoutHistory: Boolean = workoutRepo.getWorkoutCount() > 0

// Feature flags
val isOnboardingDemo: Boolean = true/false // Passed to Exercise Detail
```

### Entry Point Decision Logic
```kotlin
fun getAppEntryPoint(): Screen {
    return when {
        !userPrefs.firstTimeCompleted -> OnboardingWelcomeScreen
        workoutRepo.getDraftWorkout() != null -> DashboardWithDraftBanner
        else -> Dashboard
    }
}
```

---

## Version History

- **2025-11-07**: Updated Dashboard to user journey-based states (Type 1, 2, 3)
- **2025-11-07**: Added This Week Hero Card with progress visualization
- **2025-11-07**: Updated navigation flows for adaptive dashboard
- **2025-01-11**: Created central navigation file
- **2025-01-11**: Defined all feature integration points
- **2025-01-11**: Established navigation rules and patterns
