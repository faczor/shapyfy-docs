---
tags:
  - mobile
  - component
screen: Top Bar
agent: "[[designer]]"
feature: "[[features/00-app-overview]]"
design_state: "[[Statuses/Finished]]"
---
# Top Bar Component

## Overview

### Purpose
The Top Bar is a reusable navigation and status component that provides consistent branding, navigation controls, and real-time network status across all screens in the Shapyfy app.

### Key Features
- **Configurable layout**: Supports multiple layout combinations (logo, back button, title)
- **Network status indicator**: Always-visible badge showing Offline, Online, or Syncing state
- **Consistent branding**: Shapyfy logo text prominently displayed
- **Responsive design**: Adapts to different screen sizes
- **Accessibility**: Full screen reader support and reduced motion compliance

---

## Component Structure

The Top Bar consists of three zones with consistent spacing:

```
┌─────────────────────────────────────────┐
│ [Left Zone] [Center Zone] [Right Zone]  │
│ 16dp        flexible      16dp          │
└─────────────────────────────────────────┘
```

**Fixed Properties:**
- **Height**: 56 dp
- **Background**: #FFFFFF (from LightSurface)
- **Shadow**: Elevation 2 dp
- **Horizontal padding**: 16 dp (both edges)

---

## Layout Configurations

### Configuration 1: Logo + Network Badge
**Used in**: Dashboard (returning user), Active Workout

```
┌─────────────────────────────────────────┐
│ Shapyfy!                   [Network]    │
└─────────────────────────────────────────┘
```

**Elements:**
- Left: Shapyfy logo text
- Center: Empty
- Right: Network status badge

---

### Configuration 2: Logo + Title + Network Badge
**Used in**: Future use cases, custom screens

```
┌─────────────────────────────────────────┐
│ Shapyfy!    Workout           [Network] │
└─────────────────────────────────────────┘
```

**Elements:**
- Left: Shapyfy logo text
- Center: Screen title
- Right: Network status badge

---

### Configuration 3: Back + Logo + Network Badge
**Used in**: Exercise Detail screen

```
┌─────────────────────────────────────────┐
│ ← Shapyfy!                    [Network] │
└─────────────────────────────────────────┘
```

**Elements:**
- Left: Back button + Shapyfy logo text
- Center: Empty
- Right: Network status badge

---

### Configuration 4: Back + Title + Network Badge
**Used in**: Exercise Picker, Personalization, Recommendations, Program Preview

```
┌─────────────────────────────────────────┐
│ ← Select Exercise             [Network] │
└─────────────────────────────────────────┘
```

**Elements:**
- Left: Back button
- Center: Screen title
- Right: Network status badge

---

## Left Zone Elements

### Shapyfy Logo Text (Standalone)

**Used in**: Configuration 1, 2

- **Text**: "Shapyfy!"
- **Font size**: 24 sp / Bold
- **Color**: #000000 (from TextOnLight)
- **Position**: 16 dp from left edge
- **Vertical alignment**: Center in top bar
- **Width**: ~80 dp

**Purpose:** Primary branding element, always visible when no back button present

---

### Back Button + Logo Combination

**Used in**: Configuration 3

**Back Button:**
- **Icon**: Left arrow ← (Material Icon: `arrow_back`)
- **Color**: #000000 (from TextOnLight)
- **Size**: 24 dp
- **Position**: 16 dp from left edge
- **Tap area**: 48 dp × 48 dp (minimum touch target)
- **Margin right**: 12 dp (space before logo)
- **Behavior**: Calls `onBackClick` callback

**Logo Text:**
- **Text**: "Shapyfy!"
- **Font size**: 20 sp / Bold (slightly smaller to fit with back button)
- **Color**: #000000 (from TextOnLight)
- **Vertical alignment**: Center
- **Position**: 52 dp from left edge (after back button + margin)

**Total left zone width**: ~100-110 dp

---

### Back Button Only

**Used in**: Configuration 4

**Back Button:**
- **Icon**: Left arrow ←
- **Color**: #000000 (from TextOnLight)
- **Size**: 24 dp
- **Position**: 16 dp from left edge
- **Tap area**: 48 dp × 48 dp
- **Margin right**: 12 dp
- **Behavior**: Calls `onBackClick` callback

