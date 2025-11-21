---
tags:
  - design
  - success-state
  - delight
status: DRAFT
---

# Post-Signup Celebration Screen (The "God-Tier" Moment)

**Trigger:**
1.  **Success:** User successfully submits the "Join Beta" form.
2.  **Sold Out:** Global countdown hits 00:00:00.

**Goal:** Make the user feel like they just joined the Illuminati of Fitness (or missed the boat entirely).

---

## 1. The Sequence (Total Duration: 4.5s)

### Phase 1: The Takeover (0.0s - 0.5s)
-   **Transition:** The form button ("Save My Spot") explodes into a radial shockwave that covers the entire screen in **Deep Void** (`#0A0F0C`).
-   **Sound:** A subtle, low-frequency "thud" (like a heavy door closing).
-   **Haptics:** Single device vibration (long haptic burst) + sub-bass "thoom" (if permission granted).

### Phase 2: The Assembly (0.5s - 2.0s)
-   **Visual:** Shards of **Mint Electric** glass fly in from the edges of the screen.
-   **Action:** They magnetically assemble in the center to form the **Shapyfy Logo** (3D Render).
-   **Effect:** A bright lens flare flashes across the logo as it locks into place.

### Phase 3: The Announcement (2.0s - 3.5s)
-   **Confetti Upgrade (Champagne Physics):**
    -   **Burst:** First burst shoots **downward** super fast (like champagne cork pressure).
    -   **Float:** Particles decelerate, reverse, and float **upward** forever with negative gravity.
    -   **Shapes:** 5% of particles are tiny **3D Dumbbells** that leave **Mint vapor trails**.
    -   **Colors:** Mint, **Premium Gold**, White.

-   **Variant A: Success (User Joined)**
    -   **Text:** "WELCOME TO THE FUTURE"
    -   **Style:** **Premium Gold** Gradient.
    -   **Animation:** 3D Extrusion.

-   **Variant B: Sold Out (Timer Zero)**
    -   **Text:** "BETA SOLD OUT"
    -   **Subtext:** "YOU WITNESSED HISTORY"
    -   **Style:** **Coral Magma** (Radioactive Glow).
    -   **Action:** Auto-redirect to Waitlist Form after 3s.

### Phase 4: The Return (3.5s - 4.5s)
-   **Action:** The full-screen overlay zooms out (scales down to 0) and disappears.
-   **Result:** User is back on the Landing Page.
-   **Persistent Change (Success Only):** The "Join Beta" form is replaced by a **3D Golden Ticket** card that says:
    > "You're In. Check your email."

---

## 2. Technical Specs

### 2.1 Confetti Physics
-   **Library:** `canvas-confetti` (lightweight).
-   **Particle Count:** 150.
-   **Spread:** 70 degrees.
-   **Start Velocity:** 60.
-   **Shapes:** Square, Circle, Dumbbell (SVG path).

### 2.2 Accessibility
-   **Reduced Motion:**
    -   Skip the explosion and flying shards.
    -   Simple fade-in of the message.
    -   No confetti.
