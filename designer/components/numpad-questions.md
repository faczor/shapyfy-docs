# GymNumpad Implementation Questions

**Date:** 2025-01-22
**Status:** Deferred - Using practical implementations for MVP

---

## Technical Challenges & Decisions

### 1. Glow Implementation (Double-Layer System)

**Spec Requirement:**
- Field focused state: Inner glow (6dp blur #A8E6CF at 40%) + Outer glow (16dp blur #2D7A5F at 15%)
- Display value: 16dp blur #A8E6CF at 30%
- Button press: 12dp blur #A8E6CF at 50%

**Compose Limitation:**
- Native `shadow()` modifier creates drop shadows (with offset), not glows
- No built-in "blur without offset" support

**Options:**
- A) Use multiple overlapping shadows with different blur radii?
- B) Use `drawBehind` with custom radial gradient?
- C) Accept shadow elevation as close-enough approximation?

**Decision (MVP):**
→ **Option B** - `drawBehind` with radial gradient for field/display glows
→ Skip button press glow for MVP (add in Phase 2 if needed)

**Rationale:** Most flexible, works on all API levels, gives good approximation of glow effect

---

### 2. Motion Blur (Counter Animation)

**Spec Requirement:**
- Value change animation should have motion blur: 0dp → 2dp → 0dp during slide
- Creates "velocity feel" and "weightful" mass effect

**Compose Limitation:**
- No native motion blur support
- `blur()` modifier requires RenderEffect (API 31+, excludes Android 11 and below)

**Options:**
- A) Skip it (value animation without blur)?
- B) Simulate with semi-transparent trailing copies?
- C) Use blur() modifier (requires RenderEffect, API 31+)?

**Decision (MVP):**
→ **Option A** - Skip motion blur, implement scale bounce animation only

**Rationale:** API 31+ requirement too limiting for MVP. Focus on scale bounce (1.0 → 0.98 → 1.02 → 1.0) which gives "weight" feel without blur.

**Future:** Can add blur() with API level check in Phase 2

---

### 3. Inner Shadow (Button Press Depth)

**Spec Requirement:**
- Number buttons on press: inset 0dp 2dp 4dp rgba(0,0,0,0.2)
- Creates "sinking into surface" 3D depth illusion

**Compose Limitation:**
- No native inset shadow support
- Shadow modifier only creates outer shadows

**Options:**
- A) Skip it entirely?
- B) Fake it with `drawBehind` gradient overlay?
- C) Use separate semi-transparent overlay layer?

**Decision (MVP):**
→ **Option B** - `drawBehind` with subtle top-to-bottom gradient overlay on pressed state

**Rationale:** Good enough visual approximation, lightweight, works everywhere

---

### 4. Chevron Overshoot Animation

**Spec Requirement:**
- Rotation: 0° → **190°** → 180° (overshoot with spring-back)
- Scale: 1.0 → **1.1** → 1.0 (subtle pump)
- Color: #B8BCC8 → #FFFFFF (brightens)
- Spring physics: damping 0.7, stiffness 350

**Current Implementation:**
- Uses `animateFloatAsState` targeting 180° directly (no overshoot)
- Correct spring physics, but missing intermediate overshoot keyframe

**Options:**
- A) Use `animateFloatAsState` with manual keyframes (0 → 190 → 180)?
- B) Use `Animatable` with spring to 190°, then spring back to 180°?
- C) Use custom animation spec with overshoot?
- D) Accept current spring animation as "close enough"?

**Decision (MVP):**
→ **Option C** - Use spring with `dampingRatio < 1.0` for natural overshoot

**Rationale:** Spring with low damping (0.5-0.6) naturally creates overshoot. Simplest, most physics-accurate approach.

**Implementation:**
```kotlin
spring(
    dampingRatio = 0.5f,  // Lower = more overshoot
    stiffness = 350f
)
```

---

## MVP Implementation Strategy

**Phase 1 (This PR):**
1. ✅ Fix preset/increment visual styles (outlined pills, borders, icons)
2. ✅ Implement field double-layer glow with radial gradient
3. ✅ Add chevron overshoot + scale + color animation
4. ✅ Implement display glow with radial gradient
5. ✅ Add value counter scale bounce animation (without motion blur)
6. ✅ Add ✓ icon to active preset
7. ✅ Implement increment disabled state

**Phase 2 (Polish Pass):**
8. ⏸️ Button press glow (if needed after testing)
9. ⏸️ Motion blur (API 31+ with fallback)
10. ⏸️ Inner shadow refinement
11. ⏸️ Advanced animation choreography (multi-phase preset animations)

---

## Notes

- All decisions prioritize **cross-platform compatibility** and **API level support**
- Focus on achieving 90% of premium feel with 100% reliable techniques
- Can iterate with advanced effects in Phase 2 after user testing
- Document performance impact of radial gradients on low-end devices

---

**Last Updated:** 2025-01-22
**Next Review:** After Phase 1 implementation complete
