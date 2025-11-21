# Gym Numpad Component

**Version:** 1.0 (Phase 1 MVP)
**Status:** Ready for Development
**Design State:** Finished
**Component Type:** Reusable Bottom Sheet Input

---

## 1. Component Overview

### Purpose
A **gym-optimized numeric input component** designed for speed, accuracy, and tactile satisfaction in workout environments. Replaces native keyboards with a custom interface optimized for:
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

```
┌─────────────────────────────────────────┐
│  Weight (kg)                      [✓]  │ ← Header
├─────────────────────────────────────────┤
│                                         │
│          ┌─────────────┐               │
│          │    52.5     │               │ ← Display (glowing)
│          └─────────────┘               │
│                                         │
├─────────────────────────────────────────┤
│                                         │
│    [  1  ]  [  2  ]  [  3  ]  [ ← ]    │ ← Number grid
│    [  4  ]  [  5  ]  [  6  ]  [ C ]    │
│    [  7  ]  [  8  ]  [  9  ]            │
│    [  .  ]  [  0  ]  [  ⌫  ]           │ ← Decimal, Zero, Backspace
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
  initialValue: number;           // Starting value (e.g., 50.0)
  unit: 'kg' | 'lbs' | 'reps' | 'seconds' | null;  // Display unit

  // === Constraints ===
  min?: number;                   // Minimum value (e.g., 0 for weight, 1 for reps)
  max?: number;                   // Maximum value (e.g., 500 for weight)
  allowDecimals: boolean;         // true for weight, false for reps
  step?: number;                  // Increment step (e.g., 0.5 for kg, 1 for reps)

  // === Callbacks ===
  onConfirm: (value: number) => void;   // User taps Done
  onCancel?: () => void;                // User taps X or swipes down
  onChange?: (value: number) => void;   // Live value updates (optional)

  // === Behavior ===
  autoDismiss?: boolean;          // Auto-close on Done (default: true)
  showCancelButton?: boolean;     // Show X in header (default: true)

  // === Styling ===
  theme?: 'dark' | 'light' | 'auto';  // Color scheme (default: 'auto' follows system)
  label?: string;                 // Header label (e.g., "Weight (kg)")
}
```

### Example Usage

```typescript
// Weight input (allows decimals)
<GymNumpad
  initialValue={50.0}
  unit="kg"
  min={0}
  max={500}
  allowDecimals={true}
  step={0.5}
  label="Weight (kg)"
  onConfirm={(value) => saveWeight(value)}
  theme="dark"
/>

// Reps input (no decimals)
<GymNumpad
  initialValue={10}
  unit="reps"
  min={1}
  max={100}
  allowDecimals={false}
  label="Reps"
  onConfirm={(value) => saveReps(value)}
/>

// Rest timer (seconds)
<GymNumpad
  initialValue={90}
  unit="seconds"
  min={0}
  max={600}
  allowDecimals={false}
  label="Rest Time"
  onConfirm={(value) => setRestTimer(value)}
/>
```

---

## 4. Detailed Design Specification

### <gym_numpad_dark_mode>

**Design Version**: v1.0 (2025-01-XX) — Phase 1 MVP

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

**Animation on Value Change:**
- **Counter Animation** (slot machine effect):
  - Old value: opacity 1 → 0, translateY 0 → -20dp (slides up, fades out)
  - New value: opacity 0 → 1, translateY 20dp → 0 (slides up from below, fades in)
  - Duration: 200ms
  - Easing: FastOutSlowInEasing
  - Stagger: Digit-by-digit if multiple digits change
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
- Shadow: Elevation 2dp (subtle depth)

*Pressed State:*
- Background: #A8E6CF (Brand Mint — active glow)
- Scale: 0.95 (compress slightly)
- Shadow: Elevation 0dp (pressed flat)
- Text color: #1A1A1A (dark text on mint background)
- Duration: 100ms
- Easing: Linear
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

### </gym_numpad_dark_mode>

---

### <gym_numpad_light_mode>

**Context:** Light mode variant for planning screens in daylight environments (e.g., reviewing plan at home).

**Design Version**: v1.0 (2025-01-XX)

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

### </gym_numpad_light_mode>

---

## 5. Animation Choreography

### Entry Sequence (Bottom Sheet Opens)

**Total Duration:** 350ms

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

**Total Duration:** 300ms

**Phase 1: Content Fade (0-150ms):**
- All content: opacity 1 → 0
- Easing: LinearEasing

**Phase 2: Sheet Slide (0-300ms):**
- Sheet: translateY(0) → translateY(100%)
- Easing: FastOutLinearInEasing (accelerates downward)

**Phase 3: Backdrop (100-300ms):**
- Overlay: opacity 0.6 → 0
- Easing: LinearEasing

**Special Case (Done Button Success):**
- Before exit sequence:
  - Done button: Success ripple (0-300ms)
  - Haptic: Success pattern (light-medium-light)
- Then proceed with normal exit

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

## 6. Haptic Feedback Map

