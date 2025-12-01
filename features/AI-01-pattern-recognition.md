---
tags:
  - feature
  - ai
  - version/mvp
  - version/v1.1
name: AI Pattern Recognition
status: "[[Statuses/Done]]"
---

# AI Feature: Pattern Recognition & Auto-Plan Detection

> **Status**: Approved for MVP (January 1, 2025)
> **Priority**: #3 (after Smart Suggestions and Progress Predictions)
> **Type**: AI-powered automation feature

---

## Overview

### Purpose
Reduce onboarding friction by automatically detecting workout patterns and converting unstructured workout logs into structured workout plans. Saves users 20-30 minutes of manual plan setup while demonstrating AI intelligence.

### User Problem
- **Testing mode**: "I don't want to spend time setting up a plan until I know I'll stick with this app"
- **Plan migration**: "I have my plan in Excel/Strong/paper - re-entering it manually is tedious"
- **Informal routine**: "I follow the same workouts but never bothered to structure them"

### Solution
After 2-4 weeks of consistent workout logging, AI detects the pattern and offers to auto-generate a structured plan. User can preview, edit, and accept - instantly moving from Type 2 (custom workouts) to Type 3 (active plan) Dashboard state.

---

## User Value Propositions

### 1. Zero Upfront Commitment (PRIMARY)
**User Journey**:
- Download app → Log workouts (no plan setup)
- Test core features for 2-4 weeks
- AI detects pattern → Auto-generates plan
- Accept plan → Unlock Type 3 features (pre-loaded exercises, progression tracking)

**Benefit**: Respects "try before you commit" mindset

### 2. Automated Plan Migration
**User Journey**:
- Switching from Strong/JEFIT/Excel
- Copy workouts as they train
- AI structures the plan automatically
- No manual re-entry required

**Benefit**: Saves 20-30 minutes of tedious setup

### 3. Structure Without Effort
**User Journey**:
- User follows informal routine (same workouts, no plan)
- AI recognizes consistency
- Converts to structured plan

**Benefit**: Unlocks plan-based features without manual work

---

## App Areas Touched

This feature requires modifications/additions to multiple parts of the app:

### 1. Onboarding Feature
**Screen**: Exercise Picker (Path B) - `1.09-exercise-picker.md` (See [1.09-exercise-picker.md](../designer/1.09-exercise-picker.md))
**Change**: Add subtle hint about auto-plan detection
**Component**: Tooltip or info banner
**Copy**: *"💡 Tip: Just log your workouts - we'll auto-detect your plan in 2-4 weeks"*
**When shown**: First time user enters Exercise Picker after choosing Path B

---

### 2. Dashboard Feature (Type 2 State)
**Screen**: Dashboard Standard Layout - `3-dashboard.md`
**Change**: Add new conditional banner for pattern detection
**Position**: Below Resume Draft Banner (if exists), above This Week Hero Card
**Component**: NEW - Pattern Detection Banner
**States**:
- Hidden (no pattern detected yet)
- Visible (pattern detected, awaiting user action)
- Dismissed (user clicked "Not now" or "Don't ask again")

**Banner Spec** (to be designed):
```
┌─────────────────────────────────────────────┐
│ 🤖 AI Detected Your Plan!                  │
│                                             │
│ We noticed you're following a consistent   │
│ Push/Pull/Legs routine. Want to save it    │
│ as a structured plan?                       │
│                                             │
│ [View Plan] [Not Now]                      │
└─────────────────────────────────────────────┘
```

---

### 3. Dashboard Feature (Type 2 → Type 3 Transition)
**Screen**: Dashboard Standard Layout - `3-dashboard.md`
**Change**: After user accepts detected plan, transition to Type 3 state
**Components affected**:
- Program Card Slot: Changes from "No active plan" → Active program card
- Primary Action Button: Changes from "Start New Workout" → "Start Day X: [Workout Name]"
- Recent Workouts: Now shows workout source badges (📋 for plan workouts)

---

### 4. NEW: Plan Preview Screen (Auto-Detected)
**Screen**: Uses unified Plan Preview (5.06) - See [5.06-plan-preview.md](../designer/5.06-plan-preview.md) with Auto-Detected context
**Purpose**: Show user the detected plan structure before they accept
**Trigger**: User taps "View Plan" in Dashboard detection banner
**Components**:
- Plan overview (name, detected structure)
- Workout day cards (Day 1: Push - 8 exercises, Day 2: Pull - 7 exercises, etc.)
- Edit controls (add/remove exercises, change order)
- Action buttons: "Save Plan" / "Edit First" / "Dismiss"