**Total left zone width**: ~50 dp

---

## Center Zone Elements

### Empty State
**Used in**: Configurations 1, 3

- No content
- Zone collapses, flexible space distributed between left and right elements
- Allows left and right zones to breathe with ample spacing

---

### Title Text
**Used in**: Configurations 2, 4

- **Text**: Screen title (examples: "Select Exercise", "Build Your Workout", "Workout Plans")
- **Font size**: 18 sp / Bold
- **Color**: #000000 (from TextOnLight)
- **Text alignment**: Center
- **Max width**: Dynamically calculated based on available space
  - Formula: `screen width - left zone width - right zone width - (32 dp margins)`
- **Truncation**: Ellipsis (...) if text exceeds max width
- **Vertical alignment**: Center in top bar
- **Line limit**: 1 line only

**Examples:**
- "Select Exercise" → Full display on all screens
- "Build Your Workout Plan" → May truncate on small screens to "Build Your Worko..."

---

## Right Zone: Network Status Badge

The network status badge is **always present** in the right zone across all configurations and screens.

### Container (Shared Across All States)

- **Shape**: Pill (horizontal capsule)
- **Border radius**: 16 dp
- **Height**: 32 dp
- **Padding**: Horizontal 12 dp, Vertical 6 dp
- **Position**: 16 dp from right edge of top bar
- **Vertical alignment**: Center in top bar (12 dp margin from top/bottom)
- **Layout**: Horizontal (icon + text, both centered vertically)
- **Interactive**: No (visual indicator only, not tappable)

---

### State 1: Offline Badge

**Context**: Device has no internet connectivity. Data cannot sync with server.

#### Container
- **Background**: #F3F3F8 (from LightBackground - very light grey)
- **Border**: 1 dp solid #E5E5E5 (from LightGray)
- **Border radius**: 16 dp
- **Height**: 32 dp
- **Padding**: Horizontal 12 dp, Vertical 6 dp
- **Shadow**: None

#### Icon
- **Icon**: Cloud with slash ☁️🚫 (Material Icon: `cloud_off`)
- **Color**: #999999 (medium grey)
- **Size**: 16 dp
- **Margin right**: 6 dp (space before text)
- **Vertical alignment**: Center with text

#### Text
- **Text**: "Offline"
- **Font size**: 13 sp / Medium
- **Color**: #999999 (medium grey)
- **Vertical alignment**: Center with icon
- **Letter spacing**: 0 (normal)

#### Total Badge Width
- Icon (16dp) + margin (6dp) + text (~45dp) + padding (24dp total) = **~91 dp**

#### Purpose
- Informs user they're working offline (expected in gyms)
- Neutral tone (not alarming) - offline is a normal state
- Data is saved locally and will sync when connection restored

---

### State 2: Online Badge

**Context**: Device is connected to internet. No active sync operations.

#### Container
- **Background**: #CBFFC9 (from LimeGreenBright)
- **Border**: 1 dp solid #A8E6CF (from LimeGreenDark)
- **Border radius**: 16 dp
- **Height**: 32 dp
- **Padding**: Horizontal 12 dp, Vertical 6 dp
- **Shadow**: None

#### Icon
- **Icon**: Cloud with checkmark ☁️✓ (Material Icon: `cloud_done`)
- **Color**: #24392A (from DarkSurface)
- **Size**: 16 dp
- **Margin right**: 6 dp
- **Vertical alignment**: Center with text

#### Text
- **Text**: "Online"
- **Font size**: 13 sp / Medium
- **Color**: #24392A (from DarkSurface)
- **Vertical alignment**: Center with icon
- **Letter spacing**: 0

#### Total Badge Width
- Icon (16dp) + margin (6dp) + text (~40dp) + padding (24dp total) = **~86 dp**

#### Behavior
- **Always visible** - Does NOT auto-hide after delay
- Provides reassurance that connection is active
- Data can be synced on demand

#### Purpose
- Confirms device has internet connectivity
- Happy, positive color (brand lime green)
- Indicates ready state for data sync

---

### State 3: Syncing Badge

**Context**: Device is actively syncing workout data with server. Operation in progress.

