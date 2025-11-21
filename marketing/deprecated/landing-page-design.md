---
tags:
  - design
  - landing
  - specs
status: DRAFT
---

# Shapyfy Landing Page - Visual Design Specification

**Vibe:** ⚡️ PREMIUM KINETIC ENERGY.
**Philosophy:** "If it's static, it's dead." Everything breathes, reacts, and moves.
**Banned Items:** No Emojis. No Flat Colors. No Default Shadows.

---

## 1. Global Design Language

### 1.1 Layout & Spacing (The "Swiss" Grid)
**Container:**
-   **Max Width:** `1360px` (Desktop).
-   **Padding:** `64px` (Desktop) / `32px` (Mobile).
-   **Grid:** 12-column fluid grid with `32px` gutters.

**Vertical Rhythm (Harmonic Sequence):**
-   **Hero → Problem:** `96px`
-   **Problem → Solution:** `144px`
-   **Solution → Features:** `192px`
-   **Features → Beta:** `240px`
-   **Mobile:** `64px` → `96px` → `128px`
-   *Rationale: Predictable acceleration on the Swiss grid.*

### 1.2 Typography
**Headings (Montserrat):**
-   **Weight:** ExtraBold (800) / Black (900).
-   **Tracking:** `-0.02em` (Tight but accessible).
-   **Leading:** `1.1`.
-   **Texture:** Animated Gradient (`8s` loop). Starts as **Mint Electric**, shifts slowly to **Chrome Lavender**. Feels like liquid metal breathing.

**Body (Inter):**
-   **Weight:** Medium (500).
-   **Tracking:** `-0.01em`.
-   **Leading:** `1.6`.

### 1.3 Color Palette (Standardized)

**Primary Energy:**
-   **Mint Electric:** `linear-gradient(135deg, #A8E6CF 0%, #55C6A8 50%, #24392A 100%)`
-   **Coral Magma:** `linear-gradient(135deg, #FFC4C8 0%, #FF9999 50%, #FF6B6B 100%)`
-   **Chrome Lavender:** `linear-gradient(135deg, #FFFFFF 0%, #E8E3F3 50%, #B8A5D9 100%)`
-   **Premium Gold:** `linear-gradient(to bottom, #FFD700 0%, #B8860B 100%)`

**Surface Materials:**
-   **Glass:** `background: rgba(255, 255, 255, 0.65); backdrop-filter: blur(24px); border: 1px solid rgba(255, 255, 255, 0.4);`
-   **Deep Void:** `#0A0F0C` (For high contrast sections).

### 1.4 Performance Guardrails 🛡️
-   **FPS Cap:** WebGL effects capped at 60fps (30fps on low-power mode).
-   **Reduced Motion:** If `prefers-reduced-motion: reduce` is detected:
    -   Disable WebGL displacement.
    -   Disable Gyro-Parallax.
    -   Switch "Magnetic" buttons to standard hover states.
    -   **Cursor:** Standard system cursor.

---

## 2. Section-by-Section Visuals & Behaviors

### Section 1: Hero (The "Cinematic" Opener)
**Background:**
-   **Clean:** No mouse ripple. Pure focus on the content.

**Component Behaviors:**
-   **Headline:** **Mask Reveal**. Words "Bad WiFi" and "Offline" reveal via a 3D extruded chrome text effect that rotates letter-by-letter on load.
-   **Phone Mockup (The "Portal"):**
    -   **Pulse:** Emits a slow-breathing **Mint Electric** pulse (like a heartbeat).
    -   **Intensity:** Pulse speed and brightness intensify with scroll velocity.
    -   *Note: No gyro-parallax. Single source of energy.*

### Section 2: Problem (The "Holographic" Grid)
**Vibe:** Tech-noir. Darker, serious, high contrast.

**Component Behaviors:**
-   **Cards:**
    -   **Hover:** **3D Tilt** towards cursor.
    -   **Spotlight:** Radial gradient follows cursor inside border.
-   **Visuals:**
    -   **Motion:** All 3D assets frozen in **ultra-slow motion (0.2x)**.
    -   **Effect:** Subtle chromatic aberration vignette that gets stronger on hover.
    -   **Brain (Plan Paralysis):** 3D glass brain. No crackle.
    -   **Router (WiFi):** Red light (ensure WCAG AA compliance or use Coral Magma with white stroke).

### Section 3: Solution (The "Cleanse")
**Vibe:** Fresh air. Bright, clean, hopeful.

**Component Behaviors:**
-   **AI Hero Card (Center):**
    -   **Aurora Borealis:** The background glow color-shifts based on scroll progress through the section. Starts icy **Mint Electric** → warms to **Coral Magma** as you scroll down.
    -   **Levitation:** Permanent slow Y-axis sine wave.
-   **Visuals:**
    -   **Rising Graph:** 3D line draws itself with a Mint trail.

### Section 4: Feature Highlights (The "Precision")
**Component Behaviors:**
-   **Checkmarks:** Animated SVG paths (draw themselves) + sparkle burst.
-   **Visuals:**
    -   **Dumbbell:** 3D Chrome Dumbbell slowly rotates and casts colored shadows (Mint/Coral).

### Section 5: Beta Incentive (The "Rush")
**Vibe:** High stakes. Exclusive.

**Component Behaviors:**
-   **Countdown:** **Mechanical Tumble**. 3D numbers physically tumble like a slot machine.
    -   **Lock:** When numbers lock, trigger a single **Premium Gold shockwave** that tints the section.
    -   *Zero State:* If it hits zero, trigger **Global Sold Out Sequence**.
-   **New Price:** **Heat Distortion**. Reveals with a shimmer like asphalt on a hot day.

### Section 6: Social Proof (The "Club")
**Component Behaviors:**
-   **Marquee:** Infinite scroll. Pauses on hover.
-   **Avatars:** Scale 1.5x on hover.

### Section 7: Footer (The "Signature")
**Component Behaviors:**
-   **Links:** No underlines. Hover = Mint color + dot indicator.

---

## 3. Interaction Physics (The "Feel")

### 3.1 Cursor Physics (Luxury Mode)
-   **Default:** Subtle single **Mint Orb** follow (spring physics, damping 0.8).
-   **Hover:** On hover of magnetic elements, orb **explodes** into 8–12 particles that get sucked in with violent ease-in-out-back.
-   **Reduced Motion:** Clean scale + brightness pulse. No particles.
-   **Color Tinting:** Particles temporarily tint to the element's dominant color.

### 3.2 Post-Signup Celebration
-   **Trigger:** Form Submit Success OR Countdown Zero.
-   **Action:** Full-screen takeover.
-   **See:** `marketing/post-signup-success.md` for full details.

---

## 4. Asset Checklist (Complete)
-   [ ] **3D iPhone 15 Pro Render** (Titanium).
-   [ ] **3D WiFi Router** (Red Light).
-   [ ] **3D Glass Sheet** (Shattered).
-   [ ] **3D Brain** (Electric Crackle).
-   [ ] **3D Mechanical Numbers** (0-9).
-   [ ] **3D Chrome Dumbbell** (Rotating).
-   [ ] **3D Rising Graph** (Mint Trail).
-   [ ] **3D Golden Ticket** (Physical Card).
