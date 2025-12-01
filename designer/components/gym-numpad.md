# Gym Numpad Component

**Version:** 1.3 (Phase 1 MVP — Premium Polish + UX Refinements)
**Status:** ✅ Client Approved — Ready for Development
**Design State:** Finished (Premium Grade)
**Component Type:** Reusable Input Field + Bottom Sheet (All-in-One)

---

## 1. Component Overview

### Purpose
A **gym-optimized numeric input component** that combines a visible input field with a custom bottom sheet numpad. Designed for speed, accuracy, and tactile satisfaction in workout environments. The component provides:
- **Input Field (Closed State)**: Visible in forms, shows current value, tap target to open numpad
- **Bottom Sheet Numpad (Open State)**: Custom keyboard optimized for gym use
- Large touch targets (sweaty fingers, gloves, fatigue)
- Dark mode excellence (primary use case)
- Haptic richness (every interaction feels premium)
- Zero cognitive load (clear, immediate feedback)

### Use Cases
| Screen | Input Type | Context | Configuration |
|--------|-----------|---------|---------------|
| **Target Config (5.04)** | Weight, Reps | Planning mode, calmer | Decimals ON for weight, OFF for reps |
| **Active Workout (2.02)** | Weight, Reps | Speed-critical, gym | Same as above + quick increments (Phase 2) |
| **Plan Builder** | Weight, Reps, Rest Time | Planning | Multiple unit types |
| **Profile Settings** | Body Weight, 1RM | Infrequent | Decimals ON |

### Design Philosophy
- **"Soft Tech / Calm Focus"**: No aggressive animations, calm glows, satisfying haptics
- **"Zero Friction"**: Instant feedback, no lag, optimistic UI
- **"Excel Killer"**: Data-dense inputs but premium feel
- **Dark Mode Hero**: Designed for #2D3142 (DarkNavy) first, light mode second

---

## 2. Visual Preview (Dark Mode — Hero Experience)

### Closed State (Input Field)
```
┌─────────────────────────────────────────┐
│ Weight (kg)                             │ ← Label
│ ┌─────────────────────────────────────┐ │
│ │  52.5                  kg       ▼  │ │ ← Field (tap to open)
│ └─────────────────────────────────────┘ │
└─────────────────────────────────────────┘
    ↑ Value Display      ↑ Unit   ↑ Chevron
```

### Open State (Bottom Sheet Numpad - Phase 2)
```
┌─────────────────────────────────────────┐
│  Weight (kg)                      [✓]  │ ← Header
├─────────────────────────────────────────┤
│                                         │
│          ┌─────────────┐               │
│          │    52.5     │  kg           │ ← Display (48sp, glowing)
│          └─────────────┘               │
│                                         │
│  ⦿ [ 20 ] [ 40 ] [ 60 ] [ 80 ] [100]  │ ← Presets (outlined pills, ⦿=active)
│                                         │
├─────────────────────────────────────────┤
│                                         │
│    [  1  ]  [  2  ]  [  3  ]   [+2.5↑] │ ← Number grid + Quick increments
│    [  4  ]  [  5  ]  [  6  ]   [ +5↑] │
│    [  7  ]  [  8  ]  [  9  ]   [+10↑] │
│    [  .  ]  [  0  ]  [  ⌫  ]           │
│                                         │
│  ┌───────────────────────────────────┐ │
│  │           Done                    │ │ ← Primary action (glowing)
│  └───────────────────────────────────┘ │
│                                         │
└─────────────────────────────────────────┘
```

---

## 3. Component API (Props)

### TypeScript Interface

```typescript
interface GymNumpadProps {
  // === Data ===
  value: number;                  // Current value (controlled component)
  unit: 'kg' | 'lbs' | 'reps' | 'seconds' | null;  // Display unit

  // === Constraints ===
  min?: number;                   // Minimum value (e.g., 0 for weight, 1 for reps)
  max?: number;                   // Maximum value (e.g., 500 for weight)
  allowDecimals: boolean;         // true for weight, false for reps
  step?: number;                  // Increment step (e.g., 0.5 for kg, 1 for reps)

  // === Callbacks ===
  onConfirm: (value: number) => void;   // User taps Done (value committed)
  onCancel?: () => void;                // User taps X or swipes down
  onChange?: (value: number) => void;   // Live value updates (optional)

  // === Field Configuration ===
  label: string;                  // Field label (e.g., "Weight (kg)")
  placeholder?: string;           // Placeholder when value is empty (e.g., "Enter weight")
  error?: string;                 // Error message to display below field
  disabled?: boolean;             // Disables interaction (default: false)
  required?: boolean;             // Shows required indicator (*) (default: false)

  // === Behavior ===
  autoDismiss?: boolean;          // Auto-close on Done (default: true)
  showCancelButton?: boolean;     // Show X in header (default: true)

  // === Styling ===
  theme?: 'dark' | 'light' | 'auto';  // Color scheme (default: 'auto' follows system)
}
```

### Example Usage

```typescript
// Weight input (allows decimals)
<GymNumpad
  value={weight}
  unit="kg"
  min={0}
  max={500}
  allowDecimals={true}
  step={0.5}
  label="Weight (kg)"
  onConfirm={(value) => setWeight(value)}
  theme="dark"
/>

// Reps input (no decimals, with validation)
<GymNumpad
  value={reps}
  unit="reps"
  min={1}
  max={100}
  allowDecimals={false}
  label="Reps"
  required={true}
  error={reps === 0 ? "Reps must be at least 1" : undefined}
  onConfirm={(value) => setReps(value)}
/>

// Rest timer (seconds, with placeholder)
<GymNumpad
  value={restTime}
  unit="seconds"
  min={0}
  max={600}
  allowDecimals={false}
  label="Rest Time"
  placeholder="Enter rest time"
  onConfirm={(value) => setRestTime(value)}
/>

// Disabled state (e.g., during workout)
<GymNumpad
  value={targetWeight}
  unit="kg"
  label="Target Weight"
  allowDecimals={true}
  disabled={true}  // Read-only during active workout
  onConfirm={(value) => {}}
/>
```

---

## 4. Detailed Design Specification

### <gym_numpad_field_dark_mode>

**Design Version**: v1.0 (2025-01-XX) — Phase 1 MVP

---

## Part A: Input Field (Closed State)

This is the visible field that appears in forms. Users tap this to open the bottom sheet numpad.

---

#### Field Container

