---
tags:
  - feature
  - dashboard
name: Dashboard
status: "[[Statuses/Done]]"
---

# Dashboard Feature (Redesigned)

> **Parent Feature**: Core App Hub
> **Navigation**: See [00-app-overview.md](00-app-overview.md) for complete navigation flows
> **Design Specs**: [Dashboard Design](../designer/3.01-dashboard.md)

---

## Overview

### Purpose
The Dashboard is the central hub of Shapyfy. It adapts to user state (workout history, active plan) while maintaining a consistent layout. It motivates users through progress visualization and provides clear entry points to start workouts.

### User Problem
Users need:
- Clear motivation to continue training (progress visibility)
- Quick access to start workouts
- Context about their recent activity
- Optional structure (workout programs) without feeling forced

### Key Design Principles
1. **ONE dashboard layout, conditionally rendered** - Not 3 different screens, but ONE component that adapts based on:
   - Does user have workout history? (Yes/No)
   - Does user have active plan? (Yes/No)
   - Does user have draft workout? (Yes/No)

2. **Eliminate redundancy** (v2.1) - Every component serves a unique purpose:
   - No duplicate buttons
   - No redundant cards
   - Single source of truth for each type of information

3. **Visual clarity** (v2.1) - User always knows:
   - Which workouts are from their plan (📋)
   - Which workouts are custom (✏️)
   - What action to take next (single prominent CTA)
   - **Contextual Plan Access**: "Plan Hub" card slot adapts based on library state (Empty = Get Started, Populated = Manage Library).

---

## Architecture: One Dashboard, Conditional Elements

### IMPORTANT: This is NOT 3 Different Screens

Developers should build **ONE** `DashboardScreen` composable with conditional rendering:

```kotlin
@Composable
fun DashboardScreen(
    hasWorkoutHistory: Boolean,
    activePlan: WorkoutPlan?,
    draftWorkout: Workout?,
    thisWeekStats: WeeklyStats?
) {
    when {
        // Edge case: Empty state (no history, no plan)
        !hasWorkoutHistory && activePlan == null -> DashboardEmptyState()

        // Normal case: Standard dashboard
        else -> DashboardStandardLayout(
            activePlan = activePlan,
            draftWorkout = draftWorkout,
            thisWeekStats = thisWeekStats
        )
    }
}
```

**Only 2 layouts**:
1. **Empty State** (fork screen) - Edge case for new users with 0 workouts
2. **Standard Dashboard** - Everyone else (conditional elements shown/hidden)

---

## Business Logic & Data Models

### Weekly Stats Data Model
```kotlin
data class WeeklyStats(
    val currentVolume: Double,      // in kg (e.g., 7200.0 kg = 7.2 tonnes)
    val goalVolume: Double = 10000.0, // Fixed 10 tonnes for MVP
    val lastWeekVolume: Double?,    // null if first week
    val workoutsThisWeek: Int,
    val workoutsLastWeek: Int?,
    val setsThisWeek: Int,
    val setsLastWeek: Int?
)

// Helper functions
fun WeeklyStats.volumeInTonnes(): Double = currentVolume / 1000.0
fun WeeklyStats.progressPercentage(): Float = (currentVolume / goalVolume).toFloat()
fun WeeklyStats.isOverGoal(): Boolean = currentVolume > goalVolume
fun WeeklyStats.isFirstWeek(): Boolean = lastWeekVolume == null
```

### Calculation Logic

**Weekly Volume**:
```kotlin
fun calculateWeeklyVolume(workouts: List<Workout>): Double {
    val startOfWeek = LocalDate.now().with(DayOfWeek.MONDAY).atStartOfDay()
    val endOfWeek = startOfWeek.plusDays(7)

    return workouts
        .filter { it.completedAt in startOfWeek..endOfWeek }
        .flatMap { it.exercises }
        .filter { it.status != ExerciseStatus.SKIPPED }
        .flatMap { it.sets }
        .sumOf { set -> set.weight * set.reps }
}
```

**Comparison Calculation**:
```kotlin
fun getComparisonIndicator(current: Int, previous: Int): ComparisonIndicator {
    val change = ((current - previous).toDouble() / previous) * 100
    return when {
        change > 20 -> ComparisonIndicator.SIGNIFICANT_UP
        change in 5.0..20.0 -> ComparisonIndicator.MODERATE_UP
        change in -5.0..5.0 -> ComparisonIndicator.STABLE
        change in -20.0..-5.0 -> ComparisonIndicator.MODERATE_DOWN
        else -> ComparisonIndicator.SIGNIFICANT_DOWN
    }
}

enum class ComparisonIndicator(val icon: String, val color: Color) {
    SIGNIFICANT_UP("↑", Color(0xFF4CAF50)),
    MODERATE_UP("↗", Color(0xFF4CAF50)),
    STABLE("➡️", Color(0xFF666666)),
    MODERATE_DOWN("↘", Color(0xFFFF9800)),
    SIGNIFICANT_DOWN("↓", Color(0xFFF44336))
}
```
