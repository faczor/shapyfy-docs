---
tags:
  - client
  - feature
  - version/mvp
name: Onboarding
status: "[[Statuses/Done]]"
---
# Onboarding Feature

> **Navigation**: See [00-app-overview.md](00-app-overview.md) for complete navigation flows and integration points

## Overview

### Purpose
The Onboarding feature introduces new users to Shapyfy's core functionality (set logging) through a quick interactive demo (~30 seconds), then guides them toward either guided workout programs or custom exercise selection based on their self-assessment and goals.

### User Problem
New users face decision fatigue and confusion when opening fitness apps for the first time:
- Don't understand how the app works before being asked to make decisions
- Don't know which exercises to pick or what workout to do
- Unsure if they need help or can build their own program
- Want to experience value before investing time in setup
- Need guidance based on experience level, goals, and equipment

### Responsibilities
- Deliver core value (set logging demo) before asking for commitment
- Teach app interface through hands-on experience
- Segment users by self-selection (guided vs custom)
- Collect personalization data (equipment, goal, experience, frequency)
- Recommend appropriate workout plans based on user profile
- Provide clear paths for both beginners and advanced users
- Save programs for later (home explorer use case)

---

## Screens Overview

**Universal Screens (Both Paths):**
1. **Welcome Screen** → See [1.01-welcome.md](../designer/1.01-welcome.md)
2. **Exercise Selection Screen** → See [1.02-demo-exercise-selection.md](../designer/1.02-demo-exercise-selection.md)
3. **Exercise Detail Demo** → See [1.03-demo-exercise-detail.md](../designer/1.03-demo-exercise-detail.md)
4. **Success + Fork Screen** → See [1.04-success-fork.md](../designer/1.04-success-fork.md)

**Path A Screens (Guided):**
5. **Personalization Screen** → See [1.05-personalization.md](../designer/1.05-personalization.md)
6. **Recommendations Loading/Success/Error** → See [1.06-recommendations.md](../designer/1.06-recommendations.md)
7. **Workout Plan Preview** → See [5.06-plan-preview.md](../designer/5.06-plan-preview.md) (moved to Workout Plan feature)
8. **Dashboard Draft Banner** → See [1.08-dashboard-draft-banner.md](../designer/1.08-dashboard-draft-banner.md)

**Path B Screens (Custom):**
5. **Exercise Picker** → See [1.09-exercise-picker.md](../designer/1.09-exercise-picker.md) (pointer to workout_process.md)
6. **Active Workout** → Uses [workout_process.md](2-workout_process.md#active_workout_redesigned)

---

## Data Collection & State Management

### Demo Session
- **Created**: When user selects exercise from Exercise Selection Screen
- **Temporary**: Not persisted to workout history
- **Discarded**: After Success + Fork screen
- **Flag**: `isOnboardingDemo=true` passed to Exercise Detail screen (from workout_process.md)

### Path A: Personalization Data
**Personalization Questions** (collected sequentially):
1. **Equipment**: Home / Gym
2. **Goal**: Build Muscle / Lose Fat / Get Stronger / Stay Healthy
3. **Experience**: Beginner / Intermediate / Advanced
4. **Frequency**: 2 days/week / 3 days/week / 4 days/week / 5+ days/week

**Recommendation Request**:
- Input: User profile (equipment, goal, experience, frequency)
- Output: List of 3 workout plans (1 recommended, 2 alternatives)
- States: Loading → Success OR Error

**Program Save States**:
- **Start Now**: Program active immediately (Day 1 exercises loaded in Active Workout)
- **Save for Later**: Program saved as draft, shown in Dashboard Draft Banner

### Path B: Custom Workout
- **Exercise Selection**: User manually selects exercises from Exercise Picker (see workout_process.md)
- **Workout Creation**: Custom workout created with selected exercises
- **No Program**: No multi-day program saved (single workout session)

---

## Developer Notes

### Demo Session Lifecycle
1. Create temporary workout session when exercise selected
2. Navigate to Exercise Detail with `isOnboardingDemo=true`
3. After 1 set logged, auto-return to Success + Fork
4. Discard session data (do not persist to history)

### Recommendation API Integration
- Endpoint: `/api/recommendations/workout-plans`
- Request body: `{ equipment, goal, experience, frequency }`
- Response: `List<WorkoutPlan>` (3 plans)
- Error handling: Network timeout, server error → Show error screen with retry

### Program Persistence (Path A)
- **Start Now**: Load Day 1 exercises → Navigate to Active Workout
- **Save for Later**: Persist program with `status: DRAFT` → Navigate to Dashboard

---

## Screen References

| Step | Screen                 | File Reference                                                     |
| ---- | ---------------------- | ------------------------------------------------------------------ |
| 1    | Welcome                | [1.01-welcome.md](../designer/1.01-welcome.md)                                 |
| 2    | Exercise Selection     | [1.02-demo-exercise-selection.md](../designer/1.02-demo-exercise-selection.md) |
| 3    | Exercise Detail (Demo) | [1.03-demo-exercise-detail.md](../designer/1.03-demo-exercise-detail.md)       |
| 4    | Success + Fork         | [1.04-success-fork.md](../designer/1.04-success-fork.md)                       |
| 5A   | Personalization        | [1.05-personalization.md](../designer/1.05-personalization.md)                 |
| 6A   | Recommendations        | [1.06-recommendations.md](../designer/1.06-recommendations.md)                 |
| 7A   | Plan Preview           | [5.06-plan-preview.md](../designer/5.06-plan-preview.md)       |
| 8A   | Dashboard Draft        | [1.08-dashboard-draft-banner.md](../designer/1.08-dashboard-draft-banner.md)   |
| 5B   | Exercise Picker        | [1.09-exercise-picker.md](../designer/1.09-exercise-picker.md)                 |
| 6B   | Active Workout         | [workout_process.md](2-workout_process.md#active_workout_redesigned) |

> **Complete navigation flows**: See [00-app-overview.md](00-app-overview.md)