**UX Flow**:
1. User taps "View Plan" from Dashboard banner
2. See full plan preview with all detected workout days
3. Can edit before accepting (add/remove exercises)
4. Tap "Save Plan" → Dashboard transitions to Type 3
5. Tap "Dismiss" → Return to Dashboard, banner disappears

---

### 5. Workout Process Feature (Indirect Impact)
**Screen**: Active Workout - `2.02-active-workout.md` (See [2.02-active-workout.md](../designer/2.02-active-workout.md))
**Change**: Behavior changes for Type 3 users after plan acceptance
**Current behavior (Type 2)**: User manually adds exercises via Exercise Picker
**New behavior (Type 3)**: "Start Day X" button pre-loads exercises from detected plan

**No direct changes to Active Workout screen itself** - it just receives pre-loaded exercises like any other Type 3 user.

---

### 6. NEW: Settings / Manual Trigger (Future)
**Screen**: Settings (not yet designed)
**Feature**: Manual plan detection trigger
**Use case**: User dismissed auto-detection but changed mind later
**Component**: "Create Plan from History" button
**Flow**: Settings → Tap button → Analyze workout history → Show Plan Preview

---

### 7. Backend / Data Layer (NEW)
**New Services Required**:

**Pattern Detection Service**:
- Runs after every 3 workouts logged
- Phase 1: Naive pattern matching (backend logic)
- Phase 2: AI deep analysis (if Phase 1 passes 80% threshold)
- Stores detected plan suggestion in database

**Data Models**:
```kotlin
data class DetectedPlan(
    val userId: String,
    val detectedAt: Timestamp,
    val confidence: Float, // 0.0 - 1.0 (e.g., 0.85 = 85% match)
    val planStructure: List<DetectedWorkoutDay>,
    val status: DetectionStatus // PENDING, ACCEPTED, DISMISSED, DISMISSED_PERMANENTLY
)

data class DetectedWorkoutDay(
    val dayNumber: Int,
    val dayName: String, // "Push", "Pull", "Legs" or auto-generated
    val exercises: List<ExerciseTemplate>,
    val sourceWorkoutIds: List<String> // Original workouts used for detection
)

enum class DetectionStatus {
    PENDING,              // Detected, awaiting user action
    ACCEPTED,             // User saved as plan
    DISMISSED,            // User clicked "Not now"
    DISMISSED_PERMANENTLY // User clicked "Don't ask again"
}
```

**Database Tables** (new):
- `detected_plans`: Stores AI-detected plan suggestions
- `detection_history`: Logs all detection attempts for analytics

---

## Integration Points Summary

| App Area | Screen/Component | Change Type | Priority |
|----------|-----------------|-------------|----------|
| **Onboarding** | Exercise Picker (1.09) | Add tooltip/banner | Medium |
| **Dashboard** | Type 2 Standard Layout | Add detection banner | HIGH |
| **Dashboard** | Type 2 → Type 3 transition | State change logic | HIGH |
| **NEW** | Plan Preview Screen | New screen design | HIGH |
| **Workout Process** | Active Workout (2.02) | Indirect (receives pre-loaded exercises) | Low |
| **Settings** | Manual trigger | New feature (future) | Low |
| **Backend** | Pattern Detection Service | New AI service | HIGH |
| **Backend** | Data models | New tables/schemas | HIGH |

---

## User Flow (End-to-End)

### Week 1-3: User Logs Workouts

```
User chooses Path B ("Build My Own")
    ↓
Exercise Picker shows hint: "We'll auto-detect your plan in 2-4 weeks"
    ↓
User logs workouts manually (copying from paper/Excel)
    ↓
Backend: After every 3 workouts, run pattern detection
    ↓
Backend: Pattern not confident yet, continue monitoring
```

### Week 4: Pattern Detected

```
Backend: 80%+ consistency detected (naive check)
    ↓
Backend: Queue for AI deep analysis
    ↓
AI: Confirms pattern is intentional (Push/Pull/Legs split)
    ↓
Backend: Store DetectedPlan with status = PENDING
    ↓
Frontend: Show detection banner on Dashboard
```

### User Accepts Plan

