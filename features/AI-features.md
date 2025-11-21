---
tags:
  - feature
  - ai
name: AI Features Overview
status: "[[Statuses/Done]]"
---

# AI Features Overview

This document tracks AI feature ideas and concepts for Shapyfy.

---

## 1. AI Plan / Exercise Recommendation

**Quick Note**: AI suggests exercises based on workout history analysis. Helps users decide what to train today by analyzing:
- Muscle groups not trained recently
- Push/pull/legs balance
- Volume distribution
- User's available equipment

**Target User**: Beginners who don't have a structured plan

---

## 2. AI Exercise Conversion

**Quick Note**: When equipment is unavailable, AI suggests alternative exercises with weight conversion. Example: User planned Hammer Chest Press @ 80kg but machine is taken → AI suggests Barbell Bench Press @ 70kg (converted weight based on exercise equivalency).

**Target User**: Intermediate users dealing with equipment availability

---

## 3. Progression Planner (RiR-based)

**Quick Note**: AI suggests progressive overload based on RiR (Reps in Reserve) tracking. Analyzes how close user was to failure on each set and recommends weight/rep adjustments for next workout.

**Target User**: Intermediate/Advanced users who understand training intensity

**Consideration**: Adds friction to set logging (extra RiR input per set)

---

## 4. Plan Suggester / Pattern Recognition

**Primary Value Proposition**: Reduces onboarding friction by letting users test the app without committing to plan setup. After 2-4 weeks of logging workouts, AI detects their pattern and auto-generates a structured plan - saving 20-30 minutes of manual setup.

### Use Cases

**Use Case 1: Testing New App** (PRIMARY)
- User downloads Shapyfy but doesn't want to invest time setting up plan until they know they'll stick with the app
- User logs workouts from their existing plan (paper/Excel/other app)
- After 2-4 weeks: AI detects pattern and offers to formalize it
- Benefit: Zero upfront commitment, respects "try before you commit" mindset

**Use Case 2: Plan Migration**
- User switching from Strong/JEFIT/Excel
- Manually re-entering plan is tedious (20-30 min)
- User just logs workouts as they train
- AI detects and structures the plan automatically
- Benefit: Saves manual setup time

**Use Case 3: Informal Routine**
- User follows consistent routine but never formalized it
- AI recognizes pattern they didn't realize they had
- Benefit: Structure without effort

### Target Users
- **Primary**: Intermediate users testing new app with existing plans
- **Secondary**: Type 2 Dashboard users (custom workouts, no active plan)
- **Not for**: Beginners (don't have consistent patterns yet), Advanced users (already use structured plans)

### Detection Logic (Two-Phase Approach)

**Phase 1: Naive Backend Check**
- Simple pattern matching (rule-based, no AI)
- Checks: Exercise names, workout frequency, consistency
- Threshold: 80% match required to proceed to Phase 2
- Example: 4 workouts logged, 3+ must match pattern
- Purpose: Filter false positives, reduce AI costs

**Phase 2: AI Deep Analysis**
- Only triggered if Phase 1 passes
- AI analyzes: Exercise selection, muscle group balance, workout structure
- Confirms pattern is intentional (not random coincidence)
- Generates structured plan with named workout days

### Consistency Thresholds

**Fast Detection (2 weeks / 6-8 workouts)**:
- Requires: 90%+ consistency (exact exercise match)
- Use case: User copying from existing plan

**Standard Detection (3 weeks / 9-12 workouts)**:
- Requires: 70-80% consistency
- Use case: Flexible patterns (exercise substitutions allowed)

**Example**:
```
Week 1: Push (8 exercises) / Pull (7 exercises) / Legs (6 exercises)
Week 2: Push (same 8) / Pull (same 7) / Legs (same 6)
Week 3: Push (7/8 match) / Pull (6/7 match) / Legs (5/6 match)

Consistency: 85% → Suggest conversion
```

### UX Decisions (Approved 2025-11-16)

1. ✅ **Onboarding Communication**: Tell users about this feature
   - Subtle hint during Path B onboarding
   - "💡 Tip: Just log your workouts - we'll auto-detect your plan in 2-4 weeks"

2. ✅ **User Control**: Opt-in only
   - User can accept or dismiss suggestion
   - "Don't ask again" option available
   - Can manually trigger later (Settings → "Create Plan from History")

3. ✅ **Plan Editing**: Fully editable after conversion
   - AI-generated plan is a starting point
   - User can add/remove exercises, change order, adjust sets/reps
   - Non-destructive (original workout logs preserved)

4. ✅ **Detection Speed**: Adaptive based on consistency
   - Very consistent (90%+): 2 weeks
   - Moderately consistent (70-80%): 3-4 weeks

### Implementation Notes

**Backend Logic**:
```
1. User logs workout
2. Every 3 workouts: Run naive pattern check
3. If 80%+ match: Queue for AI analysis
4. AI confirms pattern: Store suggested plan
5. Show banner on Dashboard: "We detected your plan!"
6. User accepts: Convert to Type 3 Dashboard state
7. User dismisses: Mark as "pattern_detection_dismissed"
```

**Data Tracking**:
- Track workout history with exercise lists
- Identify repeated exercise combinations
- Detect workout day labels (if user names them)
- Calculate consistency percentage

**UX Flow**:
1. Dashboard banner appears after detection
2. User taps "View Detected Plan"
3. Shows preview: "Day 1: Push (8 exercises), Day 2: Pull (7 exercises)..."
4. User can edit before accepting
5. User taps "Save Plan" → Moves to Type 3 Dashboard

### Competitive Advantage

**No competitor has this feature:**
- Strong: Requires manual plan setup (15-30 min)
- JEFIT: Requires manual plan setup (20-40 min)
- Hevy: Has "Redo Last Workout" but no pattern detection
- Fitbod: AI-generated plans but no user control/testing period

**Shapyfy**: Only app that auto-detects user's existing plan from workout logs.

### Priority & Timeline

**Recommended**: MVP (January 1, 2025)

**Rationale**:
- Reduces core onboarding friction (not a nice-to-have)
- Unique market differentiator
- Clear "aha moment" for AI demonstration
- Supports business goal (convert Type 2 → Type 3 Dashboard)

**Phased Rollout**:
- January 1: Feature ships (built into MVP)
- January 15: Beta users (started Dec 1) have 6 weeks data → First wave of detections
- Late January: January launch users have 3-4 weeks data → Second wave

**Marketing opportunity**: "New AI feature just detected your plan!" creates ongoing excitement.

### Open Questions

- [ ] Should we show detection progress? ("2/3 weeks analyzed, pattern emerging...")
- [ ] What if user has multiple patterns? (e.g., PPL for 2 weeks, then Upper/Lower for 2 weeks)
- [ ] Should we detect rest days as part of pattern? (e.g., "You rest every Wednesday")

---

**Last Updated**: 2025-11-16