| Action | Haptic Pattern | iOS Equivalent | Android Equivalent |
|--------|---------------|----------------|-------------------|
| **Number tap** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.KEYBOARD_TAP |
| **Backspace tap** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.KEYBOARD_TAP |
| **Clear tap** | Medium impact | UIImpactFeedbackGenerator.medium | HapticFeedbackConstants.LONG_PRESS |
| **Done tap (valid)** | Success pattern | UINotificationFeedbackGenerator.success | Custom: light-medium-light (100ms gaps) |
| **Done tap (invalid)** | Error notification | UINotificationFeedbackGenerator.error | HapticFeedbackConstants.REJECT |
| **Value change** | Light impact | UIImpactFeedbackGenerator.light | HapticFeedbackConstants.CLOCK_TICK |
| **Sheet open** | None | - | - |
| **Sheet close** | None | - | - |
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

**Digit Entry Logic:**
```typescript
function handleDigitPress(digit: string) {
  let newValue = currentValue === '0' ? digit : currentValue + digit;

  // Auto-correct leading zeros
  if (newValue.startsWith('0') && !newValue.startsWith('0.')) {
    newValue = newValue.substring(1);
  }

  setCurrentValue(newValue);
  onChange?.(parseFloat(newValue));
  triggerHaptic('light');
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

### Functional Tests
- [ ] Enter valid integer (e.g., "50")
- [ ] Enter valid decimal (e.g., "52.5")
- [ ] Enter value at min boundary (e.g., "0" when min=0)
- [ ] Enter value at max boundary (e.g., "500" when max=500)
- [ ] Enter value below min → Verify error state
- [ ] Enter value above max → Verify error state
- [ ] Tap backspace to delete digits
- [ ] Long-press backspace to clear all
- [ ] Tap clear button → Resets to "0"
- [ ] Tap Done with valid value → Calls onConfirm
- [ ] Tap Done with invalid value → Disabled (no action)
- [ ] Tap Cancel → Calls onCancel, dismisses sheet
- [ ] Swipe down → Dismisses sheet
- [ ] Tap backdrop → Dismisses sheet

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
- [ ] Dark mode: All colors match branding.md
- [ ] Light mode: All colors match branding.md
- [ ] Display glow renders correctly (blur radius 16dp)
- [ ] Button pressed glow radiates outward
- [ ] Invalid state: Red glow + shake animation
- [ ] Done button glow (green halo)
- [ ] All text WCAG AA compliant (contrast ≥ 4.5:1)

### Animation Tests
- [ ] Entry animation: Smooth spring bounce
- [ ] Exit animation: Accelerates downward
- [ ] Value change: Slot machine effect
- [ ] Button press: Scale 0.95 → 1.0 spring
- [ ] Invalid input: Shake (translateX wiggle)
- [ ] Done success: Ripple emanates from tap point
- [ ] Reduce Motion enabled: All animations become fades

### Haptic Tests
- [ ] Number tap: Light impact
- [ ] Backspace tap: Light impact
- [ ] Clear tap: Medium impact
- [ ] Done tap (valid): Success pattern
- [ ] Invalid input: Error notification (3 pulses)
- [ ] Haptics disabled system-wide: No haptics fire

### Accessibility Tests
- [ ] VoiceOver/TalkBack: All elements announced correctly
- [ ] Keyboard navigation: Tab order logical
- [ ] Focus indicators: Visible on all interactive elements
- [ ] High contrast mode: Borders appear, text contrast boosted
- [ ] Dynamic Type: Text scales, buttons remain 56dp
- [ ] Reduce Motion: Animations disabled

---

## 10. Future Enhancements (Phase 2+)

### Phase 2 Features (Active Workout Integration)
- [ ] **Quick Increments:** +2.5kg, +5kg, +10kg buttons (right column)
- [ ] **Context Hints:** "Last session: 50kg" (below display)
- [ ] **Presets Row:** [20] [40] [60] [80] [100] (common weights, below grid)
- [ ] **Auto-Suggest:** Highlight AI recommendation (e.g., "52.5" glows)

### Phase 3 Features (AI Integration)
- [ ] **Progressive Overload Hint:** "AI suggests +2.5kg" (above display)
- [ ] **Tap to Accept:** Tap suggestion → Auto-fills value
- [ ] **Smart Presets:** Learn from user history (most-used weights)
- [ ] **Error Prevention:** "That's heavy! Sure?" confirmation for outliers

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

---

## 12. Revision History

- **2025-01-XX (v1.0):** Initial specification — Phase 1 MVP
  - Core numpad (0-9, decimal, backspace, clear, done)
  - Dark + Light mode full specs
  - Animation choreography
  - Haptic map
  - Accessibility compliance
  - Ready for development

---

**End of Component Specification**

**Next Steps for Designer:**
1. Integrate this component into Target Config (5.04) spec
2. Update Active Workout (2.02) spec for Phase 2 (future)
3. Create usage examples for Plan Builder

**Next Steps for Developer:**
1. Build component (estimate: 2-3 days)
2. Test on physical devices (haptics + animations)
3. Integrate into Target Config (5.04)
4. Reuse in all numeric input scenarios

---

**Status:** ✅ **COMPLETE — Ready for Development**