#### Container
- **Background**: #CBFFC9 (from LimeGreenBright - same as Online)
- **Border**: 1 dp solid #A8E6CF (from LimeGreenDark)
- **Border radius**: 16 dp
- **Height**: 32 dp
- **Padding**: Horizontal 12 dp, Vertical 6 dp
- **Shadow**: None

#### Icon (Animated)
- **Icon**: Cloud with sync arrows ☁️🔄 (Material Icon: `cloud_sync`)
- **Color**: #24392A (from DarkSurface)
- **Size**: 16 dp
- **Margin right**: 6 dp
- **Vertical alignment**: Center with text

**Animation:**
- **Type**: Continuous 360° rotation
- **Duration**: 1500 ms (1.5 seconds per full rotation)
- **Easing**: Linear (constant speed, no acceleration/deceleration)
- **Loop**: Infinite (runs continuously while syncing)
- **Direction**: Clockwise
- **Transform origin**: Center of icon
- **Performance**: Animation pauses when app is backgrounded, resumes on foreground

#### Text
- **Text**: "Syncing..."
- **Font size**: 13 sp / Medium
- **Color**: #24392A (from DarkSurface)
- **Vertical alignment**: Center with icon
- **Letter spacing**: 0
- **Ellipsis**: Included to indicate ongoing action

#### Total Badge Width
- Icon (16dp) + margin (6dp) + text (~55dp) + padding (24dp total) = **~101 dp**

#### Purpose
- Shows active data sync in progress
- Rotating icon confirms operation is not frozen
- Provides transparency about background operations
- User can continue working while sync happens

---

## State Transitions

### Offline → Online

**Visual Transition:**
- Badge background: #F3F3F8 → #CBFFC9 (300ms fade)
- Border color: #E5E5E5 → #A8E6CF (300ms fade)
- Icon: `cloud_off` → `cloud_done` (instant swap)
- Icon color: #999999 → #24392A (300ms fade)
- Text: "Offline" → "Online" (instant swap)
- Text color: #999999 → #24392A (300ms fade)

**Timing:** Smooth transition when connectivity restored

---

### Online → Syncing

