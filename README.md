# Shapyfy Design Documentation

Welcome to the Shapyfy UX/UI design documentation. This directory contains complete specifications for all app features.

---

## 📋 Start Here

**New to the project?** Read these files in order:

1. **[00-app-overview.md](features/00-app-overview.md)** ← **START HERE**
    - **Project vision & target audience** (short-term & long-term)
    - **Tech principles** (offline-first architecture)
    - **MVP scope** (implemented & planned features)
    - **Complete app navigation map** (flows, integration points, patterns)
    - **Everything you need to understand the product and its design**

2. **Feature specifications** (detailed UI/UX for each feature)
    - Read after understanding overall structure from `00-app-overview.md`

---

## 🗂️ Documentation Structure

### Central Navigation
- **[00-app-overview.md](features/00-app-overview.md)** - Single source of truth for app structure and navigation

### Feature Specifications

#### Onboarding Feature
- **[1-onboarding.md](features/1-onboarding.md)** - Feature overview
- **[1.01-welcome.md](designer/1.01-welcome.md)** - Welcome screen
- **[1.02-demo-exercise-selection.md](designer/1.02-demo-exercise-selection.md)** - Demo exercise selection
- **[1.03-demo-exercise-detail.md](designer/1.03-demo-exercise-detail.md)** - Demo set logging
- **[1.04-success-fork.md](designer/1.04-success-fork.md)** - Success celebration & path choice
- **[1.05-personalization.md](designer/1.05-personalization.md)** - Personalization questions (Path A)
- **[1.06-recommendations.md](designer/1.06-recommendations.md)** - Workout plan recommendations (Path A)
- **[1.07-workout-plan-preview.md](designer/1.07-workout-plan-preview.md)** - Plan preview (Path A)
- **[1.08-dashboard-draft-banner.md](designer/1.08-dashboard-draft-banner.md)** - Draft banner (Path A)
- **[1.09-exercise-picker.md](designer/1.09-exercise-picker.md)** - Exercise picker pointer (Path B)

#### Dashboard Feature
- **[3-dashboard.md](features/3-dashboard.md)** - Feature overview and key design decisions
- **[3.04-hero-card.md](features/3.04-hero-card.md)** - This Week Hero Card (animated volume indicator)
    - User journey-based states (Type 1, 2, 3)
    - Progress-focused motivation (visual indicators)
    - Weekly volume goal tracking
    - Self-competition metrics

#### Workout Process Feature
- **[2-workout_process.md](features/2-workout_process.md)** - Feature overview and key design decisions
- **[2.01-exercise-picker.md](designer/2.01-exercise-picker.md)** - Exercise search, filters, custom exercise creation
- **[2.02-active-workout.md](designer/2.02-active-workout.md)** - Workout hub with exercise cards and management
- **[2.03-exercise-detail.md](designer/2.03-exercise-detail.md)** - Set logging, history tracking, skip/finish functionality
- **[2.04-workout-summary.md](designer/2.04-workout-summary.md)** - Workout completion celebration screen

**Organization**: Screen-centric structure following onboarding convention. Dialogs live with their trigger screens.

#### Shared Components
- **[top_bar.md](top_bar.md)** - Reusable top bar component specification

---

## 📐 How to Use This Documentation

### For UX Designers
1. Read `00-app-overview.md` to understand navigation
2. Reference feature files for detailed screen specifications
3. All colors, spacings, and components are defined in specs
4. Use tags like `<screen_name_state>` to reference specific screen states

### For Developers
1. Start with `00-app-overview.md` for navigation architecture
2. Feature files contain implementation notes and business logic
3. All colors sourced from: `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`
4. Offline-first architecture required (see feature Developer Notes sections)

