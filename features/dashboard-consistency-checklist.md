---
tags:
  - feature
  - dashboard
  - version/mvp
  - version/v1.1
name: Dashboard Consistency Checklist
status: "[[Statuses/Done]]"
---

# Dashboard Consistency Checklist (v2.1)

**Last Updated**: 2025-11-17
**Status**: ✅ Hero Card Complete | ⚠️ Other Components Need Updates
**Purpose**: Ensure all dashboard components follow v2.1 design standards

---

## Overview

The Hero Card has been updated to v2.1 standards (Material Symbols Rounded, refined spacing, simplified animations). Other dashboard components need updates for consistency.

---

## ✅ Already Consistent (Hero Card v2.1)

### Material Symbols Rounded Icons
- ✅ Flame icon (`local_fire_department`) - 20dp, weight 500
- ✅ Dumbbell icon (`fitness_center`) - 16dp, weight 500
- ✅ Target icon (`target`) - 16dp, weight 500
- ✅ Sparkle icon (`auto_awesome`) - 16dp, weight 500
- ✅ Rocket icon (`rocket_launch`) - 16dp, weight 500
- ✅ Celebration icon (`celebration`) - 16dp, weight 500
- ✅ Trophy icon (`emoji_events`) - 16dp, weight 500

### Spacing (8dp system)
- ✅ Card padding: 16dp
- ✅ Margins: 16dp, 24dp
- ✅ Icon spacing: 8dp margin right (leading), 4dp margin left (inline)

### Typography
- ✅ Header: 18sp Bold
- ✅ Goal label: 16sp Medium
- ✅ Volume: 24sp Bold
- ✅ Messaging: 15sp Medium/Regular