```
User sees Dashboard banner: "AI Detected Your Plan!"
    ↓
User taps "View Plan"
    ↓
Navigate to Plan Preview Screen
    ↓
User previews detected plan (3 workout days: Push/Pull/Legs)
    ↓
User optionally edits (add/remove exercises)
    ↓
User taps "Save Plan"
    ↓
Backend: Create WorkoutPlan from DetectedPlan
    ↓
Backend: Update user state → Type 3 Dashboard
    ↓
Frontend: Dashboard transitions to Type 3 (shows Program Card, "Start Day 1" button)
    ↓
User taps "Start Day 1: Push Workout"
    ↓
Active Workout opens with exercises pre-loaded
```

### User Dismisses Plan

```
User sees Dashboard banner: "AI Detected Your Plan!"
    ↓
User taps "Not Now"
    ↓
Backend: Update DetectedPlan.status = DISMISSED
    ↓
Frontend: Hide banner
    ↓
Optional: Show again after 1 more week if pattern continues
```

---

## Technical Requirements

### Detection Logic (Two-Phase)

**Phase 1: Naive Backend Check**
```kotlin
fun detectPatternNaive(workouts: List<Workout>): Boolean {
    // Minimum data requirement
    if (workouts.size < 6) return false

    // Group workouts by similar exercise lists
    val workoutGroups = workouts.groupBy { workout ->
        workout.exercises.map { it.exerciseId }.sorted()
    }

    // Check if 3+ distinct workout types exist
    if (workoutGroups.size < 3) return false

    // Check if each workout type repeats (cycle detected)
    val allGroupsRepeat = workoutGroups.all { (_, group) ->
        group.size >= 2 // Each workout type logged at least twice
    }

    // Calculate consistency percentage
    val totalWorkouts = workouts.size
    val matchingWorkouts = workoutGroups.values.sumOf { it.size }
    val consistency = matchingWorkouts.toFloat() / totalWorkouts

    // Require 80%+ consistency
    return allGroupsRepeat && consistency >= 0.80
}
```

**Phase 2: AI Deep Analysis**
```kotlin
suspend fun analyzePatternWithAI(workouts: List<Workout>): DetectedPlan? {
    val prompt = """
    Analyze these workout logs and detect if user follows a structured plan:

    ${workouts.toJsonForAI()}

    Determine:
    1. Is this a structured plan or random workouts?
    2. What type of split? (PPL, Upper/Lower, Full Body, etc.)
    3. How many distinct workout days?
    4. What exercises belong to each day?
    5. Suggested names for each workout day

    Respond in JSON format.
    """

    val aiResponse = openAI.complete(prompt)
    return parseDetectedPlan(aiResponse)
}
```

### Consistency Thresholds

**Fast Detection (2 weeks / 6-8 workouts)**:
- Requires: 90%+ consistency (exact exercise match)
- Use case: User copying from existing plan

**Standard Detection (3 weeks / 9-12 workouts)**:
- Requires: 70-80% consistency
- Use case: Flexible patterns (some exercise substitutions)

**Example Calculation**:
```
Week 1: Push (8 exercises) / Pull (7 exercises) / Legs (6 exercises)
Week 2: Push (same 8) / Pull (same 7) / Legs (same 6)
Week 3: Push (7/8 match) / Pull (6/7 match) / Legs (same 6)

Total: 9 workouts
Matches: Push 2/3, Pull 2/3, Legs 3/3 = 7/9 workouts consistent
Consistency: 78% → Trigger detection
```

---

## UX Design Specifications

### Dashboard Detection Banner

**Component**: `PatternDetectionBanner.kt`
**Position**: Between Resume Draft Banner and This Week Hero Card
**Visibility**: `if (detectedPlan?.status == PENDING)`

**Design Specs** (to be detailed in screen spec):

**Container**:
- Background: Gradient (light blue to light green)
- Border radius: 12dp
- Padding: 16dp
- Margin bottom: 16dp
- Shadow: Elevation 2dp

**Icon**:
- 🤖 AI brain icon (32dp)
- Position: Left side

**Text**:
- Headline: "AI Detected Your Plan!" (16sp / Bold)
- Body: "We noticed you're following a consistent [Split Type] routine. Want to save it as a structured plan?" (14sp / Regular)

**Actions**:
- Primary button: "View Plan" (solid green)
- Secondary button: "Not Now" (text only, gray)