### For Product/Stakeholders
1. `00-app-overview.md` shows:
    - Project vision and roadmap
    - MVP scope (what's built vs planned)
    - Complete user journeys and flows
2. Feature overviews explain purpose and user problems
3. Screen specifications show exact UI details

---

## 🎯 Design Principles

### 1. Single Source of Truth
- Navigation logic lives in `00-app-overview.md`
- Feature files focus on UI/UX details
- No duplication of navigation flows

### 2. Feature Independence
- Each feature has clear responsibilities
- Integration points explicitly defined
- Features communicate through well-defined interfaces

### 3. User-First Design
- Non-destructive navigation (auto-save)
- Offline-first (works without internet)
- Gym-friendly UX (large tap targets, quick actions)

### 4. Progressive Disclosure
- Show value before asking for commitment (demo first)
- Context-aware guidance (help when needed)
- No unnecessary friction

---

## 🔄 Navigation Quick Reference

### Entry Points
- **First-time user** → Onboarding Welcome
- **Returning user (no draft)** → Dashboard
- **Returning user (has draft)** → Dashboard with Resume Banner

### Main User Flows
- **Start workout** → Exercise Picker → Active Workout → Exercise Detail → Workout Summary → Dashboard
- **Resume workout** → Active Workout (restored state)
- **Redo last workout** → Active Workout (pre-filled exercises)

### Exit Points
- **From Active Workout** → Dashboard (auto-saves as draft)
- **From Workout Summary** → Dashboard (workout complete)
- **From Onboarding** → Exercise Picker or Workout Plan Preview

---

## 📝 Documentation Standards

### Screen State Tags
Use HTML-style tags to reference specific screen states:

```markdown
<screen_name_state>
[Detailed specification]
</screen_name_state>
```

**Examples**:
- `<dashboard_first_time_user>`
- `<active_workout_redesigned>`
- `<exercise_detail_first_set>`

### Navigation References
Always reference the central navigation file:

```markdown
> **Navigation**: See [00-app-overview.md](features/00-app-overview.md) for complete navigation flows
```

### Cross-Feature References
Link to other features when needed:

```markdown
**Uses**: Exercise Detail screen from [2.03-exercise-detail.md](designer/2.03-exercise-detail.md) with `isOnboardingDemo=true` flag
```

---

## 🛠️ Maintenance Guidelines

### When Adding New Features
1. Add feature to `00-app-overview.md` first
2. Define integration points with existing features
3. Create feature specification file
4. Update this README

### When Changing Navigation
1. Update `00-app-overview.md` ONLY
2. Feature files should not contain cross-feature navigation

### When Adding New Screens
1. Add to appropriate feature file
2. Use `<screen_name_state>` tags
3. Update feature's screen list

---

## 📊 Feature Status

| Feature | Status | Last Updated |
|---------|--------|--------------|
| Onboarding | ✅ Complete | 2025-01-11 |
| Dashboard | ✅ Complete | 2025-11-07 (Redesigned) |
| Workout Process | ✅ Complete | 2025-01-05 (Restructured) |
| Authentication | ✅ Complete (Designed) | 2025-01-12 |
| Top Bar | ✅ Complete | 2024-12-XX |

---

## 🎨 Design Resources

### Color System
All colors defined in: `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`

**Key Colors**:
- Primary: `#A8E6CF` (LimeGreenDark)
- Success: `#CBFFC9` (LimeGreenBright)
- Error: `#FF9999` (ErrorRed)
- Background: `#F3F3F8` (LightBackground)
- Surface: `#FFFFFF` (LightSurface)

### Typography
- Headings: Bold
- Body: Regular/Medium
- Sizes: 12sp - 48sp (defined in each spec)

### Spacing
- Padding: 8dp, 12dp, 16dp, 24dp
- Margins: 8dp, 12dp, 16dp, 24dp
- Touch targets: Minimum 48dp × 48dp

---

## 🤝 Contributing

### For Designers
1. Understand existing structure before proposing changes
2. Follow established patterns and conventions
3. Update `00-app-overview.md` for navigation changes
4. Use consistent terminology across specs

### For Reviewers
1. Check `00-app-overview.md` for navigation conflicts
2. Verify feature boundaries are respected
3. Ensure no duplication across files
4. Validate cross-references are correct

---

## 📞 Questions?
- **Navigation unclear?** → Check `00-app-overview.md`
- **Screen details needed?** → Check feature specification files
- **Integration points?** → Check `00-app-overview.md` integration points section
- **Design rationale?** → Each feature has "User Problem" and "Purpose" sections

---

**Last Updated**: 2025-01-11
**Documentation Version**: 2.0 (Centralized Navigation Structure)