**Visual Transition:**
- Badge background: No change (already #CBFFC9)
- Border: No change (already #A8E6CF)
- Icon: `cloud_done` → `cloud_sync` (instant swap)
- Icon animation: Rotation starts (fade in 300ms)
- Text: "Online" → "Syncing..." (instant swap)
- Text/icon color: No change (already #24392A)

**Timing:** Immediate when sync operation begins

---

### Syncing → Online

**Visual Transition:**
- Badge background: No change
- Border: No change
- Icon: `cloud_sync` → `cloud_done` (instant swap)
- Icon animation: Rotation stops (fade out 300ms)
- Text: "Syncing..." → "Online" (instant swap)

**Timing:** When sync operation completes successfully

---

### Online → Offline

**Visual Transition:**
- Badge background: #CBFFC9 → #F3F3F8 (300ms fade)
- Border color: #A8E6CF → #E5E5E5 (300ms fade)
- Icon: `cloud_done` → `cloud_off` (instant swap)
- Icon color: #24392A → #999999 (300ms fade)
- Text: "Online" → "Offline" (instant swap)
- Text color: #24392A → #999999 (300ms fade)

**Timing:** When connectivity is lost

---

### Syncing → Offline

**Visual Transition:**
- Badge background: #CBFFC9 → #F3F3F8 (300ms fade)
- Border color: #A8E6CF → #E5E5E5 (300ms fade)
- Icon: `cloud_sync` → `cloud_off` (instant swap)
- Icon animation: Rotation stops immediately
- Icon color: #24392A → #999999 (300ms fade)
- Text: "Syncing..." → "Offline" (instant swap)
- Text color: #24392A → #999999 (300ms fade)

**Timing:** When connectivity lost during sync operation

---

### Offline → Syncing

**Special Case:** This transition should NOT occur (cannot sync without connection)

**Fallback Logic:** If this state is attempted, immediately transition to Offline state and log warning

---

## Layout Calculations

### Configuration 1: Logo + Network Badge
**Example on 375dp width screen:**

```
┌──────────────────────────────────────────────┐
│ 16dp │ Logo │ Flexible Space │ Badge │ 16dp │
│      │ 80dp │    162dp       │ 91dp  │      │
└──────────────────────────────────────────────┘

Total: 16 + 80 + 162 + 91 + 16 = 375dp
```

**Flexible space calculation:**
- Screen width: 375dp
- Left margin: 16dp
- Logo: 80dp
- Right margin: 16dp
- Badge: 91dp (Offline state, widest)
- Flexible space: 375 - 16 - 80 - 91 - 16 = **172dp**

---

### Configuration 4: Back + Title + Network Badge
**Example on 375dp width screen:**

```
┌───────────────────────────────────────────────────┐
│ 16dp │ Back │ 12dp │ Title │ 16dp │ Badge │ 16dp │
│      │ 24dp │      │ 186dp │      │ 91dp  │      │
└───────────────────────────────────────────────────┘

Total: 16 + 24 + 12 + 186 + 16 + 91 + 16 = 361dp
```

**Title max width calculation:**
- Screen width: 375dp
- Left margin: 16dp
- Back button: 24dp
- Back-to-title margin: 12dp
- Title-to-badge margin: 16dp
- Badge: 101dp (Syncing state, widest)
- Right margin: 16dp
- Title max width: 375 - 16 - 24 - 12 - 16 - 101 - 16 = **190dp**

---

### Configuration 3: Back + Logo + Network Badge
**Example on 375dp width screen:**

```
┌─────────────────────────────────────────────────────┐
│ 16dp │ Back │ 12dp │ Logo │ Flexible │ Badge │ 16dp │
│      │ 24dp │      │ 65dp │  141dp   │ 91dp  │      │
└─────────────────────────────────────────────────────┘

Total: 16 + 24 + 12 + 65 + 141 + 91 + 16 = 365dp
```

**Flexible space calculation:**
- Screen width: 375dp
- Left margin: 16dp
- Back button: 24dp
- Back-to-logo margin: 12dp
- Logo: 65dp
- Badge: 91dp
- Right margin: 16dp
- Flexible space: 375 - 16 - 24 - 12 - 65 - 91 - 16 = **151dp**

---

## Responsive Behavior

### Small Screens (< 360dp width)

**Priority order** when space is constrained:
1. **Back button** - Always full size (48dp touch target)
2. **Network badge** - Always full size (vital status info)
3. **Logo** - Can reduce font size: 24sp → 20sp
4. **Center title** - Truncates first with ellipsis

**Adjustments:**
- Logo font size: 24sp → 20sp (if needed)
- Center title: Aggressive truncation with ellipsis
- Minimum title visible width: 80dp (before truncation)

---

### Medium Screens (360dp - 414dp width)

**Typical devices:** Most modern Android phones, iPhone 12/13/14

- All elements fit comfortably
- No special adjustments needed
- Standard spacing maintained

---

### Large Screens (≥ 414dp width)

**Typical devices:** iPhone Pro Max models, large Android devices

- All elements have generous spacing
- No adjustments needed
- Consider max content width constraints (future enhancement)

---

## Accessibility

### Touch Targets

**Minimum Requirements:**
- Back button: 48 dp × 48 dp ✓ (meets WCAG 2.1 AA standard)
- Network badge: Visual indicator only (not interactive)
- Logo: Not interactive (no touch target requirement)

---

### Screen Reader Announcements

**Back Button:**
- Label: "Navigate back"
- Action: "Double tap to go back"

**Logo:**
- Label: "Shapyfy logo"
- Action: None (decorative element)

**Network Badge States:**
- **Offline**: "No internet connection. Your data is saved locally."
- **Online**: "Connected to internet. Ready to sync."
- **Syncing**: "Syncing workout data with server. This may take a moment."

**Implementation:**
```kotlin
contentDescription = when (networkState) {
    NetworkState.OFFLINE -> "No internet connection. Your data is saved locally."
    NetworkState.ONLINE -> "Connected to internet. Ready to sync."
    NetworkState.SYNCING -> "Syncing workout data with server. This may take a moment."
}
```

---

### Reduced Motion Support

**Syncing Icon Animation:**
- **Default**: Continuous 360° rotation
- **Reduced Motion Enabled**: Static icon (no rotation)
- **Detection**: Check OS accessibility settings
- **Fallback**: Badge still changes color/text to indicate syncing state

**Implementation:**
```kotlin
val isReduceMotionEnabled = LocalAccessibilityManager.current?.isReduceMotionEnabled == true

if (!isReduceMotionEnabled && networkState == NetworkState.SYNCING) {
    // Render rotating icon animation
} else {
    // Render static icon
}
```

---

## Color Reference

All colors sourced from: `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`

### Top Bar Container
- **Background**: #FFFFFF (LightSurface)
- **Shadow**: Default elevation shadow

### Left Zone (Logo, Back Button)
- **Text/Icons**: #000000 (TextOnLight)

### Offline Badge
- **Background**: #F3F3F8 (LightBackground)
- **Border**: #E5E5E5 (LightGray)
- **Icon/Text**: #999999 (medium grey)

### Online Badge
- **Background**: #CBFFC9 (LimeGreenBright)
- **Border**: #A8E6CF (LimeGreenDark)
- **Icon/Text**: #24392A (DarkSurface)

### Syncing Badge
- **Background**: #CBFFC9 (LimeGreenBright)
- **Border**: #A8E6CF (LimeGreenDark)
- **Icon/Text**: #24392A (DarkSurface)

---

## Component API

### Kotlin Compose Component Signature

```kotlin
@Composable
fun ShapyfyTopBar(
    // Layout configuration
    showBackButton: Boolean = false,
    onBackClick: (() -> Unit)? = null,
    centerTitle: String? = null,
    showLogo: Boolean = true,

    // Network state (always shown)
    networkState: NetworkState = NetworkState.ONLINE,

    // Styling
    backgroundColor: Color = LightSurface,
    elevation: Dp = 2.dp,

    // Modifiers
    modifier: Modifier = Modifier
)

enum class NetworkState {
    OFFLINE,
    ONLINE,
    SYNCING
}
```

### Parameter Descriptions

**showBackButton**: `Boolean`
- Default: `false`
- Shows back arrow button on left side
- Requires `onBackClick` callback if true

**onBackClick**: `(() -> Unit)?`
- Default: `null`
- Callback invoked when back button tapped
- Required if `showBackButton = true`

**centerTitle**: `String?`
- Default: `null`
- Text displayed in center zone
- Truncates with ellipsis if too long
- If null, center zone remains empty

**showLogo**: `Boolean`
- Default: `true`
- Shows "Shapyfy!" text logo on left side
- Automatically adjusts position if back button present

**networkState**: `NetworkState`
- Default: `NetworkState.ONLINE`
- Controls network badge appearance and animation
- Always visible (never hidden)
- Options: `OFFLINE`, `ONLINE`, `SYNCING`

**backgroundColor**: `Color`
- Default: `LightSurface` (#FFFFFF)
- Top bar background color
- Typically not changed from default

**elevation**: `Dp`
- Default: `2.dp`
- Shadow elevation below top bar
- Typically not changed from default

**modifier**: `Modifier`
- Default: `Modifier`
- Standard Compose modifier for additional styling

---

## Usage Examples

### Example 1: Dashboard Top Bar

**Configuration**: Logo + Network Badge (no back button, no title)

```kotlin
@Composable
fun DashboardScreen(
    viewModel: DashboardViewModel
) {
    val networkState by viewModel.networkState.collectAsState()

    Scaffold(
        topBar = {
            ShapyfyTopBar(
                showBackButton = false,
                showLogo = true,
                centerTitle = null,
                networkState = networkState
            )
        }
    ) { paddingValues ->
        // Screen content
    }
}
```

---

### Example 2: Exercise Picker Top Bar

**Configuration**: Back + Title + Network Badge (no logo)

```kotlin
@Composable
fun ExercisePickerScreen(
    navController: NavController,
    viewModel: WorkoutViewModel
) {
    val networkState by viewModel.networkState.collectAsState()

    Scaffold(
        topBar = {
            ShapyfyTopBar(
                showBackButton = true,
                onBackClick = { navController.popBackStack() },
                showLogo = false,
                centerTitle = "Select Exercise",
                networkState = networkState
            )
        }
    ) { paddingValues ->
        // Screen content
    }
}
```

---

### Example 3: Exercise Detail Top Bar

**Configuration**: Back + Logo + Network Badge (no title)

```kotlin
@Composable
fun ExerciseDetailScreen(
    navController: NavController,
    viewModel: ExerciseDetailViewModel
) {
    val networkState by viewModel.networkState.collectAsState()

    Scaffold(
        topBar = {
            ShapyfyTopBar(
                showBackButton = true,
                onBackClick = {
                    // Auto-save and return to Active Workout
                    viewModel.saveProgress()
                    navController.popBackStack()
                },
                showLogo = true,
                centerTitle = null,
                networkState = networkState
            )
        }
    ) { paddingValues ->
        // Screen content
    }
}
```

---

### Example 4: Personalization Top Bar

**Configuration**: Back + Title + Network Badge (no logo)

```kotlin
@Composable
fun PersonalizationScreen(
    navController: NavController,
    viewModel: OnboardingViewModel
) {
    val networkState by viewModel.networkState.collectAsState()

    Scaffold(
        topBar = {
            ShapyfyTopBar(
                showBackButton = true,
                onBackClick = { navController.popBackStack() },
                showLogo = false,
                centerTitle = "Build Your Workout",
                networkState = networkState
            )
        }
    ) { paddingValues ->
        // Screen content
    }
}
```

---

## Network State Management

### ViewModel Integration

**Recommended Pattern:**

```kotlin
class WorkoutViewModel : ViewModel() {

    private val _networkState = MutableStateFlow(NetworkState.ONLINE)
    val networkState: StateFlow<NetworkState> = _networkState.asStateFlow()

    init {
        observeNetworkConnectivity()
        observeSyncStatus()
    }

    private fun observeNetworkConnectivity() {
        viewModelScope.launch {
            networkMonitor.isConnected.collect { isConnected ->
                if (!isConnected) {
                    _networkState.value = NetworkState.OFFLINE
                } else if (_networkState.value == NetworkState.OFFLINE) {
                    _networkState.value = NetworkState.ONLINE
                }
            }
        }
    }

    private fun observeSyncStatus() {
        viewModelScope.launch {
            syncManager.isSyncing.collect { isSyncing ->
                if (isSyncing && _networkState.value != NetworkState.OFFLINE) {
                    _networkState.value = NetworkState.SYNCING
                } else if (!isSyncing && _networkState.value == NetworkState.SYNCING) {
                    _networkState.value = NetworkState.ONLINE
                }
            }
        }
    }
}
```

---

### State Transition Rules

**Valid Transitions:**
- `OFFLINE → ONLINE` - When connectivity restored
- `ONLINE → OFFLINE` - When connectivity lost
- `ONLINE → SYNCING` - When sync operation starts
- `SYNCING → ONLINE` - When sync completes successfully
- `SYNCING → OFFLINE` - When connectivity lost during sync

**Invalid Transitions:**
- `OFFLINE → SYNCING` - Cannot sync without connection (force to OFFLINE)

---

## Performance Considerations

### Animation Performance

**Syncing Icon Rotation:**
- Runs on GPU (hardware accelerated)
- Uses `Modifier.graphicsLayer { rotationZ = ... }` for performance
- Pauses when app backgrounded (saves battery)
- Resumes smoothly when app foregrounded

**Optimization:**
```kotlin
val rotation by infiniteTransition.animateFloat(
    initialValue = 0f,
    targetValue = 360f,
    animationSpec = infiniteRepeatable(
        animation = tween(1500, easing = LinearEasing),
        repeatMode = RepeatMode.Restart
    )
)

// Use graphicsLayer for GPU acceleration
Icon(
    modifier = Modifier.graphicsLayer {
        rotationZ = rotation
    }
)
```

---

### Memory Management

**State Flow:**
- Network state uses `StateFlow` (single active value, no replay)
- Automatically cleaned up when ViewModel destroyed
- No memory leaks from observer subscriptions

**Icon Resources:**
- Material Icons are vector drawables (small memory footprint)
- Cached by Compose runtime
- Reused across all Top Bar instances

---

## Testing Considerations

### Unit Tests

**Network State Transitions:**
```kotlin
@Test
fun `when connectivity lost, state transitions to OFFLINE`() {
    // Given
    viewModel.networkState.value = NetworkState.ONLINE

    // When
    networkMonitor.emitConnectivityChange(false)

    // Then
    assertEquals(NetworkState.OFFLINE, viewModel.networkState.value)
}

@Test
fun `when sync starts while online, state transitions to SYNCING`() {
    // Given
    viewModel.networkState.value = NetworkState.ONLINE

    // When
    syncManager.startSync()

    // Then
    assertEquals(NetworkState.SYNCING, viewModel.networkState.value)
}
```

---

### UI Tests

**Visual States:**
```kotlin
@Test
fun topBar_showsOfflineBadge_whenNetworkStateIsOffline() {
    composeTestRule.setContent {
        ShapyfyTopBar(networkState = NetworkState.OFFLINE)
    }

    composeTestRule
        .onNodeWithText("Offline")
        .assertIsDisplayed()
}

@Test
fun topBar_showsRotatingIcon_whenNetworkStateIsSyncing() {
    composeTestRule.setContent {
        ShapyfyTopBar(networkState = NetworkState.SYNCING)
    }

    composeTestRule
        .onNodeWithContentDescription("Syncing workout data")
        .assertIsDisplayed()
}
```

---

## Design Rationale

### Why Always-Visible Network Badge?

**Decision**: Network badge never auto-hides, always visible in all states

**Rationale:**
1. **Transparency**: Users always know connection status
2. **Gym context**: Connectivity issues common in gyms (thick walls, basement locations)
3. **Offline-first app**: Users need confidence their data is safe locally
4. **Sync awareness**: Users see when data is syncing to cloud
5. **No surprises**: Clear indicator prevents confusion about why features may be limited

---

### Why Lime Green for Online/Syncing?

**Decision**: Use brand color (lime green) for positive states

**Rationale:**
1. **Brand consistency**: Reinforces Shapyfy brand identity
2. **Positive association**: Lime green = "good to go" across app
3. **Visual hierarchy**: Lime green draws eye without being alarming
4. **Contrast**: Dark text on lime background is highly readable

---

### Why Grey for Offline (Not Red)?

**Decision**: Use neutral grey instead of error red for offline state

**Rationale:**
1. **Not an error**: Offline is expected/normal in gym environment
2. **Reduces anxiety**: Red would imply something is broken
3. **Calm tone**: Grey communicates "informational" not "urgent"
4. **Data safety**: Users' work is safe locally (not a failure state)

---

### Why Rotating Icon for Syncing?

**Decision**: Animate sync icon with continuous rotation

**Rationale:**
1. **Progress indicator**: Shows operation is active, not frozen
2. **Standard pattern**: Users understand rotating icon = "working"
3. **Subtle feedback**: Confirms background operations without being distracting
4. **Performance transparent**: Smooth rotation shows app is responsive

---

## Future Enhancements (Not in MVP)

### Potential Features

**1. Tappable Network Badge**
- Tap badge to see detailed network info
- Shows last sync time
- Shows sync queue size
- Manual sync trigger button

**2. Network Quality Indicator**
- Show signal strength (weak/moderate/strong)
- Different icon or color for poor connection
- Helps explain slow sync operations

**3. Sync Progress Indicator**
- Show percentage during sync
- "Syncing 3/10 workouts..."
- More transparency for long sync operations

**4. Offline Timer**
- Show how long device has been offline
- "Offline for 45 minutes"
- Helps user understand when last sync occurred

**5. Smart Auto-Hide**
- Online state fades after 3 seconds
- Returns when tapped or on scroll-to-top
- Reduces visual clutter for experienced users

---

## Related Files

**Component implementation** (to be created):
- `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/molecule/ShapyfyTopBar.kt`

**Feature documentation** (updated with Top Bar specs):
- `AI_DOCS/documentation/features/dashboard.md`
- `AI_DOCS/documentation/features/workout_process.md`
- `AI_DOCS/documentation/features/onboarding.md`

**Implementation guidance**:
- `AI_DOCS/documentation/components/top_bar_implementation.md`

**Design system**:
- `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Color.kt`
- `composeApp/src/commonMain/kotlin/com/shapyfy/design_system/theme/Typography.kt`

---

**Status**: ✅ Specification Complete - Ready for Implementation