### Colors
- ✅ All colors from Color.kt or specified (#B0EDD5, #FFB800)
- ✅ WCAG AA compliant (#666666 labels, 4.5:1 contrast)

### Border Radius Hierarchy
- ✅ Card: 16dp
- ✅ Container: 8dp
- ✅ Removed: Grid cells (no longer exist)

---

## ⚠️ Needs Updates (Other Dashboard Components)

### 1. Program Card Icons

**Current State** (line 907):
```
**Icon** (optional):
- Icon: 📋 or program icon
- Size: 20dp
- Margin right: 8dp
```

**Recommended Update**:
```
**Icon**:
- Material Symbol: "description" (clipboard/document icon)
- Style: Rounded, weight 500
- Size: 20dp
- Color: #A8E6CF (LimeGreenDark)
- Margin right: 8dp
```

**Rationale**: Replace emoji with Material Symbols Rounded for consistency with Hero Card.

---

### 2. Program Completed State Checkmark

**Current State** (line 1023):
```
- Current Day Info: "✅ Completed! Week [X]"
```

**Recommended Update**:
```
- Current Day Info: "[Check Circle Icon] Completed! Week [X]"
- Material Symbol: "check_circle"
- Style: Rounded, weight 500
- Size: 20dp, inline with text
- Color: #4CAF50 (green success)
```

**Rationale**: Replace emoji with Material Symbol for consistency.

---

### 3. Recent Workouts Source Badges

**Current State** (lines 1136-1140, examples at 1227-1245):
```
**Workout Source Badge** (Right):
- **Position**: Right-aligned on same line as date
- **Icon**: 📋 (for program workouts) or ✏️ (for custom workouts)
- **Size**: 20dp
- **Color**:
  - Program (📋): #A8E6CF (LimeGreenDark)
  - Custom (✏️): #666666 (TextOnLightTertiary)
```

**Recommended Update**:
```
**Workout Source Badge** (Right):
- **Position**: Right-aligned on same line as date
- **Program workouts**: Material Symbol "description" (Rounded, weight 500)
  - Size: 20dp
  - Color: #A8E6CF (LimeGreenDark)
- **Custom workouts**: Material Symbol "edit" (Rounded, weight 500)
  - Size: 20dp
  - Color: #666666 (TextOnLightTertiary)
```

**Rationale**: Replace emojis with Material Symbols Rounded for professional look.

**Examples Updated**:
```kotlin
// Program workout badge
ShapyfyIcon(
    name = "description",
    contentDescription = "Program workout",
    size = 20.dp,
    tint = Color(0xFFA8E6CF)
)

// Custom workout badge
ShapyfyIcon(
    name = "edit",
    contentDescription = "Custom workout",
    size = 20.dp,
    tint = Color(0xFF666666)
)
```

---

### 4. Empty State Icons (Optional Enhancement)

**Current State**: Empty state uses text-only interface

**Future Consideration** (post-MVP):
- Could add Material Symbol illustrations for "Get Workout Plan" and "Build My Own" paths
- Keep simple for now (text buttons sufficient)

---

## 📐 Spacing Audit

### Current Spacing Usage Across Dashboard

| Component | Padding/Margin | Status | Notes |
|-----------|----------------|--------|-------|
| Hero Card | 16dp padding | ✅ Consistent | Updated to 8dp system |
| Program Card | 16dp padding | ✅ Consistent | Follows standard |
| Recent Workouts items | 16dp padding | ✅ Consistent | Follows standard |
| Top Bar | 16dp horizontal | ✅ Consistent | Standard padding |
| Content Area | 16dp horizontal, 16dp top, 80dp bottom | ✅ Consistent | Safe area handling |

**Verdict**: Spacing is already consistent across dashboard. ✅

---

## 🎨 Typography Audit

### Current Typography Across Dashboard

| Element | Size / Weight | Status | Notes |
|---------|---------------|--------|-------|
| Top Bar Logo | 20sp Bold | ✅ Consistent | Appropriate for header |
| Section Headers | 18sp Bold | ✅ Consistent | "This Week", "Recent Workouts" |
| Goal Labels | 16sp Medium | ✅ Consistent | Updated in v2.1 |
| Primary Metrics | 24sp Bold | ✅ Consistent | Updated in v2.1 |
| Body Text | 16sp Regular | ✅ Consistent | Standard body |
| Messaging | 15sp Medium/Regular | ✅ Consistent | Updated in v2.1 |
| Secondary Text | 14sp Regular/Medium | ✅ Consistent | Buttons, labels |
| Tertiary Text | 12sp Regular/Medium | ✅ Consistent | Metadata, timestamps |

**Verdict**: Typography hierarchy is consistent and well-balanced. ✅

---

## 🎯 Border Radius Audit

### Current Border Radius Across Dashboard

| Element | Radius | Status | Notes |
|---------|--------|--------|-------|
| Cards (Hero, Program) | 16dp | ✅ Consistent | Top-level containers |
| Liquid Container | 8dp | ✅ Consistent | Nested element |
| Buttons (Primary) | 12dp | ⚠️ Check | Should align with 8dp or 16dp? |
| Buttons (Secondary) | 8dp | ✅ Consistent | Matches hierarchy |
| Recent Workout items | 12dp | ⚠️ Check | Should align with 8dp or 16dp? |

**Recommendation**:
- **Primary Buttons**: Keep 12dp (midpoint works for finger-friendly tappable elements)
- **Recent Workout items**: Keep 12dp (same rationale - tappable cards)
- **Hierarchy**: 16dp (cards) → 12dp (tappable elements) → 8dp (nested UI)

**Verdict**: Border radius is logical and functional. ✅ (with minor adjustment to thinking)

---

## 🔍 Implementation Priority

### High Priority (Before MVP Launch)
1. ✅ **Hero Card Material Symbols** - DONE (v2.1)
2. 🔧 **Recent Workouts badges** - Replace 📋/✏️ with Material Symbols `description`/`edit`
3. 🔧 **Program Card icon** - Replace 📋 with Material Symbol `description`
4. 🔧 **Program Completed checkmark** - Replace ✅ with Material Symbol `check_circle`

### Medium Priority (Nice to Have)
5. 📝 Document all icon decisions in `material-symbols-icon-guide.md` (add Recent Workouts + Program Card icons)

### Low Priority (Post-MVP)
6. Consider custom Shapyfy icon set to replace Material Symbols (if brand needs more unique look)

---

## 📋 Developer Handoff Checklist

### For Hero Card (v2.1) ✅
- [x] Material Symbols Rounded dependency added
- [x] All 7 icons implemented with correct weights (500)
- [x] Spacing updated to 8dp system (16dp padding)
- [x] Typography updated (24sp volume, 16sp goal, 15sp messaging)
- [x] Animations simplified (6 bubbles, slower speeds, subtle wave)
- [x] Grid metrics removed
- [x] Colors updated (#B0EDD5 gradient, #FFB800 gold)
- [x] WCAG AA compliance achieved (#666666 labels)

### For Other Dashboard Components 🔧
- [ ] Recent Workouts: Replace emoji badges with Material Symbols
  - Program: `description` icon (20dp, #A8E6CF)
  - Custom: `edit` icon (20dp, #666666)
- [ ] Program Card: Replace emoji with `description` icon (20dp, #A8E6CF)
- [ ] Program Completed: Replace ✅ with `check_circle` icon (20dp, #4CAF50)
- [ ] Update all icon references in 3-dashboard.md
- [ ] Add new icons to material-symbols-icon-guide.md

---

## 🎯 Success Criteria

### Visual Consistency
- ✅ All icons use Material Symbols Rounded (weight 500)
- ✅ No emoji placeholders in production code
- ✅ Consistent spacing (8dp system: 8, 12, 16, 24dp)
- ✅ Consistent typography hierarchy (12sp → 14sp → 15sp → 16sp → 18sp → 24sp)

### Accessibility
- ✅ WCAG AA contrast ratios met (4.5:1 minimum)
- ✅ Touch targets ≥48dp (buttons, tappable cards)
- ✅ Screen reader support (contentDescription on all icons)
- ✅ High contrast mode compatible (not color-dependent)

### Performance
- ✅ Animations respect "reduce motion" setting
- ✅ GPU-accelerated rendering (Canvas/Compose)
- ✅ 60fps target on mid-range devices (2018+)

---

## 📝 Next Steps

1. **Update 3-dashboard.md** with Recent Workouts + Program Card icon changes ⏳
2. **Update material-symbols-icon-guide.md** with 3 additional icons ⏳
3. **Developer implementation** of icon changes ⏳
4. **Visual QA** - verify all components look cohesive ⏳
5. **Accessibility testing** - TalkBack/VoiceOver, high contrast ⏳

---

## ✅ Sign-Off

**Hero Card v2.1**: Approved and Documented ✅
**Other Components**: Recommendations provided, awaiting implementation ⏳

**Documentation Owner**: UX Designer
**Last Review**: 2025-11-17
**Next Review**: After implementing recommended icon changes