---

### Plan Preview Screen (Auto-Detected)

**Component**: `AutoDetectedPlanPreviewScreen.kt`
**Uses**: Unified Plan Preview (5.06) with Auto-Detected context

**Top Bar**:
- Back arrow (returns to Dashboard)
- Title: "Your Detected Plan"
- No network badge needed

**Content**:

**Header Section**:
- Plan icon + AI badge
- Title: "[Split Type] (Detected)" (e.g., "Push/Pull/Legs (Detected)")
- Confidence indicator: "85% match" (optional)
- Description: "Based on [X] weeks of your workouts"

**Workout Day Cards** (for each detected day):
- Day number + Name: "Day 1: Push"
- Exercise count: "8 exercises"
- Source indicator: "Based on workouts from [dates]"
- Exercise list (expandable)
- Edit button (pencil icon)

**Bottom Actions**:
- Primary: "Save Plan" (green, full width)
- Secondary: "Edit First" (outline)
- Tertiary: "Dismiss" (text only)

---

## Open Questions & Decisions Needed

### UX Questions

- [ ] **Detection progress visibility**: Should we show users "2/3 weeks analyzed, pattern emerging..."?
  - Pro: Sets expectations, builds anticipation
  - Con: Might feel creepy ("app is watching me")

- [ ] **Multiple patterns**: What if user switches patterns mid-detection?
  - Example: PPL for 2 weeks, then Upper/Lower for 2 weeks
  - Do we detect most recent pattern only?
  - Do we ask user which pattern they prefer?

- [ ] **Rest days**: Should we detect rest days as part of pattern?
  - Example: "You rest every Wednesday"
  - Include in plan structure or ignore?

- [ ] **Banner persistence**: How long do we show detection banner?
  - Until user accepts/dismisses?
  - Re-show after 1 week if dismissed?
  - Never show again if dismissed permanently?

### Technical Questions

- [ ] **Detection frequency**: How often do we run pattern check?
  - After every workout? (expensive)
  - After every 3 workouts? (recommended)
  - Weekly batch job?

- [ ] **AI cost optimization**: When to use AI vs naive logic?
  - Only use AI if naive check passes 80%?
  - Always use AI for better accuracy?

- [ ] **Data retention**: How long do we keep detected plans?
  - Delete after 30 days if not accepted?
  - Keep forever for analytics?

---

## Success Metrics

### Feature Adoption
- % of Type 2 users who receive pattern detection suggestion
- % of users who accept vs dismiss detected plan
- Average time from first workout to plan detection
- % of users who edit plan before accepting

### User Impact
- Reduction in manual plan setup time (target: 20-30 min saved)
- Type 2 → Type 3 conversion rate improvement
- User retention after plan acceptance (vs manual plan users)

### Technical Performance
- Pattern detection accuracy (false positives vs true positives)
- AI cost per detection (target: <$0.01 per user)
- Detection latency (target: <5 seconds for AI analysis)

---

## Timeline & Dependencies

### MVP Requirements (January 1, 2025)
- ✅ Dashboard detection banner (design + implementation)
- ✅ Plan Preview screen (design + implementation)
- ✅ Backend pattern detection service (naive + AI)
- ✅ Data models and database schema
- ✅ Type 2 → Type 3 state transition logic

### Phase 1.5 (February 2025)
- Settings manual trigger ("Create Plan from History")
- Detection progress indicator
- Multiple pattern handling
- Rest day detection

### Dependencies
- **Requires**: Workout logging feature (already exists)
- **Requires**: Dashboard Type 2 and Type 3 states (already exists)
- **Requires**: Workout Plan data model (exists from Path A)
- **Blocks**: None (standalone feature)

---

## Design Status

| Component | Status | Assignee | Notes |
|-----------|--------|----------|-------|
| Detection Banner (Dashboard) | 🔴 Not Started | UX Designer | Need full spec |
| Plan Preview Screen | ✅ Uses 5.06 | UX Designer | Unified Plan Preview with contexts |
| Onboarding Hint | 🔴 Not Started | UX Designer | Tooltip copy |
| Backend API | 🔴 Not Started | Developer | Pattern detection service |
| AI Integration | 🔴 Not Started | Developer | OpenAI API calls |

---

**Last Updated**: 2025-11-16
**Next Review**: TBD (after Dashboard banner design)
