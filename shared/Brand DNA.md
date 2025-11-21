# 🧬 Shapyfy: Brand & Product DNA
**Version:** 1.0
**Status:** Locked

---

## 1. The Strategic Core
### Mission
To empower strength trainees with an **unbreakable, offline-first digital tool** that replaces the notebook. We provide seamless reliability for the Gym Rat and intelligent guidance for the Newbie.

### Value Proposition
* **Offline First:** Zero dependency on internet connection. Speed is a feature.
* **AI Powered:** Smart defaults and personalized recommendations, not generic templates.
* **Excel Killer:** Data-dense but visually clean.

### The Audience
1.  **Primary (The Retainer):** "The Gym Rat." Needs speed, efficiency, and dark mode.
2.  **Secondary (The Acquirer):** "The Newbie." Needs safety, guidance, and clear instructions.

---

## 2. Visual Identity
**Archetype:** "The Digital Coach" (Smart, Precise, Encouraging).
**Vibe:** Soft Tech / Calm Focus.

### Color Palette
| Role               | Color Name         | Hex Code  | Usage Rule                                            |
| :----------------- | :----------------- | :-------- | :---------------------------------------------------- |
| **Primary Action** | **LimeGreenDark**  | `#A8E6CF` | Main CTAs, Active States, Progress Bars.              |
| **Background**     | **DarkNavy**       | `#2D3142` | Default "Gym Mode" background.                        |
| **Intelligence**   | **LavenderMedium** | `#D4D0E8` | AI suggestions & tips (distinct from success/action). |
| **Alert**          | **CoralPink**      | `#FF9999` | Destructive actions or High Intensity warnings.       |
| **Text Primary**   | **White**          | `#FFFFFF` | High contrast text on DarkNavy.                       |

### Typography
* **Font Family:** Montserrat (consider: Roboto / Inter for the data).
* **Why:** Open shapes, modern feel, excellent readability for numbers/timers.

### Theme Strategy
* **Default:** Follow System Settings.
* **Priority:** **Dark Mode** is the "Hero" experience (optimized for gym environments).

---

## 3. Interaction Principles (UX)
### 1. Optimistic UI
* **Rule:** The interface updates instantly. Never show a loading spinner for a local action (like finishing a set).
* **Goal:** Zero friction.

### 2. The Thumb Zone
* **Rule:** All critical workflow actions (Start Workout, Log Set, Rest, Edit) must be reachable with one hand at the bottom 30% of the screen.

### 3. Haptic Feedback
* **Rule:** Use tactile vibrations for timers completing and sets saved. The user feels the app working without looking.

---

## 4. Behavioral Logic
### Smart Defaults ("Pre-fill, don't ask")
* If history exists: Pre-fill input fields with last session's data (or progressive overload suggestion).
* If new: Pre-fill with AI recommendation.
* *Never show a blank cell.*

### Input Mechanism
* **Tool:** Custom Numpad (Bottom Sheet).
* **Avoid:** System keyboard or small +/- steppers for large weight jumps.

### The Rest Flow
* **Trigger:** Clicking "Finish Set" **automatically** starts the rest timer.
* **Philosophy:** Do not make the user click twice for one logical sequence.

### Feedback Tone (Gamification)
* **Level:** "The Micro-Celebrator."
* **Behavior:** Celebrate Personal Records (PRs) with subtle gold sparks and unique haptics.
* **Avoid:** Childish XP bars or cartoonish gamification.

---