**Appearance:**
- Background: #2D3142 (DarkNavy, matches sheet container for visual consistency)
- Border radius: 12dp
- Width: Full width (minus parent padding)
- Height: 56dp (same as Done button for consistency)
- Border: 1dp solid #4A4F62 (subtle outline)
- Padding horizontal: 16dp
- Padding vertical: 16dp
- Shadow: Elevation 2dp (subtle lift, #000000 at 20% opacity, blur 4dp)

**Touch Target:**
- Full field is tappable (56dp height meets accessibility minimum)
- Ripple effect on tap: #A8E6CF at 10% opacity (mint ripple)
- Tap triggers: Opens bottom sheet numpad

**Default State (Not Focused):**
- Border: 1dp solid #4A4F62 (subtle gray)
- Background: #2D3142
- Cursor: Not visible (read-only display)
- Shadow: Elevation 2dp

**Focused State (Sheet is Open):**
- Border: 2dp solid #A8E6CF (mint highlight)
- Background: #2D3142 (same)
- Border animates: width 1dp → 2dp (100ms, FastOutSlowInEasing)
- Glow (Double-Layer Premium Effect):
  - **Inner Glow**: 0dp 0dp 6dp #A8E6CF at 40% opacity (bright mint core)
  - **Outer Glow**: 0dp 0dp 16dp #2D7A5F at 15% opacity (subtle green halo)
  - **Breathing Animation**: Inner glow pulses 40% → 45% → 40% (2000ms loop, sine wave)
- Shadow: Elevation 4dp (lifts higher when focused)

**Error State:**
- Border: 2dp solid #FF6B6B (error red)
- Background: #2D3142
- Glow (Double-Layer Error Effect):
  - **Inner Glow**: 0dp 0dp 6dp #FF6B6B at 40% opacity (bright red core)
  - **Outer Glow**: 0dp 0dp 16dp #FF6B6B at 15% opacity (red halo)
- Shake animation on error appearance: translateX -4dp → +4dp → -2dp → 0 (200ms)
- Shadow: Elevation 2dp

**Disabled State:**
- Background: #2D3142 (same as default)
- Border: 1dp solid #4A4F62 (grayed out)
- Opacity: 0.5 (entire field)
- No interaction (no tap, no ripple)
- Cursor: Not allowed (system cursor)
- Shadow: None

---

#### Label Text (Above Field)

**Appearance:**
- Text: Prop value (e.g., "Weight (kg)", "Reps")
- Font: 14sp / Medium
- Color: #B8BCC8 (Text Secondary)
- Position: Above field, margin-bottom 8dp
- Alignment: Left

**Required Indicator (If required={true}):**
- Append asterisk: "Weight (kg) *"
- Asterisk color: #FF6B6B (error red)
- Font: 14sp / Medium

**Behavior on Focus:**
- Color: #B8BCC8 → #FFFFFF (pure white, WCAG AA compliant 15.3:1 contrast)
- Animation: 150ms color transition
- Subtle glow: 0dp 0dp 4dp #A8E6CF at 20% opacity (hint of mint aura)

**Behavior on Error:**
- Color: #B8BCC8 → #FF6B6B (error red, 4.8:1 contrast - WCAG AA compliant)
- Animation: 100ms color transition

---

#### Value Display (Inside Field)

**Container:**
- Position: Left side of field, vertical center
- Flex: 1 (takes available space before unit + chevron)

**Text Display (When Value Exists):**
- Text: Current value formatted (e.g., "52.5", "10", "90")
- Font: 18sp / Bold
- Color: #FFFFFF (Text Primary)
- Letter spacing: -0.3sp (tighter for numbers)
- Alignment: Left

**Placeholder Display (When Value is Empty):**
- Text: Placeholder prop value (e.g., "Enter weight") OR default "Tap to enter"
- Font: 18sp / Regular (not bold)
- Color: #666666 (dimmer than secondary text)
- Alignment: Left
- Italic: Optional (can be applied for clarity)

**Number Formatting:**
- **Trailing Zeros (Always Hidden):**
  - 52.0 → "52" (strip unnecessary .0)
  - 50.00 → "50" (strip all trailing zeros)
  - 52.50 → "52.5" (strip trailing zero, keep meaningful decimal)
  - Rule: Strip all trailing zeros after decimal point
- **Meaningful Decimals (Always Shown):**
  - 52.5 → "52.5" (half-kilo increment)
  - 52.25 → "52.25" (quarter-kilo increment)
  - 0.5 → "0.5" (fractional value)
  - Rule: Preserve decimals that add semantic value
- **Whole Numbers (No Decimal Point):**
  - 10 → "10"
  - 100 → "100"
  - 10.0 → "10"
  - Rule: Whole numbers never show .0
- **Leading Zeros (Always Removed):**
  - 05 → "5"
  - 00.5 → "0.5"
  - Rule: Strip leading zeros (except 0.x format)
- **Trailing Decimal Point (Stripped on Confirm):**
  - User types: "52." → On Done: Stored as 52, displayed as "52"
  - Rule: Incomplete decimals sanitized to whole numbers

---

#### Unit Label (Inside Field)

**Appearance:**
- Text: Unit prop value (e.g., "kg", "lbs", "reps", "sec")
- Font: 16sp / Regular
- Color: #B8BCC8 (Text Secondary)
- Position: Right of value, before chevron
- Margin left: 8dp (from value)
- Margin right: 8dp (from chevron)
- Vertical alignment: Center

**Conditional Display:**
- Show if unit prop is provided (not null)
- Hide if unit={null}

---

#### Chevron Icon (Inside Field, Right Side)

**Appearance:**
- Icon: Material Symbol `expand_more` (Rounded, weight 400)
- Size: 20dp
- Color: #B8BCC8 (Text Secondary)
- Position: Right-aligned, vertical center
- Margin right: 0dp (already padded by container)

**Animation on Tap (Premium Spring Physics):**
- Rotate: 0deg → 190deg → 180deg (overshoot with spring-back for tactile feel)
- Scale: 1.0 → 1.1 → 1.0 (subtle pump during rotation)
- Duration: 300ms
- Easing: Spring (damping 0.7, stiffness 350)
- Color: #B8BCC8 → #FFFFFF (brightens when active)
- On dismiss: 180deg → -10deg → 0deg (reverse overshoot), scale 1.0 → 1.05 → 1.0

**Interaction:**
- Not separately interactive (entire field is tap target)
- Visual indicator only

---

#### Error Message (Below Field)

**Appearance:**
- Text: Error prop value (e.g., "Reps must be at least 1")
- Font: 12sp / Regular
- Color: #FF6B6B (Error Red)
- Position: Below field, margin-top 4dp
- Alignment: Left
- Icon: Material Symbol `error` (Rounded, weight 400, 16dp, #FF6B6B) — positioned before text
- Icon margin-right: 4dp

**Conditional Display:**
- Show if error prop is provided (not null/undefined)
- Hide if error={undefined}

**Animation on Appear:**
- Slide down + fade in: translateY -8dp → 0dp, opacity 0 → 1 (200ms)
- Easing: FastOutSlowInEasing

**Animation on Disappear:**
- Fade out: opacity 1 → 0 (150ms)
- Easing: LinearEasing

---

#### Field State Transitions

**Idle → Tap (Opening Sheet):**
1. Ripple effect emanates from tap point (#A8E6CF, 10% opacity, 300ms)
2. Border: 1dp #4A4F62 → 2dp #A8E6CF (100ms)
3. Double-layer glow appears (see Focused State spec above)
4. Chevron rotates: 0deg → 190deg → 180deg (300ms, spring overshoot)
5. Chevron scales: 1.0 → 1.1 → 1.0 (300ms)
6. Chevron color: #B8BCC8 → #FFFFFF (300ms)
7. Label color: #B8BCC8 → #FFFFFF (150ms)
8. Label glow appears: 0dp 0dp 4dp #A8E6CF at 20%
9. Shadow: Elevation 2dp → 4dp (field lifts)
10. Haptic: Light impact
11. Bottom sheet slides up (see Animation Choreography section)

**Sheet Open → Confirm (Value Changed):**
1. Value in field updates: Old value → New value (instant, no animation in field)
2. Sheet slides down (exit animation)
3. Border: 2dp #A8E6CF → 1dp #4A4F62 (150ms, after sheet closes)
4. Double-layer glow fades out (200ms)
5. Breathing animation stops
6. Chevron rotates: 180deg → -10deg → 0deg (300ms, reverse overshoot)
7. Chevron scales: 1.0 → 1.05 → 1.0 (300ms)
8. Chevron color: #FFFFFF → #B8BCC8 (300ms)
9. Label color: #FFFFFF → #B8BCC8 (150ms)
10. Label glow fades out (150ms)
11. Shadow: Elevation 4dp → 2dp (field settles)
12. Haptic: Selection (subtle closure signal)

**Sheet Open → Cancel (Value Unchanged):**
- Same animation as Confirm, but value remains unchanged
- Haptic: Selection (same as Confirm)

**Idle → Error (Validation Fails):**
1. Border: 1dp #4A4F62 → 2dp #FF6B6B (100ms)
2. Glow: #A8E6CF → #FF6B6B (100ms)
3. Shake animation: translateX -4dp → +4dp → -2dp → 0 (200ms)
4. Label color: #B8BCC8 → #FF6B6B (100ms)
5. Error message slides down (translateY -8dp → 0, opacity 0 → 1, 200ms)
6. Haptic: Notification (error pattern)

**Error → Valid (Error Clears):**
1. Border: 2dp #FF6B6B → 1dp #4A4F62 (150ms)
2. Glow fades: opacity 20% → 0 (200ms)
3. Label color: #FF6B6B → #B8BCC8 (150ms)
4. Error message fades out: opacity 1 → 0 (150ms)

**Any State → Disabled:**
1. Opacity: 1.0 → 0.5 (200ms)
2. Background: Current → #2D3142 (200ms)
3. Border: Current → 1dp #4A4F62 (200ms)
4. All glows/animations cancel

---

#### Accessibility (Field)

**Screen Reader Support:**
- Label: Announced as "Label, [label text]. Input field."
- Value: Announced as "Current value: [value] [unit]"
- Placeholder: Announced as "Placeholder: [placeholder text]"
- Required: Announced as "Required field"
- Error: Announced as "Error: [error message]"
- Disabled: Announced as "Disabled. [label text]."

**Focus Order:**
- Field receives focus in tab order (position determined by parent form)
- Tapping/Enter key: Opens bottom sheet numpad

**Keyboard Support:**
- Tab: Focus field
- Enter/Space: Open numpad sheet
- Escape: If sheet is open, close it

**Touch Target:**
- Minimum: 56dp height (exceeds WCAG 44dp requirement)
- Width: Full width available (easy to tap)

**High Contrast Mode:**
- Border width: 1dp → 3dp (all states)
- Border color: High contrast variants (#FFFFFF for focus, #FF0000 for error)
- Background: Darker (#1A1A1A)

---

### </gym_numpad_field_dark_mode>

---

### <gym_numpad_sheet_dark_mode>

**Design Version**: v1.0 (2025-01-XX) — Phase 1 MVP

---

## Part B: Bottom Sheet Numpad (Open State)

This is the custom keyboard that slides up when the field is tapped.

---

#### Bottom Sheet Container

**Appearance:**
- Background: #2D3142 (DarkNavy from branding.md)
- Border radius: 24dp (top corners only)
- Shadow: Elevation 24dp (strong upward shadow, #000000 at 40% opacity, blur 32dp)
- Width: Full screen width
- Height: Auto (based on content, ~420dp typical)
- Safe area: Padding bottom = device safe area inset (accounts for home indicator)

**Entry Animation:**
- Slide up from bottom: translateY(100%) → translateY(0)
- Duration: 350ms
- Easing: Spring (damping 0.8, stiffness 400)
- Backdrop overlay: #000000 opacity 0 → 0.6 (fades in, 300ms)

**Exit Animation:**
- Slide down: translateY(0) → translateY(100%)
- Duration: 300ms
- Easing: FastOutLinearInEasing
- Backdrop fades out: opacity 0.6 → 0 (200ms)

**Dismissal Triggers:**
- Tap backdrop (outside sheet)
- Swipe down on drag handle
- Tap Done button (if autoDismiss=true)
- Tap X button (if showCancelButton=true)

---

#### Drag Handle (Swipe Indicator)

**Appearance:**
- Background: #4A4F62 (lighter than container, subtle)
- Width: 40dp
- Height: 4dp
- Border radius: 2dp (pill shape)
- Position: Centered horizontally, margin-top 12dp

**Interaction:**
- Swipe down gesture: Sheet follows finger (spring physics)
- Release below 50% screen → Dismiss
- Release above 50% → Snap back (spring)

---

#### Header Bar

**Container:**
- Background: #3A3F52 (elevated surface, darker than branding.md Surface)
- Height: 56dp
- Padding horizontal: 20dp
- Border bottom: 1dp solid #4A4F62 (subtle divider)

**Label Text (Left):**
- Text: Dynamic (e.g., "Weight (kg)", "Reps", "Rest Time")
- Font: 16sp / Medium
- Color: #FFFFFF (Text Primary from branding.md)
- Position: Left-aligned, vertical center

**Cancel Button (Right):**
- Icon: Material Symbol `close` (Rounded, weight 500)
- Size: 24dp
- Color: #B8BCC8 (Text Secondary equivalent for dark)
- Touch target: 48dp diameter (circular)
- Position: Right-aligned, vertical center
- Pressed state: Background #4A4F62 (circle fills), scale 0.95, duration 100ms
- Haptic: Light impact

**Conditional Display:**
- Show if `showCancelButton={true}` (default)
- Hide if false

---

#### Display Area (Current Value)

**Container:**
- Background: Transparent
- Padding vertical: 24dp
- Padding horizontal: 20dp
- Border bottom: 1dp solid #4A4F62

**Value Display:**
- Text: Current numeric value (e.g., "52.5", "10", "90")
- Font: 48sp / Bold
- Color: #FFFFFF (white)
- Glow effect:
  - Shadow: 0dp 0dp 16dp #A8E6CF at 30% opacity (soft mint glow)
  - Creates depth and premium feel
- Alignment: Center horizontal
- Letter spacing: -0.5sp (tighter for numbers)

**Unit Label (if provided):**
- Text: Unit string (e.g., "kg", "reps", "sec")
- Font: 16sp / Regular
- Color: #B8BCC8 (dimmer than value)
- Position: To the right of value, baseline-aligned
- Margin left: 8dp

**Value Replacement Behavior:**
- **Default Mode (Sheet Opens):**
  - Display shows existing value (e.g., "52.5")
  - Value is visually "selected" (subtle opacity reduction to 90%)
  - Glow pulses (breathing animation continues)
- **Replace Mode (User Taps Any Number First):**
  - First number tap → Old value cleared, new value starts
  - Example: Display shows "52.5", user taps "6" → Display becomes "6"
  - Animation: Old value fades out (100ms), new value fades in (150ms)
  - Glow pulse confirms action
  - Rationale: Speed-critical gym context; complete value changes are most common
- **Edit Mode (User Taps Backspace First):**
  - Backspace tap → Last digit removed, edit mode activated
  - Example: Display shows "52.5", user taps ⌫ → Display becomes "52."
  - Glow stops pulsing (signals edit mode)
  - User can continue backspacing or append new digits
  - Rationale: Provides escape hatch for small edits (52.5 → 52.0)
- **Reset Mode (User Taps Clear Button):**
  - Clear tap → Value resets to "0" (always)
  - Example: Display shows "52.5", user taps C → Display becomes "0"
  - Animation: Shake + fade out old value, fade in "0"

**Animation on Value Change (Premium "Weightful" Counter):**
- **Counter Animation** (slot machine effect with mass):
  - Old value: opacity 1 → 0, translateY 0 → -20dp (slides up, fades out)
  - New value: opacity 0 → 1, translateY 20dp → 0 (slides up from below, fades in)
  - **Scale Bounce**: 1.0 → 0.98 → 1.02 → 1.0 (creates "landing" weight effect)
  - **Motion Blur**: 0dp → 2dp → 0dp blur during slide (adds velocity feel)
  - Duration: 200ms
  - Easing: FastOutSlowInEasing
  - Stagger: Digit-by-digit if multiple digits change (50ms offset creates ripple effect)
- **Glow Pulse** (on change):
  - Glow opacity: 30% → 50% → 30%
  - Duration: 300ms
  - Easing: Sine wave

**Invalid Value State:**
- If value < min OR value > max:
  - Text color: #FF6B6B (Error Accent from branding.md)
  - Glow color: #FF6B6B (red glow instead of mint)
  - Shake animation: translateX -8dp → +8dp → 0 (wiggle, 300ms)
  - Haptic: Notification (error pattern)

---

#### Number Grid (Keypad)

**Container:**
- Background: Transparent
- Padding: 16dp all sides
- Grid layout: 4 columns × 4 rows
- Gap between buttons: 12dp (both horizontal and vertical)

**Button Layout:**
```
Row 1:  [1]  [2]  [3]  [←]
Row 2:  [4]  [5]  [6]  [C]
Row 3:  [7]  [8]  [9]
Row 4:  [.]  [0]  [⌫]
```

**Number Buttons (0-9):**

*Default State:*
- Background: #4A4F62 (elevated button surface)
- Border radius: 12dp
- Width: 56dp
- Height: 56dp
- Text: Number (e.g., "1", "2", "0")
- Font: 24sp / Bold
- Color: #FFFFFF
- Shadow: Elevation 2dp (subtle depth, #000000 at 30% opacity, blur 4dp)

*Pressed State (Premium Tactile Effect):*
- Background: #A8E6CF (Brand Mint — active glow)
- Scale: 0.95 (compress slightly, creates "sinking" effect)
- Shadow (Outer): Elevation 0dp (pressed flat against surface)
- **Shadow (Inner)**: inset 0dp 2dp 4dp rgba(0,0,0,0.2) (depth illusion)
- Text color: #1A1A1A (dark text on mint background)
- Duration: 100ms
- Easing: FastOutLinearInEasing (snappy press feel)
- Glow: 0dp 0dp 12dp #A8E6CF at 50% opacity (radiates outward)
- Haptic: Light impact

*Disabled State (if not applicable):*
- Not used for numbers (always enabled)

**Decimal Button [.]:**

*Default State:*
- Same styling as number buttons
- Text: "." (period)

*Conditional Display:*
- Show if `allowDecimals={true}`
- Hide if `allowDecimals={false}` (button space collapses, grid adjusts)

*Disabled State (when decimal already entered):*
- Background: #3A3F52 (dimmer)
- Text color: #666666 (grayed out)
- No press interaction
- Explanation: Only one decimal point allowed

**Backspace Button [⌫]:**

*Default State:*
- Background: #4A4F62
- Icon: Material Symbol `backspace` (Rounded, weight 500)
- Size: 20dp
- Color: #FFFFFF

*Pressed State:*
- Background: #FF9999 (Coral — destructive hint, subtle)
- Icon color: #1A1A1A
- Scale: 0.95
- Haptic: Light impact

*Long Press Behavior:*
- Hold for 500ms → Clear entire value (resets to 0)
- Visual feedback: Button pulses every 100ms while held
- Haptic: Medium impact on clear trigger

**Clear Button [C]:**

*Default State:*
- Background: #4A4F62
- Text: "C"
- Font: 20sp / Bold
- Color: #FFFFFF

*Pressed State:*
- Background: #FF9999 (Coral — destructive)
- Text color: #1A1A1A
- Scale: 0.95
- Haptic: Medium impact

*Tap Behavior:*
- Clears entire value → Resets to 0
- Display value animates: Fade out + fade in 0 (300ms)

**Navigation Buttons (Top Row Right: [←], [C]):**

*[←] Back/Delete (Row 1, Column 4):*
- Same as Backspace button (duplicate for ergonomics)
- Some users prefer top-right position

*[C] Clear (Row 2, Column 4):*
- Already specified above

**Empty Grid Cells:**
- Row 3, Column 4: Empty (no button)
- Background: Transparent
- Maintains grid alignment

---

#### Preset Buttons (Phase 2 — Quick Picks Row)

**Container:**
- Position: Between display area and number grid
- Padding horizontal: 20dp
- Padding vertical: 12dp
- Layout: Horizontal row, centered
- Gap between buttons: 8dp

**Purpose:**
- Quick access to common values (e.g., 20, 40, 60, 80, 100 kg for weight; 5, 8, 10, 12 for reps)
- Configurable per use case via props
- Active state shows current value

**Preset Button (Default State):**
- Style: **Outlined pill** (not filled, clearly different from number buttons)
- Background: Transparent
- Border: 1.5dp solid #4A4F62 (neutral gray)
- Border radius: 18dp (pill shape)
- Width: Auto (fits content)
- Height: 36dp (compact, not 56dp like grid buttons)
- Padding horizontal: 12dp
- Text: Preset value (e.g., "20", "40", "60")
- Font: 14sp / Medium
- Color: #FFFFFF
- Shadow: None (outlined style, no elevation)

**Active State (Current Value):**
- Indicates which preset matches the current display value
- Background: #2D7A5F at 20% opacity (subtle mint fill)
- Border: 2dp solid #A8E6CF (mint highlight)
- Glow: 0dp 0dp 8dp #A8E6CF at 30% (soft mint glow)
- Icon: ✓ checkmark before text (12dp, #A8E6CF)
- Text: Same as default (#FFFFFF)
- Animation on becoming active: Border animates 1.5dp → 2dp, glow fades in (150ms)

**Pressed State (User Taps Preset):**
- Background: #2D7A5F (solid green fill, instant feedback)
- Border: 2dp solid #A8E6CF
- Scale: 1.0 → 0.95 (compress, 50ms)
- Text color: #FFFFFF (white on green)
- Glow: 0dp 0dp 12dp #A8E6CF at 50% (intensified)
- Haptic: Light impact (on press start)
- Duration: Total 200ms (50ms compress + 150ms spring back)
- Release: Scale 0.95 → 1.0 (spring, damping 0.7)

**Tap Behavior:**
1. User taps preset (e.g., "60")
2. Preset compresses (scale 0.95) + fills green
3. Display value updates: Old value → 60 (slot machine animation)
4. Previous active preset loses active state (border #A8E6CF → #4A4F62, glow fades)
5. New preset gains active state (border #4A4F62 → #A8E6CF, glow fades in, ✓ appears)
6. Preset returns to active state styling (outlined + mint border + ✓)
7. Haptics: Light on press, Selection on value change

**Disabled State (Not Used):**
- Preset buttons are always enabled (all values are valid in Phase 2)

**Accessibility:**
- Label: "Preset button, [value] [unit]. Tap to select."
- Active state: "Selected, [value] [unit]."
- Focus order: Left to right across row
- Keyboard: Tab to focus, Enter/Space to activate

**Configuration Examples:**
```typescript
// Weight presets (kg)
presets={[20, 40, 60, 80, 100]}

// Reps presets
presets={[5, 8, 10, 12]}

// Rest time presets (seconds)
presets={[30, 60, 90, 120, 180]}

// No presets (hide row entirely)
presets={[]} // or presets={undefined}
```

---

#### Quick Increment Buttons (Phase 2 — Right Column)

**Container:**
- Position: Right column of number grid (Column 4)
- Alignment: Aligned with grid rows 1-3
- Gap: 12dp vertical (same as grid)

**Purpose:**
- Micro-adjustments to current value (e.g., +2.5kg, +5kg, +10kg for weight; +1, +2, +5 for reps)
- Speed feature for small progressions (52.5kg → 55kg with one tap)
- Configurable per use case via props

**Quick Increment Button (Default State):**
- Style: **Outlined** (not filled, clearly different from number buttons)
- Background: Transparent
- Border: 1.5dp solid #A8E6CF (mint accent — key difference from presets)
- Border radius: 12dp (rounded square, not pill)
- Width: 56dp (same as grid buttons for consistency)
- Height: 56dp (same as grid buttons for muscle memory)
- Icon: ↑ arrow (16dp, #A8E6CF, positioned above text)
- Icon margin-bottom: 2dp (from text)
- Text: Increment value (e.g., "+1", "+2.5", "+5")
- Font: 18sp / Bold (same as numbers)
- Color: #A8E6CF (mint accent)
- Shadow: None (outlined style)
- Layout: Vertical stack (icon top, text bottom, centered)

**Pressed State (User Taps Increment):**
- Background: #A8E6CF at 20% opacity (mint tint)
- Border: 2dp solid #A8E6CF (thickens slightly)
- Scale: 1.0 → 0.95 (compress, snappy feedback)
- Glow: 0dp 0dp 12dp #A8E6CF at 40% (radiates mint glow)
- Icon + Text color: #A8E6CF (unchanged, maintains mint identity)
- Haptic: Medium impact (stronger than number tap — indicates value change)
- Duration: 100ms compress, 150ms spring release

**Tap Behavior:**
1. User taps +2.5 button (current value = 50)
2. Button compresses (scale 0.95) + mint tint background
3. Display value updates: 50 → 52.5 (slot machine animation)
4. Glow pulse on display (confirms change)
5. Button returns to default state (outlined, no fill)
6. Haptics: Medium impact on press, Light impact on value change
7. If result exceeds max: Display shows error state (red glow + shake), no value change

**Disabled State (Value Would Exceed Max):**
- Background: Transparent
- Border: 1.5dp solid #4A4F62 (grayed out, same as neutral buttons)
- Icon + Text color: #666666 (dimmed)
- Opacity: 0.5 (entire button)
- No press interaction
- Condition: currentValue + increment > max
- Example: If max=100 and current=98, "+5" button disables

**Visual Hierarchy (vs Number Buttons):**
| Element | Number Buttons | Quick Increments |
|---------|---------------|------------------|
| Style | Filled | Outlined |
| Background | #4A4F62 | Transparent |
| Border | None | 1.5dp solid #A8E6CF |
| Color | #FFFFFF | #A8E6CF (mint) |
| Icon | None | ↑ arrow |
| Purpose | Manual entry | Micro-adjustments |

**Configuration Examples:**
```typescript
// Weight increments (kg)
quickIncrements={[2.5, 5, 10]}

// Reps increments
quickIncrements={[1, 2, 5]}

// Rest time increments (seconds)
quickIncrements={[15, 30, 60]}

// No quick increments (hide right column)
quickIncrements={[]} // or quickIncrements={undefined}
```

**Accessibility:**
- Label: "Quick increment button, plus [value] [unit]. Tap to add."
- Disabled state: "Disabled. Adding [value] would exceed maximum."
- Focus order: After number grid, before Done button
- Keyboard: Tab to focus, Enter/Space to activate

**Positioning in Grid:**
```
Row 1: [1] [2] [3] [+2.5↑]  ← First increment
Row 2: [4] [5] [6] [ +5↑]   ← Second increment
Row 3: [7] [8] [9] [+10↑]   ← Third increment
Row 4: [.] [0] [⌫]          ← No increment (standard backspace)
```

---

#### Done Button (Primary Action)

**Container:**
- Position: Below number grid
- Padding horizontal: 16dp
- Padding bottom: 16dp + safe area inset (accounts for home indicator)
- Margin top: 16dp (from grid)

**Button:**

*Default State:*
- Background: #2D7A5F (Functional Primary from branding.md)
- Border radius: 12dp
- Width: Full width (minus padding)
- Height: 56dp
- Text: "Done"
- Font: 18sp / Bold
- Color: #FFFFFF
- Shadow: Elevation 4dp
- Glow: 0dp 0dp 16dp #2D7A5F at 20% opacity (subtle green halo)

*Pressed State:*
- Background: #257055 (~10% darker)
- Scale: 0.98
- Shadow: Elevation 2dp (pressed down)
- Glow opacity: 40% (intensifies)
- Duration: 100ms
- Haptic: Medium impact

*Disabled State (if value invalid):*
- Background: #4A4F62 (grayed out)
- Text color: #666666
- Shadow: None
- Glow: None
- No interaction
- Condition: value < min OR value > max

**Tap Behavior (When Valid):**
1. Button compresses (scale 0.98)
2. Success ripple emanates from tap point (#A8E6CF, 40% opacity, expands 300ms)
3. Calls `onConfirm(currentValue)`
4. If `autoDismiss={true}`: Sheet slides down (exit animation)
5. Haptic: Success pattern (light-medium-light, 100ms gaps)

---

#### Accessibility Features

**Screen Reader Support:**
- Header label: Announced as "Heading, [label text]"
- Display value: "Current value: [number] [unit]"
- Number buttons: "Button, [number]. Tap to enter."
- Backspace: "Button, delete last digit"
- Clear: "Button, clear all"
- Done: "Button, confirm value. [Value] [unit]"
- Invalid state: "Error. Value must be between [min] and [max]."

**Focus Order:**
1. Cancel button (X)
2. Display value (focusable, announces current value)
3. Number grid (1-9, 0, ., backspace, clear) — left-to-right, top-to-bottom
4. Done button

**Keyboard Support (for accessibility):**
- Number keys (0-9): Enter digit
- Decimal key (.): Enter decimal (if allowed)
- Backspace: Delete last digit
- Delete: Clear all
- Enter: Confirm (Done)
- Escape: Cancel (dismiss sheet)

**High Contrast Mode:**
- All borders become 2dp (from 1dp)
- Button borders: 2dp solid #FFFFFF
- Text contrast boosted: All grays → pure white or pure black

**Reduce Motion Support:**
- If system setting enabled:
  - Entry/exit: Fade only (no slide)
  - Value change: Crossfade only (no slot machine)
  - Button press: No scale, only color change
  - Glow effects: Disabled

**Dynamic Type Support:**
- Display value: Scales with system font size (48sp baseline, max 64sp)
- Button text: Scales proportionally (24sp baseline, max 32sp)
- Minimum button size: Always 56dp (never shrinks)

---

### </gym_numpad_sheet_dark_mode>

---

### <gym_numpad_field_light_mode>

**Design Version**: v1.0 (2025-01-XX) — Phase 1 MVP

---

## Part A: Input Field (Closed State) — Light Mode

All layout, spacing, sizing, and animations are **identical to dark mode**. Only colors change.

---

#### Color Palette Changes (vs Dark Mode)

| Element | Dark Mode | Light Mode |
|---------|-----------|------------|
| **Field Background** | #2D3142 (DarkNavy) | #FFFFFF (White) |
| **Field Border (Default)** | #4A4F62 | #E5E5E5 (Border) |
| **Field Border (Focused)** | #A8E6CF (Mint) | #2D7A5F (Functional Primary) |
| **Field Border (Error)** | #FF6B6B | #E05A5A (Error Dark) |
| **Field Background (Disabled)** | #2D3142 | #F3F3F8 (Page Background) |
| **Label Text** | #B8BCC8 | #666666 (Text Secondary) |
| **Label (Focused)** | #FFFFFF (White) | #1A1A1A (Black, WCAG AA) |
| **Label Glow (Focused)** | #A8E6CF at 20% | #2D7A5F at 10% |
| **Label (Error)** | #FF6B6B | #E05A5A |
| **Value Text** | #FFFFFF | #1A1A1A (Text Primary) |
| **Placeholder Text** | #666666 | #B8BCC8 (lighter than dark mode) |
| **Unit Text** | #B8BCC8 | #666666 |
| **Chevron Icon (Default)** | #B8BCC8 | #666666 |
| **Chevron Icon (Active)** | #FFFFFF | #1A1A1A |
| **Error Message** | #FF6B6B | #E05A5A |
| **Ripple Effect** | #A8E6CF at 10% | #2D7A5F at 10% |
| **Glow Inner (Focused)** | #A8E6CF at 40% | #2D7A5F at 30% |
| **Glow Outer (Focused)** | #2D7A5F at 15% | #2D7A5F at 10% |
| **Glow Inner (Error)** | #FF6B6B at 40% | #E05A5A at 30% |
| **Glow Outer (Error)** | #FF6B6B at 15% | #E05A5A at 10% |

#### Visual Adjustments (Light Mode)

**Field Glow:**
- Reduced to 10% opacity (vs 20% in dark mode)
- Light mode needs subtler glows to avoid overwhelming the white background

**Ripple Effect:**
- Color: #2D7A5F (Functional Primary, darker green)
- Opacity: 10% (same as dark mode)

**Shadow Adjustments:**
- Field shadow: None by default (light mode uses borders, not elevation)
- Focused shadow: Optional — 0dp 2dp 4dp #2D7A5F at 10% (very subtle)

**All Other Specs:**
- Layout, spacing, sizing, animations, haptics: **Identical to dark mode**

---

### </gym_numpad_field_light_mode>

---

### <gym_numpad_sheet_light_mode>

**Design Version**: v1.0 (2025-01-XX)

---

## Part B: Bottom Sheet Numpad (Open State) — Light Mode

**Context:** Light mode variant for planning screens in daylight environments (e.g., reviewing plan at home).

All layout, spacing, sizing, and animations are **identical to dark mode**. Only colors change.

---

#### Color Palette Changes (vs Dark Mode)

| Element | Dark Mode | Light Mode |
|---------|-----------|------------|
| **Container Background** | #2D3142 (DarkNavy) | #FFFFFF (White) |
| **Header Background** | #3A3F52 | #F3F3F8 (Page Background) |
| **Button Default** | #4A4F62 | #F0F0F5 (Surface) |
| **Button Pressed** | #A8E6CF (Mint) | #2D7A5F (Functional Primary) |
| **Text Primary** | #FFFFFF | #1A1A1A (Text Primary) |
| **Text Secondary** | #B8BCC8 | #666666 (Text Secondary) |
| **Dividers** | #4A4F62 | #E5E5E5 (Border) |
| **Display Glow** | #A8E6CF at 30% | #2D7A5F at 15% (subtler) |
| **Done Button** | #2D7A5F | #2D7A5F (same) |
| **Error Color** | #FF6B6B | #E05A5A (Error Dark) |

#### Visual Adjustments

**Display Value:**
- Glow: Reduced to 15% opacity (light mode needs subtlety)
- Color: #2D7A5F glow (darker green, not mint)

**Number Buttons:**

*Pressed State (Light Mode):*
- Background: #2D7A5F (Functional Primary — dark button press)
- Text color: #FFFFFF (white on dark green)
- Glow: 0dp 0dp 12dp #2D7A5F at 30% opacity

**Shadow Adjustments:**
- All shadows: Reduce opacity by 50% (light mode needs lighter shadows)
- Container shadow: #000000 at 20% (vs 40% in dark)
- Button shadows: Elevation 1dp (vs 2dp in dark)

**Done Button:**
- Same as dark mode (#2D7A5F background)
- Glow slightly reduced: 15% opacity (vs 20%)

**All Other Specs:**
- Spacing, sizing, animations, haptics: **Identical to dark mode**
- Only colors change

---

### </gym_numpad_sheet_light_mode>

---

## 5. Animation Choreography

### Field Tap to Sheet Open (Complete Flow)

**Total Duration:** 650ms (field animation + sheet entry)

**Phase 1: Field Response (0-150ms):**
1. User taps field
2. Ripple emanates from tap point (#A8E6CF, 10% opacity, expands 300ms)
3. Haptic: Light impact (on tap start)
4. Chevron rotates: 0deg → 180deg (300ms, starts immediately)
5. Border: 1dp #4A4F62 → 2dp #A8E6CF (100ms)
6. Glow appears: blur 0dp → 8dp #A8E6CF at 20% (200ms)
7. Label color: #B8BCC8 → #A8E6CF (150ms)

**Phase 2: Sheet Entry (Overlaps with Phase 1, starts at 50ms):**
(See "Entry Sequence" below for details)

---

### Entry Sequence (Bottom Sheet Opens)

**Total Duration:** 350ms

**Trigger:** Field is tapped OR programmatically opened

**Phase 1: Backdrop (0-300ms):**
- Overlay: #000000 opacity 0 → 0.6
- Easing: LinearEasing
- User can tap backdrop to dismiss immediately

**Phase 2: Sheet Slide (0-350ms):**
- Sheet: translateY(100%) → translateY(0)
- Easing: Spring (damping 0.8, stiffness 400)
- Overshoot: Slight bounce at end (spring natural behavior)

**Phase 3: Content Fade (100-350ms):**
- Header, Display, Grid, Done button: opacity 0 → 1
- Easing: FastOutSlowInEasing
- Stagger: 50ms delay between sections (Header → Display → Grid → Done)

---

### Value Change Animation

**Trigger:** User taps number/backspace/clear

**Digit Addition (User taps "5"):**
1. Old value "52" slides up: translateY 0 → -20dp, opacity 1 → 0 (150ms)
2. New value "525" slides in: translateY 20dp → 0, opacity 0 → 1 (200ms)
3. Glow pulse: opacity 30% → 50% → 30% (300ms, overlaps with slide)
4. Haptic: Light impact (on tap)

**Digit Removal (User taps backspace):**
1. Last digit "5" in "525" fades out: opacity 1 → 0 (100ms)
2. Remaining "52" slight scale bounce: scale 1.0 → 1.05 → 1.0 (200ms)
3. Haptic: Light impact

**Clear Action (User taps "C"):**
1. Entire value "525" shakes: translateX -8dp → +8dp → -4dp → 0 (200ms)
2. Then fades out: opacity 1 → 0 (150ms)
3. "0" fades in: opacity 0 → 1 (150ms)
4. Haptic: Medium impact

---

### Exit Sequence (Sheet Closes)

**Trigger:** User taps Done / Cancel / Backdrop / Swipes down

**Total Duration:** 450ms (sheet exit + field reset)

**Phase 1: Sheet Exit (0-300ms):**
1. Content Fade (0-150ms):
   - All content: opacity 1 → 0
   - Easing: LinearEasing
2. Sheet Slide (0-300ms):
   - Sheet: translateY(0) → translateY(100%)
   - Easing: FastOutLinearInEasing (accelerates downward)
3. Backdrop (100-300ms):
   - Overlay: opacity 0.6 → 0
   - Easing: LinearEasing

**Phase 2: Field Reset (Starts at 150ms, overlaps with sheet exit):**
1. Value updates (if confirmed): Old value → New value (instant)
2. Border: 2dp #A8E6CF → 1dp #4A4F62 (150ms)
3. Glow fades: opacity 20% → 0 (200ms)
4. Chevron rotates: 180deg → 0deg (300ms)
5. Label color: #A8E6CF → #B8BCC8 (150ms)

**Special Case (Done Button Success):**
- Before exit sequence:
  - Done button: Success ripple (0-300ms)
  - Haptic: Success pattern (light-medium-light)
- Then proceed with normal exit

**Special Case (Cancel/Backdrop Dismiss):**
- Value in field does NOT update (remains original)
- Field reset animation proceeds normally

---

### Button Press Micro-interaction

**Trigger:** User taps any button

**Number Button:**
1. Scale: 1.0 → 0.95 (compress, 50ms, LinearEasing)
2. Background: #4A4F62 → #A8E6CF (color shift, 50ms)
3. Glow: Radiates outward (blur 0dp → 12dp, 100ms)
4. Haptic: Light impact (on press start)
5. Release: Scale 0.95 → 1.0, background reverts (100ms, spring)

**Done Button:**
1. Scale: 1.0 → 0.98 (subtle compress, 50ms)
2. Glow: opacity 20% → 40% (intensify, 50ms)
3. Ripple: Emanates from tap point (#A8E6CF, expands 0dp → full width, 300ms)
4. Haptic: Medium impact (on press)
5. Success pattern: light-medium-light (100ms gaps)
6. Release: Triggers exit sequence

---

### Invalid Input Feedback

**Trigger:** User enters value < min OR > max, then taps Done

**Animation:**
1. Display value: Shake (translateX -8dp → +8dp → -4dp → 4dp → 0, 300ms)
2. Text color: #FFFFFF → #FF6B6B (error red, 100ms)
3. Glow: #A8E6CF → #FF6B6B (red glow, 100ms)
4. Done button: Disabled state (grays out, 100ms)
5. Haptic: Notification (error pattern, 3 quick pulses)

**Recovery:**
- User taps backspace/clear or enters valid digit
- Text color: #FF6B6B → #FFFFFF (200ms)
- Glow: #FF6B6B → #A8E6CF (200ms)
- Done button: Disabled → Enabled (200ms)

---

### Preset Tap Animation (Phase 2)

**Trigger:** User taps a preset button (e.g., [60] kg)

**Total Duration:** 350ms (button press + value change + state update)

**Phase 1: Button Press (0-50ms):**
1. Preset button scales: 1.0 → 0.95 (compress)
2. Background: Transparent → #2D7A5F (solid green fill)
3. Border: 1.5dp #4A4F62 → 2dp #A8E6CF (thickens + mint)
4. Haptic: Light impact (on press start)
5. Easing: LinearEasing (snappy feedback)

**Phase 2: Value Change (50-250ms):**
6. Display value updates: Slot machine animation
   - Old value (e.g., "50"): translateY 0 → -20dp, opacity 1 → 0 (150ms)
   - New value (e.g., "60"): translateY 20dp → 0, opacity 0 → 1 (200ms)
   - Scale bounce: 1.0 → 0.98 → 1.02 → 1.0 (adds weight/mass feel)
7. Display glow pulses: 30% → 50% → 30% (300ms, sine wave)
8. Haptic: Selection (on value change confirmation)

**Phase 3: Active State Transfer (150-350ms):**
9. Previous active preset (e.g., [50]) loses active state:
   - Border: 2dp #A8E6CF → 1.5dp #4A4F62 (mint → gray)
   - Background: #2D7A5F at 20% → Transparent (fade out fill)
   - Glow: 8dp #A8E6CF at 30% → 0dp (fade out)
   - ✓ icon: opacity 1 → 0, scale 1.0 → 0.8 (fade + shrink, 100ms)
10. New active preset (e.g., [60]) gains active state:
    - Border: 2dp #A8E6CF (remains mint from press)
    - Background: #2D7A5F (solid) → #2D7A5F at 20% (dissolves to subtle fill)
    - Glow: Fades in 0dp → 8dp #A8E6CF at 30% (150ms)
    - ✓ icon: Appears with scale 0.8 → 1.0, opacity 0 → 1 (150ms, spring)
11. Button scale: 0.95 → 1.0 (spring release, damping 0.7)

**Easing:**
- Press: LinearEasing (instant feedback)
- Release: Spring (damping 0.7, stiffness 350)
- Active state transition: FastOutSlowInEasing (smooth handoff)

**Edge Case (Tapping Already-Active Preset):**
- Button press animation plays normally (compress + fill)
- Display value does NOT change (no slot machine)
- Button returns to active state (no state transfer)
- Haptic: Light impact only (no Selection haptic)
- Duration: 200ms (shorter, no value change)

---

### Quick Increment Tap Animation (Phase 2)

**Trigger:** User taps a quick increment button (e.g., [+5↑])

**Total Duration:** 300ms (button press + value change)

**Phase 1: Button Press (0-100ms):**
1. Button scales: 1.0 → 0.95 (compress)
2. Background: Transparent → #A8E6CF at 20% (mint tint)
3. Border: 1.5dp → 2dp #A8E6CF (thickens)
4. Glow: Radiates 0dp → 12dp #A8E6CF at 40% (mint burst)
5. Haptic: Medium impact (stronger than number tap)
6. Easing: LinearEasing (snappy)

**Phase 2: Value Increment (50-250ms):**
7. Display value updates: Add increment to current value
   - Example: 50 + 5 = 55
   - Slot machine animation: "50" → "55" (same as manual entry)
   - Scale bounce: 1.0 → 0.98 → 1.02 → 1.0 (weight effect)
8. Display glow pulses: 30% → 50% → 30% (confirms change)
9. If preset exists for new value (e.g., [60]):
   - Previous active preset loses active state
   - Preset matching new value gains active state (✓ icon appears)
10. Haptic: Light impact (on value change confirmation)

**Phase 3: Button Release (100-300ms):**
11. Button scale: 0.95 → 1.0 (spring release, damping 0.7)
12. Background: #A8E6CF at 20% → Transparent (fade out tint)
13. Border: 2dp → 1.5dp (returns to default)
14. Glow: 12dp at 40% → 0dp (fade out burst)

**Error Case (Increment Would Exceed Max):**
- Button is disabled before tap (grayed out, see line 854-861)
- No animation plays
- No haptic feedback

**Success Feedback:**
- Total haptics: Medium (on press) + Light (on value change) = satisfying two-stage feedback
- Visual: Mint burst → Value change → Return to outlined state
- Duration: Fast (300ms) for speed-critical context

---

## 6. Haptic Feedback Map

| Action | Haptic Pattern | iOS Equivalent | Android Equivalent |
|--------|---------------|----------------|-------------------|
| **Field tap (opening)** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.KEYBOARD_TAP |
| **Field reset (closing)** | Selection | UISelectionFeedbackGenerator | HapticFeedbackConstants.CONTEXT_CLICK |
| **Number tap** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.KEYBOARD_TAP |
| **Backspace tap** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.KEYBOARD_TAP |
| **Clear tap** | Medium impact | UIImpactFeedbackGenerator.medium | HapticFeedbackConstants.LONG_PRESS |
| **Done tap (valid)** | Success pattern | UINotificationFeedbackGenerator.success | Custom: light-medium-light (100ms gaps) |
| **Done tap (invalid)** | Error notification | UINotificationFeedbackGenerator.error | HapticFeedbackConstants.REJECT |
| **Value change** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.CLOCK_TICK |
| **Invalid input** | Notification (error) | UINotificationFeedbackGenerator.error | Custom: 3 quick pulses |

**Haptic Guidelines:**
- **Never disable haptics** (core to gym experience)
- Respect system "Haptic Feedback" setting (if user disables system-wide, honor it)
- Test on real devices (simulators don't accurately represent haptic feel)

---

## 7. Edge Cases & Error Handling

### Case 1: Empty Input (User clears to "0")
- **Display shows:** "0" (not blank)
- **Done button:** Enabled if min ≤ 0, disabled if min > 0
- **Behavior:** "0" is a valid input for weight (bodyweight exercises), invalid for reps

### Case 2: Leading Zero (User types "05")
- **Auto-correct:** "05" → "5" (remove leading zero)
- **When:** On next digit entry or Done tap
- **Animation:** Value morphs smoothly (no jarring jump)

### Case 3: Decimal Without Integer (User types ".")
- **Auto-prepend:** "." → "0."
- **Display:** Shows "0." immediately
- **User continues typing:** "0.5" (natural flow)

### Case 4: Multiple Decimals Attempted
- **Prevent:** Decimal button disables after first "." entered
- **Visual:** Decimal button grays out (#3A3F52 background, #666 text)
- **Tap feedback:** No haptic, no action

### Case 5: Exceeds Max Value (User types "999" when max=500)
- **Allow typing:** Don't block input (user might be mid-entry)
- **Show error:** When done typing, display turns red (#FF6B6B)
- **Done button:** Disabled (can't confirm invalid value)
- **Recovery:** User must backspace to valid range

### Case 6: Below Min Value (User types "0" when min=1)
- **Same as Case 5:** Red display, disabled Done button
- **Common scenario:** Reps input (min=1), user accidentally clears

### Case 7: Very Long Numbers (User types "12345.67")
- **Display overflow:** Reduce font size dynamically (48sp → 36sp → 28sp)
- **Max digits:** 8 total (including decimal) before font reduction
- **Truncation:** Never truncate (always show full number)

### Case 8: Rapid Tapping (User spams "5" button)
- **Throttle:** Debounce input to 50ms (prevent accidental double-entry)
- **Animation:** Each tap queues animation (no skipping frames)
- **Haptic:** Every tap triggers haptic (satisfying feedback)

### Case 9: Sheet Opened with Invalid initialValue
- **Auto-correct:** If initialValue < min, set to min; if > max, set to max
- **Display:** Show corrected value (not invalid one)
- **Behavior:** Treat as valid starting point

### Case 10: User Swipes Down Mid-Entry
- **Prompt confirmation:** "Discard changes?" dialog
  - **IF value ≠ initialValue** → Show dialog
  - **IF value = initialValue** → Dismiss immediately (no changes)
- **Dialog options:** "Discard" / "Keep Editing"

---

## 8. Developer Implementation Notes

### Component State Management

**Local State (Internal):**
```typescript
const [currentValue, setCurrentValue] = useState<string>(initialValue.toString());
const [isValid, setIsValid] = useState<boolean>(true);
const [showDecimalButton, setShowDecimalButton] = useState<boolean>(allowDecimals);
```

**Validation Logic:**
```typescript
function validateValue(value: string): boolean {
  const num = parseFloat(value);
  if (isNaN(num)) return false;
  if (min !== undefined && num < min) return false;
  if (max !== undefined && num > max) return false;
  return true;
}

useEffect(() => {
  setIsValid(validateValue(currentValue));
}, [currentValue]);
```

**Number Formatting Logic:**
```typescript
function formatDisplayValue(value: number): string {
  // Convert to string, strip trailing zeros
  if (Number.isInteger(value)) {
    return value.toString(); // "52" not "52.0"
  }

  // Has decimals: strip trailing zeros
  let formatted = value.toFixed(10); // Max precision
  formatted = formatted.replace(/\.?0+$/, ''); // Strip trailing zeros

  return formatted;
  // Examples:
  // 52.0 → "52"
  // 52.5 → "52.5"
  // 52.50 → "52.5"
}
```

**Digit Entry Logic (Replace Mode):**
```typescript
const [isEditMode, setIsEditMode] = useState(false);

function handleDigitPress(digit: string) {
  let newValue: string;

  // Replace Mode: First digit tap clears existing value
  if (!isEditMode) {
    newValue = digit;
    setIsEditMode(true); // Now in edit mode for subsequent digits
  } else {
    // Edit/Append Mode: Add digit to existing value
    newValue = currentValue === '0' ? digit : currentValue + digit;
  }

  // Auto-correct leading zeros
  if (newValue.startsWith('0') && !newValue.startsWith('0.')) {
    newValue = newValue.substring(1);
  }

  setCurrentValue(newValue);
  onChange?.(parseFloat(newValue));
  triggerHaptic('light');
}

function handleBackspacePress() {
  // Backspace activates edit mode (prevents replace on next digit)
  setIsEditMode(true);

  const newValue = currentValue.slice(0, -1) || '0';
  setCurrentValue(newValue);
  onChange?.(parseFloat(newValue));
  triggerHaptic('light');
}

// Reset edit mode when sheet opens
function handleSheetOpen() {
  setIsEditMode(false); // First digit will replace
  // ... rest of open logic
}
```

**Decimal Logic:**
```typescript
function handleDecimalPress() {
  if (!allowDecimals) return;
  if (currentValue.includes('.')) return; // Already has decimal

  const newValue = currentValue + '.';
  setCurrentValue(newValue);
  setShowDecimalButton(false); // Disable until cleared
  triggerHaptic('light');
}
```

---

### Animation Implementation (React Native / Kotlin)

**React Native (Reanimated 2):**
```typescript
import { useSharedValue, useAnimatedStyle, withSpring, withTiming } from 'react-native-reanimated';

const scale = useSharedValue(1);

const buttonStyle = useAnimatedStyle(() => ({
  transform: [{ scale: scale.value }]
}));

function handlePress() {
  scale.value = withTiming(0.95, { duration: 50 }, () => {
    scale.value = withSpring(1.0, { damping: 15, stiffness: 300 });
  });
}
```

**Kotlin (Jetpack Compose):**
```kotlin
val scale by animateFloatAsState(
    targetValue = if (isPressed) 0.95f else 1.0f,
    animationSpec = if (isPressed) {
        tween(durationMillis = 50)
    } else {
        spring(dampingRatio = 0.6f, stiffness = 300f)
    }
)

Box(modifier = Modifier.scale(scale)) {
    // Button content
}
```

---

### Platform-Specific Considerations

**iOS:**
- Use `UIImpactFeedbackGenerator` (pre-warm on sheet open)
- Respect "Reduce Motion" accessibility setting
- Handle safe area insets properly (home indicator)
- Support VoiceOver (all elements must have labels)

**Android:**
- Use `HapticFeedbackConstants` (Material 3 compliant)
- Respect "Remove animations" accessibility setting
- Handle navigation bar insets (3-button vs gesture navigation)
- Support TalkBack (content descriptions on all buttons)

---

## 9. Testing Checklist

### Field (Closed State) Tests
- [ ] Field displays correct initial value with unit (e.g., "52.5 kg")
- [ ] Label displays correctly above field
- [ ] Required indicator (*) shows when required={true}
- [ ] Placeholder shows when value is empty
- [ ] Tap field → Opens bottom sheet numpad
- [ ] Field shows focused state when sheet opens (mint border + glow)
- [ ] Chevron rotates 180° when sheet opens
- [ ] Label color changes to mint when focused
- [ ] Error message displays below field when error prop provided
- [ ] Error state: Red border + glow + shake animation
- [ ] Disabled state: Grayed out, no interaction
- [ ] Field resets to default state after sheet closes
- [ ] Value updates in field after Done is tapped
- [ ] Value remains unchanged if Cancel/Backdrop tapped

### Bottom Sheet Numpad Tests
- [ ] Enter valid integer (e.g., "50")
- [ ] Enter valid decimal (e.g., "52.5")
- [ ] Enter value at min boundary (e.g., "0" when min=0)
- [ ] Enter value at max boundary (e.g., "500" when max=500)
- [ ] Enter value below min → Verify error state
- [ ] Enter value above max → Verify error state
- [ ] Tap backspace to delete digits
- [ ] Long-press backspace to clear all
- [ ] Tap clear button → Resets to "0"
- [ ] Tap Done with valid value → Calls onConfirm, updates field
- [ ] Tap Done with invalid value → Disabled (no action)
- [ ] Tap Cancel → Calls onCancel, dismisses sheet, field unchanged
- [ ] Swipe down → Dismisses sheet, field unchanged
- [ ] Tap backdrop → Dismisses sheet, field unchanged

### Edge Case Tests
- [ ] Initial value = 0 → Displays correctly
- [ ] allowDecimals=false → Decimal button hidden
- [ ] allowDecimals=true → Decimal button shows, disables after use
- [ ] Rapid tap same digit (spam "5") → No double-entry
- [ ] Type leading zero ("05") → Auto-corrects to "5"
- [ ] Type decimal first (".") → Auto-prepends "0."
- [ ] Exceed max digits → Font size reduces
- [ ] Sheet opened mid-app (e.g., from Active Workout) → Backdrop doesn't block other UI

### Visual Tests
- [ ] Dark mode (field): All colors match branding.md
- [ ] Dark mode (sheet): All colors match branding.md
- [ ] Light mode (field): All colors match branding.md
- [ ] Light mode (sheet): All colors match branding.md
- [ ] Field glow renders correctly (blur radius 8dp, focused/error)
- [ ] Sheet display glow renders correctly (blur radius 16dp)
- [ ] Button pressed glow radiates outward
- [ ] Invalid state: Red glow + shake animation
- [ ] Done button glow (green halo)
- [ ] Field ripple effect on tap (mint, 10% opacity)
- [ ] All text WCAG AA compliant (contrast ≥ 4.5:1)

### Animation Tests
- [ ] Field tap → Sheet open: Coordinated animation (650ms total)
- [ ] Chevron rotates smoothly (0° → 180° on open, reverse on close)
- [ ] Field border animates (1dp → 2dp on focus)
- [ ] Label color animates (mint on focus, red on error)
- [ ] Sheet entry animation: Smooth spring bounce
- [ ] Sheet exit animation: Accelerates downward
- [ ] Value change: Slot machine effect
- [ ] Button press: Scale 0.95 → 1.0 spring
- [ ] Invalid input: Shake (translateX wiggle)
- [ ] Done success: Ripple emanates from tap point
- [ ] Field value updates after Done (instant, no animation in field)
- [ ] Reduce Motion enabled: All animations become fades

### Haptic Tests
- [ ] Field tap: Light impact
- [ ] Number tap (in sheet): Light impact
- [ ] Backspace tap: Light impact
- [ ] Clear tap: Medium impact
- [ ] Done tap (valid): Success pattern
- [ ] Invalid input: Error notification (3 pulses)
- [ ] Haptics disabled system-wide: No haptics fire

### Accessibility Tests
- [ ] VoiceOver/TalkBack (field): Announces label, value, unit, required, error
- [ ] VoiceOver/TalkBack (sheet): All elements announced correctly
- [ ] Keyboard navigation: Tab to field → Enter to open → Tab through sheet → Enter to confirm
- [ ] Focus indicators: Visible on field and all sheet elements
- [ ] High contrast mode (field): 3dp borders, high contrast colors
- [ ] High contrast mode (sheet): Borders appear, text contrast boosted
- [ ] Dynamic Type (field): Label and value scale, field remains 56dp
- [ ] Dynamic Type (sheet): Text scales, buttons remain 56dp
- [ ] Reduce Motion: Field and sheet animations become fades

---

## 10. Future Enhancements (Phase 2+)

### Phase 2 Features (Active Workout Integration) — ✅ COMPLETE
**Status**: Quick increments + presets fully specified with Hybrid layout design.

**Design Completed (v1.5)**:
- [x] **Layout Decision:** Hybrid approach — Display Hero + Compact Presets + Grid with Quick Increments (right column)
- [x] **Preset Buttons Specification:** Outlined pill style, 36dp height, active state with ✓ icon and mint glow (see line 728-806)
- [x] **Quick Increment Buttons Specification:** Outlined mint style with ↑ icon, 56dp size, right column positioning (see line 809-900)
- [x] **Visual Hierarchy:** Clear differentiation: Display (48sp glowing) ≠ Presets (outlined pills) ≠ Grid (filled buttons) ≠ Increments (outlined mint)
- [x] **Configuration Props:** `presets={[]}` and `quickIncrements={[]}` arrays for customization per use case
- [x] **Animation Specs:** Preset tap animation, increment tap animation, active state transitions
- [x] **Styler Approved:** 9.5/10 energy score, ready for implementation

**Implementation Notes:**
- **Presets:** Configurable per context (e.g., [20, 40, 60, 80, 100] for weight, [5, 8, 10, 12] for reps)
- **Quick Increments:** Configurable per context (e.g., [2.5, 5, 10] for kg, [1, 2, 5] for reps)
- **Active State Logic:** Preset with ✓ icon shows which value matches current display
- **Disabled State Logic:** Quick increment disables if result would exceed max value

**Phase 2.5 Creative Boosts (Optional Polish)**:
- [ ] **Swipe Gesture on Presets:** Swipe left/right to cycle through common values
- [ ] **Long-Press Preset:** Auto-fills and closes sheet (1-tap workflow for power users)
- [ ] **Preset Entry Animation:** Slide in from left with 50ms stagger on sheet open

**Postponed (No AI Backend Yet)**:
- [ ] **AI-Recommended Presets:** Breathing mint glow on AI-suggested values — **POSTPONED** (requires AI backend)
- [ ] **Context Hints:** "Last session: 50kg" below display — **POSTPONED** (requires history tracking)
- [ ] **Smart Preset Learning:** Auto-populate presets based on user history — **POSTPONED** (requires AI backend)

---

### Phase 3 Features (AI Integration) — ⏸️ POSTPONED (No AI Backend)
**Status**: All AI-powered features postponed until AI recommendation engine is built.

- [ ] **Progressive Overload Hint:** "AI suggests +2.5kg" (above display) — **POSTPONED**
- [ ] **Tap to Accept:** Tap suggestion → Auto-fills value — **POSTPONED**
- [ ] **Smart Presets:** Learn from user history (most-used weights) — **POSTPONED**
- [ ] **Error Prevention:** "That's heavy! Sure?" confirmation for outliers — **POSTPONED**
- [ ] **AI Badge:** Tiny "AI" chip on recommended preset button — **POSTPONED**
- [ ] **Contextual Hint Component:** "Last session: 50kg" or "AI suggests: 60kg" with lightbulb icon — **POSTPONED**
- [ ] **Breathing Animation on AI Preset:** Glowing mint border pulses 2s loop — **POSTPONED**

**Rationale**: AI recommendation engine not yet built. These features require:
- User workout history tracking
- AI analysis of progression patterns
- Progressive overload algorithms
- Session-to-session comparison logic

**When to Implement**: After AI recommendation backend is available (Phase 3+).

### Polish Ideas (Post-MVP)
- [ ] **Sound Effects:** Subtle "click" on number tap (optional, off by default)
- [ ] **Confetti:** When user logs PR weight (Phase 3, Active Workout)
- [ ] **History Scrubber:** Swipe left/right on display to cycle through recent values
- [ ] **Calculator Mode:** Long-press display to enable +/- operations (niche use case)

---

## 11. Design Decisions Log

### 2025-01-XX — Component Created (Phase 1 MVP)

**Decision 1: Dark Mode First**
- **Why:** Brand DNA states "Dark Mode is the Hero experience"
- **Impact:** Designed all specs for #2D3142 background first, light mode second

**Decision 2: Large Touch Targets (56dp)**
- **Why:** Gym environment = sweaty fingers, gloves, fatigue
- **Impact:** Buttons 56dp × 56dp (vs standard 48dp)

**Decision 3: Custom Animations (Not Native)**
- **Why:** Native keyboard = no haptics, no glow, no delight
- **Impact:** Built slot machine value animation, button glows, success ripples

**Decision 4: Decimal Button Conditional**
- **Why:** Reps don't need decimals, weight does
- **Impact:** `allowDecimals` prop hides/shows decimal button

**Decision 5: No Quick Increments (Phase 1)**
- **Why:** Phase 1 = core numpad only, Phase 2 = speed features
- **Impact:** Simpler MVP, ship faster, validate core UX first

**Decision 6: Glow Effects (Brand Signature)**
- **Why:** Styler demanded "premium feel", glows create depth + luxury
- **Impact:** Display glow (#A8E6CF), button press glow, Done button halo

**Decision 7: Success Pattern Haptic (Done Button)**
- **Why:** Completing input is a micro-achievement (celebrate it)
- **Impact:** light-medium-light pattern (vs single tap)

**Decision 8: Invalid Input = Red Glow + Shake**
- **Why:** Clear, immediate feedback (no error dialog needed)
- **Impact:** User instantly knows value is wrong, can self-correct

**Decision 9: All-in-One Component (Field + Sheet)**
- **Why:** Ensures consistent UX across all numeric inputs; prevents each screen from designing its own field
- **Impact:** Component owns both closed state (field) and open state (sheet), eliminating design drift
- **Alternative Rejected:** Separate field + sheet components = inconsistent implementations

**Decision 10: Controlled Component (value prop, not initialValue)**
- **Why:** React best practice; parent controls state, component is stateless UI
- **Impact:** Parent must manage value state; enables validation at form level
- **Trade-off:** Slightly more code for parent, but cleaner architecture

**Decision 11: Chevron Rotates on Open**
- **Why:** Clear visual feedback that field is "active"; mirrors native dropdown behavior
- **Impact:** 180° rotation (0° → 180° → 0°); helps user understand state
- **Alternative Rejected:** Static chevron = less clear affordance

**Decision 12: Field Glow Subtler Than Sheet Glow**
- **Why:** Field is persistent (always visible); sheet is modal (temporary focus)
- **Impact:** Field glow = 8dp blur at 20%, Sheet display glow = 16dp blur at 30%
- **Principle:** Persistent elements should be calmer than modal elements

---

### 2025-01-XX — Styler Review & Premium Polish Pass (v1.2)

**Decision 13: Double-Layer Glow System (Premium Signature)**
- **Why:** Styler critique revealed single-layer glow felt flat; double-layer creates depth
- **Impact:** Field focused = Inner (6dp #A8E6CF at 40%) + Outer (16dp #2D7A5F at 15%)
- **Breathing Animation:** Inner glow pulses 40% → 45% → 40% (2s sine wave loop)
- **Result:** "Energy field" effect that screams premium, aligns with "Soft Tech" philosophy

**Decision 14: Chevron Spring Overshoot (Tactile Personality)**
- **Why:** Standard 180° rotation felt robotic and lifeless
- **Impact:** 0° → 190° → 180° overshoot with spring physics (damping 0.7)
- **Plus:** Scale 1.0 → 1.1 → 1.0 + color shift #B8BCC8 → #FFFFFF
- **Result:** Component feels responsive and alive, not just functional

**Decision 15: Button Inner Shadow (3D Depth Illusion)**
- **Why:** Flat button press lacked physicality; needed "sinking into surface" feel
- **Impact:** Added inset 0dp 2dp 4dp rgba(0,0,0,0.2) on press state
- **Result:** Buttons feel like physical keys, not just colored rectangles

**Decision 16: Counter "Weight" Animation (Mass & Velocity)**
- **Why:** Slot machine effect was smooth but lacked physicality
- **Impact:** Added scale bounce (1.0 → 0.98 → 1.02 → 1.0) + motion blur (0 → 2dp → 0)
- **Result:** Numbers feel like they have mass, "land" with weight

**Decision 17: Symmetrical Haptic Feedback (Closure Signal)**
- **Why:** Asymmetric haptics broke user expectations (open had haptic, close didn't)
- **Impact:** Added "Selection" haptic when field returns to idle after sheet closes
- **Result:** Complete feedback loop, satisfying closure

**Decision 18: White Label on Focus (Accessibility First)**
- **Why:** #A8E6CF on #2D3142 = 2.8:1 contrast (WCAG fail)
- **Impact:** Changed focused label to #FFFFFF (15.3:1 contrast) + subtle #A8E6CF glow
- **Trade-off:** Lost pure mint label, but gained WCAG AA compliance + premium glow hint
- **Principle:** Accessibility is non-negotiable, but we can still be beautiful

**Decision 19: Unified Field Background (#2D3142)**
- **Why:** #3A3F52 violated branding.md (undefined token)
- **Impact:** Changed field background to #2D3142 (DarkNavy) to match sheet container
- **Result:** Visual consistency, single source of truth, cleaner design system

---

### 2025-01-XX — Client UX Refinement (Replace Mode + Number Formatting)

**Decision 20: Replace Mode (Not Append Mode)**
- **Why:** Gym context = speed-critical; complete value changes (50kg → 60kg) are more common than small edits (52.5 → 52.0)
- **Impact:** First number tap clears existing value, starts fresh (calculator behavior)
- **Escape Hatch:** Backspace-first activates edit mode for small refinements
- **Rationale:** "Type 6-0-Done" is faster than "Backspace 4× then type 6-0-Done"
- **Client Input:** User (Sebastian) proposed this after analyzing real workout flow

**Decision 21: Hide Trailing Zeros (52 not 52.0)**
- **Why:** Trailing zeros add visual clutter without semantic value (52kg = 52.0kg in fitness context)
- **Impact:** Display "52" (not "52.0"), "52.5" (not "52.50"), matches human speech ("52 kilos")
- **Consistency:** Standard behavior across fitness apps (Strong, MyFitnessPal), calculators, Excel
- **Rule:** Show decimals only when meaningful (52.5 = yes, 52.0 = no)
- **Client Input:** User (Sebastian) confirmed Option A aligns with user mental model

---

### 2025-01-XX — Phase 2 Layout Design (Hybrid Architecture)

**Decision 22: Hybrid Layout (Display Hero + Compact Presets)**
- **Why:** Balances speed (presets) with context (display) for Active Workout use case
- **Alternatives Considered:**
  - **Option A (Presets First):** Fastest for preset users (2 taps), but buries display (loses context)
  - **Option B (Display First, Presets Below):** Context-first, but presets require visual scan
  - **Hybrid (Chosen):** Display HERO (48sp, glowing) → Compact presets (1 line, outlined) → Grid
- **Impact:** Clear visual hierarchy: Display ≠ Presets ≠ Grid ≠ Quick Increments
- **Rationale:**
  - **Context first:** User sees current value immediately (5 reps)
  - **Speed second:** Presets right below (tap 8 → Done, 2 taps total)
  - **Manual third:** Grid for custom values (natural flow)
  - **Micro-adjustments:** Quick increments in right column (speed feature)
- **User Flow Analysis:**
  - Preset scenario (5→8): 2 taps, ~900ms (presets visible, no scroll)
  - Custom scenario (5→7): 2 taps, ~1100ms (grid not blocked by presets)
  - Increment scenario (5→6): 2 taps, ~900ms (+1 in right column)
- **Styler Verdict:** 9.5/10 energy score, premium + fast + clear
- **Result:** Best of all three options, approved for implementation

**Decision 23: Preset Buttons as Outlined Pills (Not Filled)**
- **Why:** Visual differentiation from grid buttons (filled #4A4F62)
- **Impact:**
  - Style: Outlined pills (18dp radius), not filled rectangles
  - Border: 1.5dp #4A4F62 (neutral gray), active = 2dp #A8E6CF (mint)
  - Height: 36dp (compact), not 56dp (clearly secondary to display)
  - Active indicator: ✓ icon + mint border + subtle glow
- **Rationale:** User should instantly distinguish presets from manual grid
- **Inspiration:** iOS segmented control, Material 3 filter chips
- **Result:** Presets feel like "quick picks", not full buttons

**Decision 24: Quick Increments as Outlined Mint (Right Column)**
- **Why:** Third style tier (Display → Presets → Grid → Increments)
- **Impact:**
  - Style: Outlined (not filled), mint accent (#A8E6CF border)
  - Icon: ↑ arrow above text (instant recognition)
  - Position: Right column of grid (rows 1-3), aligned with number buttons
  - Size: 56dp (same as grid for muscle memory)
- **Rationale:**
  - Outlined = speed feature (not primary input method)
  - Mint color = branded accent (stands out from neutral gray)
  - Right column = ergonomic (thumb reaches naturally on mobile)
- **Alternative Rejected:** Filled mint buttons felt too aggressive (competed with Display)
- **Result:** Increments are discoverable but not distracting

**Decision 25: Active Preset with ✓ Icon (Not Just Border)**
- **Why:** Border-only active state too subtle (user might not notice which preset is current)
- **Impact:**
  - Active preset: ✓ icon (12dp, #A8E6CF) before value
  - Plus: Mint border (2dp) + subtle glow (8dp at 30%) + background tint (#2D7A5F at 20%)
  - Animation: ✓ scales 0.8→1.0, opacity 0→1 (150ms spring) when becoming active
- **Rationale:**
  - ✓ = universal "current selection" symbol
  - Glow = premium signature (matches display, brand consistency)
  - Animation = satisfying state transfer (user sees which preset "won")
- **Accessibility:** ✓ announced by screen reader ("Selected, 60 kilograms")
- **Result:** Zero ambiguity about current value

---

## 12. Revision History

- **2025-01-XX (v1.5):** Phase 2 Complete — Hybrid Layout + Full Specifications
  - **Context:** Reviewed first implementation screenshot showing Phase 2 features (presets + quick increments) without proper design
  - **Layout Decision Finalized:**
    - **Hybrid Architecture:** Display Hero (48sp, glowing) → Compact Presets (outlined pills) → Number Grid → Quick Increments (right column)
    - Analyzed 3 layout options (Presets First, Display First, Hybrid) with user flow analysis
    - Chose Hybrid for best balance: Context + Speed + Clear hierarchy
  - **New Specifications Added:**
    - **Preset Buttons (lines 728-806):** Outlined pill style, 36dp height, active state with ✓ icon and mint glow
    - **Quick Increment Buttons (lines 809-900):** Outlined mint style with ↑ icon, 56dp size, right column positioning
    - **Visual Preview Updated (line 50-74):** Shows Phase 2 layout with presets and increments
  - **Animation Choreography:**
    - Preset tap animation (350ms, 3 phases: press → value change → active state transfer)
    - Quick increment tap animation (300ms, mint burst → value increment → release)
    - Active state transfer animation (✓ icon scales in, glow fades, border color animates)
  - **Design Decisions:**
    - Decision 22: Hybrid Layout (Display Hero + Compact Presets)
    - Decision 23: Preset Buttons as Outlined Pills (Not Filled)
    - Decision 24: Quick Increments as Outlined Mint (Right Column)
    - Decision 25: Active Preset with ✓ Icon (Not Just Border)
  - **Configuration Props:**
    - `presets={[20, 40, 60, 80, 100]}` — Configurable per context (weight/reps/rest)
    - `quickIncrements={[2.5, 5, 10]}` — Configurable per context
  - **Styler Review:** 9.5/10 energy score, approved with premium polish notes
  - **Status:** ✅ **Phase 2 Design Complete — Ready for Implementation**

- **2025-01-XX (v1.4):** Implementation Review — Critical Fixes + AI Postponement
  - **Dev Team Submission**: First implementation review from screenshot
  - **Critical Issues Identified**:
    - Display shows "52.0" instead of "52" (trailing zeros violation)
    - Done button wrong color (#A8E6CF instead of #2D7A5F)
    - Missing display glow (premium signature)
    - Missing Done button glow (premium signature)
    - Display text too small (36-40sp instead of 48sp)
    - Missing button elevation shadows
  - **Phase 2 Features**:
    - Quick increments (+2.5, +5, +10) added by dev team ✅
    - Presets row (20, 40, 60, 80, 100) added by dev team ✅
    - Need visual hierarchy design (outlined style, icons, colors)
  - **AI Features Postponed**:
    - All AI-powered features (recommendations, history, smart presets) postponed
    - Rationale: AI backend not yet built
    - Status: Phase 3+ (when AI recommendation engine available)
  - **Styler Verdict**: ❌ BLOCKED — Fix critical issues, then re-submit
  - **Client Decision**: Mark all AI features as POSTPONED for now

- **2025-01-XX (v1.3):** Client UX Refinement — Replace Mode + Number Formatting
  - **Replace Mode Behavior**:
    - First number tap → Clears existing value, starts fresh (calculator UX)
    - Backspace-first → Activates edit mode for small refinements
    - Rationale: Speed-critical gym context favors complete value changes
  - **Number Formatting Rules**:
    - Hide trailing zeros: 52.0 → "52", 52.50 → "52.5"
    - Show meaningful decimals: 52.5 → "52.5"
    - Matches user mental model ("52 kilos" not "52.0 kilos")
    - Standard behavior across fitness apps, calculators
  - **Implementation**:
    - Added `isEditMode` state to track replace vs. edit mode
    - Added `formatDisplayValue()` function for consistent formatting
    - Updated Developer Implementation Notes with code examples
  - **Client Input**: Sebastian proposed both refinements after workflow analysis
  - Status: ✅ **Approved — Ready for Implementation**

- **2025-01-XX (v1.2):** Styler Review — Premium Polish Pass
  - **Critical Fixes**:
    - Fixed color token violation (#3A3F52 → #2D3142)
    - Fixed accessibility: Focused label #A8E6CF → #FFFFFF (WCAG AA compliant)
    - Added missing "Selection" haptic on field reset to idle
  - **Premium Enhancements**:
    - Double-layer glow system (inner + outer) with breathing animation
    - Chevron spring overshoot (0° → 190° → 180°) + scale + color shift
    - Button inner shadow for 3D depth illusion
    - Counter scale bounce + motion blur for "weightful" feel
  - **Result**: Component elevated from 7.5/10 to 9.5/10 (Styler verdict)
  - Status: ✅ **Approved by Styler — Ready for Legendary Implementation**

- **2025-01-XX (v1.1):** Expanded to include Input Field (Closed State)
  - Added complete field specification (Part A of design spec)
  - Field states: Default, Focused, Error, Disabled
  - Label, value display, unit, chevron, error message
  - Field-to-sheet coordination animations
  - Updated props interface: `value` (controlled), `label` (required), `error`, `disabled`, `required`
  - Updated testing checklist with field tests
  - Component now "all-in-one" (field + sheet)

- **2025-01-XX (v1.0):** Initial specification — Phase 1 MVP
  - Core numpad (0-9, decimal, backspace, clear, done)
  - Bottom sheet numpad (Part B of design spec)
  - Dark + Light mode full specs
  - Animation choreography
  - Haptic map
  - Accessibility compliance

---

**End of Component Specification**

**Next Steps for Designer:**
1. ✅ **DONE:** Expanded spec to include input field (closed state)
2. Integrate this component into Target Config (5.04) spec (replace generic input fields)
3. Update Active Workout (2.02) spec for Phase 2 (presets + increments)
4. Create usage examples for Plan Builder

**Next Steps for Developer:**
1. Build component (estimate: 3-4 days — field + sheet + premium animations)
2. Implement state management (controlled component pattern)
3. Implement double-layer glow system (critical for premium feel)
4. Test on physical devices (haptics + spring physics + glows + motion blur)
5. Integrate into Target Config (5.04) — replace weight/reps inputs
6. Reuse across app (Profile Settings, Plan Builder, etc.)

---

**Status:** ✅ **COMPLETE v1.2 — Styler Approved — Premium Grade — Ready for Legendary Implementation**
