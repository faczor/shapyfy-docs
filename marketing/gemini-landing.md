# **Section 1 (The Hero)**.

I have split this into two parts as requested:

1. **The Human Overview:** A clean summary of the content and layout.
    
2. **The Developer Specification:** A rigorous technical blueprint. You can hand Part 2 directly to a frontend developer (or use it as a prompt for an AI coder), and there will be zero ambiguity.
    

---

## Part 1: The Overview (For Humans)

The Goal:

To instantly communicate "Speed" and "Intelligence." The user lands in a premium, dark, high-tech environment. It feels like an engineering tool, not a fitness blog.

**The Layout:**

- **Desktop:** A classic 50/50 split.
    
    - **Left Side:** The text logic (Hook -> Problem -> Solution -> Action).
        
    - **Right Side:** The visual proof (The 3D Floating Phone).
        
- **Mobile:** A vertical stack. Text on top, visual in the middle, CTA button sticky or easily accessible at the bottom.
    

**The Content:**

- **Tagline:** `OFFLINE-FIRST STRENGTH TRAINING`
    
- **Headline:** **The Speed of a Tracker.** **The Brain of a Coach.**
    
- **Subhead:** Shapyfy is the first offline-first strength tool that adapts to your progress. Get personalized AI recommendations without the loading screens.
    
- **Button:** **[ Request Alpha Access ]**
    

---

## Part 2: The Developer Specification (For Code)

Instruction to Developer:

Build the Hero Section using the specifications below. Do not deviate from colors, spacing, or effects. The aesthetic is "Premium Dark SaaS" (e.g., Linear, Stripe, Apple).

### 1. Global Variables (CSS Tokens)

_Use these exact hex codes._

CSS

```
:root {
  /* The Palette */
  --bg-navy: #2D3142;        /* Main Background */
  --bg-navy-deep: #1F222E;   /* Darker shade for gradients */
  --brand-mint: #A8E6CF;     /* Primary Accent/Button */
  --brand-mint-glow: rgba(168, 230, 207, 0.4); /* Glow effect */
  --brand-lavender: #D4D0E8; /* AI/Intelligence Accent */
  --text-white: #FFFFFF;     /* Headlines */
  --text-muted: #CBD5E1;     /* Subheads (readable grey) */
  
  /* Fonts */
  --font-display: 'Montserrat', sans-serif; /* Headings */
  --font-body: 'Outfit', sans-serif;        /* Body text */
}
```

### 2. The Container & Background

- **Background Color:** Base is `--bg-navy`.
    
- **Lighting Effect (The Aurora):**
    
    - Create a `radial-gradient` circle positioned at `top center` (approx 50% horizontal, -20% vertical).
        
    - Color: A soft teal/slate blend fading into the dark navy.
        
    - _Purpose:_ To create a "spotlight" effect so the page isn't flat.
        
- **Texture (The Engineering Grid):**
    
    - Overlay a CSS pattern: A 40px by 40px square grid using 1px lines.
        
    - Color: White at 3% opacity (`rgba(255,255,255,0.03)`).
        
    - **Crucial:** Apply a `mask-image: linear-gradient(...)` so the grid is visible at the top but fades out completely by the bottom of the section. It should disappear into the void.
        

### 3. Typography & Copy Styling

**A. The Tagline (Top element)**

- **Text:** `OFFLINE-FIRST STRENGTH TRAINING`
    
- **Font:** `--font-body`, Weight: 600 (SemiBold).
    
- **Style:** Uppercase.
    
- **Color:** `--brand-mint`.
    
- **Spacing:** `letter-spacing: 2px` (Wide tracking for a technical look).
    
- **Size:** Small (e.g., 12px or 14px).
    

**B. The Headline (H1)**

- **Text:**
    
    - Line 1: "The Speed of a Tracker."
        
    - Line 2: "The Brain of a Coach."
        
- **Font:** `--font-display`, Weight: 700 (Bold).
    
- **Color Logic:**
    
    - "The Speed of a Tracker." = `--text-white`.
        
    - "The Brain of a Coach." = **Gradient Text**. Use a linear gradient from `--text-white` (start) to `--brand-mint` (end). `background-clip: text`.
        
- **Size:** Large (Desktop: 56px-64px / Mobile: 36px-42px).
    
- **Line Height:** Tight (1.1).
    

**C. The Subheadline (H2)**

- **Text:** "Shapyfy is the first offline-first strength tool that adapts to your progress. Get personalized AI recommendations without the loading screens."
    
- **Font:** `--font-body`, Weight: 400 (Regular).
    
- **Color:** `--text-muted`.
    
- **Size:** Readable (Desktop: 18px-20px / Mobile: 16px).
    
- **Max-Width:** Limit the width to roughly 500px on desktop so lines don't get too long to read.
    

### 4. The Primary CTA Button

- **Text:** "Request Alpha Access"
    
- **Font:** `--font-display`, Weight: 600 (SemiBold).
    
- **Shape:** Pill shape (Full rounded corners).
    
- **Colors:** Background: `--brand-mint`. Text: `--bg-navy` (Dark text on light button for max contrast).
    
- **The "Neon" Glow (Shadow):**
    
    - `box-shadow: 0px 0px 20px --brand-mint-glow`.
        
    - The button should look like it is emitting light.
        
- **Animation (The Shimmer):**
    
    - Create an `::after` pseudo-element (a white streak) that slides diagonally across the button every 4 seconds. This draws the eye.
        

### 5. The Visual Asset (Right Side)

_Since the 3D composition is complex, the developer will place a single high-fidelity image asset, but they must style its behavior._

- **The Asset:** A transparent PNG/WebP containing:
    
    - iPhone 15 Pro (Matte Black, Tilted 15°).
        
    - Screen showing Dark Mode UI + Numpad + Lavender AI Banner.
        
    - Floating Glass Card (Top Right): "New 1RM".
        
    - Floating Glass Card (Bottom Left): "Rest Timer".
        
- **Placement (Desktop):**
    
    - Align right.
        
    - Ensure the phone overlaps slightly with the "Aurora" light in the background.
        
- **Placement (Mobile):**
    
    - Center below text.
        
    - Scale down to fit width (approx 80-90% of screen width).
        
- **Animation (The Float):**
    
    - Apply a CSS animation to the entire image container: `transform: translateY()`.
        
    - Move it up and down by 15px over a 6-second loop (Ease-in-out).
        
    - _Effect:_ The phone is gently levitating in zero gravity.
        

### 6. Mobile Responsiveness Rules

- **Padding:** Ensure 24px padding on left/right for mobile screens.
    
- **Stacking Order:** Text First -> Button Second -> Image Last.
    
- **Navbar:** On mobile, ensure the Navbar (Logo) is fixed at the top with a "glass" blur background so it doesn't obscure the content when scrolling.
# Section 2 (The Problem)

Here is the final, locked-in documentation for **Section 2 (The Problem)**.

This matches the format of Section 1 perfectly. You can hand this directly to your developer alongside the previous file.

---

## Part 1: The Overview (For Humans)

**The Goal:** To agitate the user's pain points using a "Premium Dark Glass" aesthetic. We move from the airy Hero section into a grounded, serious environment that attacks the status quo of fitness apps.

**The Layout:**

- **Transition:** A smooth fade from the Hero's lighter Navy to a deeper, richer Navy.
    
- **Header:** Center-aligned text establishing the core conflict.
    
- **The Grid:** A row of 3 "Premium Glass Cards" that float on the dark background.
    

**The Content:**

- **Label:** `THE GYM REALITY`
    
- **Headline:** **"Smart apps need a signal. Dumb apps need a spreadsheet."**
    
- **Card 1 (Connectivity):** "The Infinite Load" – attacking loading spinners and bad gym Wi-Fi.
    
- **Card 2 (Friction):** "Manual Math" – attacking the mental load of Excel/Notes.
    
- **Card 3 (Stagnation):** "Passive Tracking" – attacking apps that don't offer guidance.
    

---

## Part 2: The Developer Specification (For Code)

**Instruction to Developer:** Build the "Problem Section" using the specifications below. The aesthetic is "Dark Glassmorphism." The cards must feel like physical slabs of high-tech material, not flat HTML divs.

### 1. Global Variables (Updates)

_Use the variables defined in Section 1, plus:_

CSS

```
:root {
  --bg-navy-deep: #1F222E;    /* Deeper background for Section 2 */
  --alert-coral: #FF9999;     /* The 'Problem' Color */
  --alert-coral-glow: rgba(255, 153, 153, 0.2);
  --glass-border: rgba(255, 255, 255, 0.08);
  --card-gradient: linear-gradient(145deg, rgba(58, 61, 82, 1) 0%, rgba(45, 49, 66, 1) 100%);
}
```

### 2. The Container & Transition

- **Background:** `--bg-navy-deep` (`#1F222E`).
    
- **The Transition (The "Fade Down"):**
    
    - Do not use a hard line between Section 1 and 2.
        
    - Apply a `background: linear-gradient(to bottom, var(--bg-navy) 0%, var(--bg-navy-deep) 150px);` to the top of this section.
        
    - _Effect:_ The user descends into a darker environment as they scroll.
        
- **Spacing:** `padding-top: 120px`, `padding-bottom: 120px`.
    

### 3. The Section Header (Centered)

- **Label (Top):**
    
    - Text: `THE GYM REALITY`
        
    - Font: `--font-body`, Uppercase, Bold (700).
        
    - Spacing: `letter-spacing: 3px`.
        
    - Color: `--brand-mint` at 60% opacity.
        
    - Size: 12px.
        
    - Margin: `margin-bottom: 16px`.
        
- **Headline (H2):**
    
    - Text: **"Smart apps need a signal. Dumb apps need a spreadsheet."**
        
    - Font: `--font-display`, Bold (700).
        
    - Color: `--text-white`.
        
    - Size: 42px (Desktop) / 32px (Mobile).
        
    - Layout: Max-width 700px, Centered.
        
    - Line-height: 1.2.
        

### 4. The Premium Cards (The Grid)

- **Layout:** CSS Grid (`grid-template-columns: repeat(3, 1fr)`). Gap: 24px.
    
- **Card Style (The "Dark Glass" Look):**
    
    - **Background:** `var(--card-gradient)`. _Do not use flat color._
        
    - **Border:** `1px solid var(--glass-border)`.
        
    - **Radius:** `16px`.
        
    - **Padding:** `40px 32px` (Tall and elegant).
        
    - **Shadow (Depth):** `box-shadow: 0px 10px 30px -10px rgba(0, 0, 0, 0.5)`.
        

### 5. Card Content & Icons

- **The Icon Container (The "Glowing Ember"):**
    
    - Size: 64px x 64px circle.
        
    - Background: `radial-gradient(circle, var(--alert-coral-glow) 0%, transparent 70%)`.
        
    - Border: `1px solid rgba(255, 153, 153, 0.1)`.
        
    - Icon Color: `--alert-coral` (`#FF9999`). Stroke width: 1.5px.
        
    - Margin: `margin-bottom: 24px`.
        
- **Headlines (H3):**
    
    - Text: "The Infinite Load", "Manual Math", "Passive Tracking".
        
    - Font: `--font-display`, SemiBold (600).
        
    - Size: 20px. Color: `--text-white`.
        
    - Margin: `margin-bottom: 12px`.
        
- **Body Text:**
    
    - Font: `--font-body`, Regular (400).
        
    - Size: 16px. Color: `--text-muted`.
        
    - Line-height: 1.6.
        

### 6. Interaction (Hover States)

- **Trigger:** Hover over the entire card.
    
- **Movement:** `transform: translateY(-8px)`. (Smooth ease-out transition 0.3s).
    
- **Lighting Change:**
    
    - Change border color to: `rgba(212, 208, 232, 0.4)` (Lavender hint).
        
    - Deepen shadow: `box-shadow: 0px 20px 50px -10px rgba(0, 0, 0, 0.7)`.
        
    - _Why:_ This subtle purple border signals that "Intelligence" (Lavender) is the answer to the problem.
        

### 7. Mobile Responsiveness

- **Layout:** Switch grid to 1 column (`grid-template-columns: 1fr`).
    
- **Spacing:** Reduce vertical gap to 16px.
    
- **Touch Targets:** Ensure the hover effect is disabled or subtle on touch devices to strictly avoid "sticky" hover states.

Here is the final, locked-in documentation for **Section 3: The Solution (The Bento Grid)**.

I have also included the **Founder's Note** immediately following it as **Section 4**. You can copy-paste this entire block to have the complete "Middle" of your landing page defined.

---

# Section 3: The Engine (The Solution)

## Part 1: The Overview (For Humans)

The Goal:

To reveal the "Engine" of Shapyfy. We move from the "Agitation" (Problem) into the "Solution." We use a high-tech dashboard layout to highlight the 3 pillars: Reliability, Speed, and Intelligence.

**The Layout:**

- **Header:** Left-aligned text.
    
- **The Grid:** A "Bento Box" layout with 3 cells.
    
    - **Cell 1 (Tall Left):** **"The Offline Core."** Focused on zero latency. (Mint Accents).
        
    - **Cell 2 (Top Right):** **"The Thumb Zone."** Focused on the Numpad. (Mint Accents).
        
    - **Cell 3 (Bottom Right):** **"The AI Brain."** Focused on suggestions. (Lavender Accents).
        

## Part 2: The Developer Specification (For Code)

### 1. Global Variables (Updates)

_Ensure these specific gradients are defined._

CSS

```
:root {
  --glass-dark: rgba(30, 34, 46, 0.7);
  --glass-border: rgba(255, 255, 255, 0.08);
  --lavender-border: rgba(212, 208, 232, 0.3);
  --mint-gradient: linear-gradient(to top, rgba(168, 230, 207, 0.1), transparent);
}
```

### 2. Section Container

- **Background:** `--bg-navy-deep` (`#1F222E`).
    
- **Spacing:** `padding-top: 50px`, `padding-bottom: 120px`.
    

### 3. The Header (Left Aligned)

- **Headline (H2):**
    
    - Text: **"Engineered for flow."**
        
    - Font: `--font-display`, Bold. Color: `--text-white`. Size: 48px.
        
- **Subhead:**
    
    - Text: "Everything you need to get stronger. Nothing to slow you down."
        
    - Font: `--font-body`, Regular. Color: `--text-muted`. Size: 18px.
        
    - Max-width: 500px.
        
    - Margin: `margin-bottom: 48px`.
        

### 4. The Bento Grid Layout

- **Desktop:**
    
    - `display: grid;`
        
    - `grid-template-columns: 1.5fr 1fr;`
        
    - `grid-template-rows: 280px 280px;`
        
    - `gap: 24px;`
        
- **Mobile:** Stacked column (`gap: 16px`).
    

### 5. The Cells (Detailed Design)

#### **Cell A: The Core (Spans 2 Rows on Left)**

- **Position:** `grid-column: 1 / 2; grid-row: 1 / 3;`
    
- **Styling:** Dark Glass background (`backdrop-filter: blur(10px)`). Border: `var(--glass-border)`.
    
- **Content (Top Left):**
    
    - _Label:_ `OFFLINE FIRST` (Small Caps, Mint Color).
        
    - _Headline:_ **Zero Latency.**
        
    - _Body:_ "Local-first architecture means instant saves. No loading spinners, ever."
        
- **Visual (Bottom):**
    
    - _Effect:_ A **Mint Green Gradient** (`--mint-gradient`) fading up from the bottom.
        
    - _Icon:_ A large, semi-transparent "Database" icon turning into a "Checkmark" in the center.
        

#### **Cell B: The Input (Top Right)**

- **Position:** `grid-column: 2 / 3; grid-row: 1 / 2;`
    
- **Styling:** Dark Glass. Border: `var(--glass-border)`.
    
- **Content (Top Left):**
    
    - _Label:_ `FAST INPUT` (Small Caps, Mint Color).
        
    - _Headline:_ **Custom Numpad.**
        
- **Visual (Bottom Right):**
    
    - _Asset:_ A cropped screenshot of the **Shapyfy Numpad**.
        
    - _Highlight:_ The "Next" button in the screenshot must be bright **Mint Green**.
        

#### **Cell C: The Brain (Bottom Right)**

- **Position:** `grid-column: 2 / 3; grid-row: 2 / 3;`
    
- **Styling:** Dark Glass with **Lavender Tint**.
    
    - _Border:_ `var(--lavender-border)`.
        
    - _Background:_ `linear-gradient(145deg, rgba(212, 208, 232, 0.05), rgba(31, 34, 46, 1))`.
        
- **Content (Top Left):**
    
    - _Label:_ `INTELLIGENCE` (Small Caps, Lavender Color).
        
    - _Headline:_ **Auto-Regulation.**
        
- **Visual (Bottom Right):**
    
    - _Asset:_ A cropped screenshot of a **"Suggestion Banner"** (e.g., "Increase load +2.5kg?").
        
    - _Color:_ The UI element in the shot should be **Lavender**.
        

---

# 📝 Section 4: The Founder's Note (The Connection)

## Part 1: The Overview

The Goal:

To build trust through vulnerability. This section breaks the "high-tech" flow with a personal, editorial moment.

**The Layout:**

- **Split Screen:** Founder Photo (Left) + Personal Letter (Right).
    
- **Aesthetic:** Quiet, Human, Gritty (Black & White photography).
    

## Part 2: The Developer Specification

### 1. Section Container

- **Background:** `--bg-navy` (`#2D3142`). (Lighter than previous section).
    
- **Spacing:** `padding-top: 100px`, `padding-bottom: 100px`.
    

### 2. The Layout

- **Desktop:** `grid-template-columns: 1fr 2fr; gap: 64px; align-items: center;`
    
- **Mobile:** Stacked column.
    

### 3. The Photo (Left Side)

- **Asset:** [User's Photo - provided].
    
- **Filter:** `grayscale(100%)` (Black & White).
    
- **Desktop Shape:** Rounded Rectangle (`border-radius: 24px`). Aspect Ratio: 4/5.
    
- **Mobile Shape:** Circle (`border-radius: 50%`). Width: 160px.
    
- **Border:** `2px solid rgba(168, 230, 207, 0.2)` (Faint Mint ring).
    

### 4. The Letter (Right Side)

- **Headline:**
    
    - Text: **"For 3 years, my progress was trapped in a spreadsheet."**
        
    - Font: `--font-display`, Bold, 32px, White.
        
- **Body Text:**
    
    - Font: `--font-body`, Regular, 18px, Color: `--text-muted`, Line-height: 1.6.
        
    - _Content:_ "I was stuck in Excel, just noting and switching digits. It felt like data entry, not training. Those numbers didn't tell me a story—they didn't know when I needed to push or when I needed to recover."
        
    - _Content:_ "I built Shapyfy to fix that. I wanted the speed and freedom of a notepad, but with the intelligence of a coach. I wanted a tool that could take those 'dumb' numbers and turn them into a clear path forward."
        
    - _Content:_ "I use it for every single workout. It's the tool I wish I had three years ago."
        
- **Signature:**
    
    - Name: **[Your Real Name]** (Bold, White).
        
    - Title: **Founder & Lifter** (Regular, **Mint Green**).
        

---

**That concludes the documentation for the "Solution" and "Founder" sections.**

Once you save this, the Landing Page is officially Ready for Code.

Shall we switch gears to The Mobile App: The Workout Screen Wireframe?

# 🚀 Section 5: The Invitation (Final CTA)

## Part 1: The Overview

**The Goal:** To convert the visitor into an Alpha Tester. This section focuses purely on the email input. It should look like a "Portal" or a "Ticket" to an exclusive club.

**The Visual:** A large, glowing "Glass Card" floating in the center of the screen. It separates itself from the background, demanding attention.

**The Content:**

- **Headline:** "Build the future of strength."
    
- **Subhead:** "We are opening limited spots for Alpha testing. Secure your place in the queue."
    
- **Action:** Email Input + "Join Waitlist" Button.
    

## Part 2: The Developer Specification

### 1. Section Container

- **Background:** `--bg-navy-deep` (`#1F222E`).
    
- **Spacing:** `padding-top: 100px`, `padding-bottom: 120px`.
    
- **The "Glow" Effect:**
    
    - Place a large, circular **Mint Green radial gradient** (`#A8E6CF` at 15% opacity) strictly behind the CTA card.
        
    - _Effect:_ The card looks like it is backlit by a neon light.
        

### 2. The CTA Card (The "Portal")

- **Layout:** Centered, Max-width 800px.
    
- **Styling:**
    
    - **Background:** `rgba(45, 49, 66, 0.6)` (Dark semi-transparent).
        
    - **Backdrop Filter:** `blur(20px)` (Heavy frosted glass).
        
    - **Border:** `1px solid rgba(168, 230, 207, 0.3)` (Mint border).
        
    - **Radius:** `24px`.
        
    - **Padding:** `80px 40px` (Desktop) / `40px 24px` (Mobile).
        

### 3. The Typography

- **Headline (H2):**
    
    - Text: **"Build the future of strength."**
        
    - Font: `--font-display`, Bold, 48px, Color: White.
        
    - Alignment: Center.
        
- **Subhead:**
    
    - Text: "We are accepting a limited number of Alpha testers. Help us shape the ultimate offline training tool."
        
    - Font: `--font-body`, Regular, 18px, Color: `--text-muted`.
        
    - Alignment: Center.
        
    - Margin: `margin-bottom: 40px`.
        

### 4. The Input Form (Crucial for Conversion)

- **Layout:** Flex Row (Input + Button). Centered.
    
    - _Mobile:_ Stacked Column (Input on top, Button below).
        
- **The Input Field:**
    
    - **Background:** `rgba(0,0,0,0.3)`.
        
    - **Border:** `1px solid rgba(255,255,255,0.1)`.
        
    - **Text Color:** White.
        
    - **Placeholder:** "Enter your email" (Color: `#666`).
        
    - **Height:** `56px`.
        
    - **Width:** `300px` (Desktop) / `100%` (Mobile).
        
    - **Radius:** `12px`.
        
- **The Button:**
    
    - **Text:** **Join the Waitlist**
        
    - **Background:** `--brand-mint` (`#A8E6CF`).
        
    - **Text Color:** `--bg-navy` (`#2D3142`) -> _High Contrast._
        
    - **Font:** Bold.
        
    - **Height:** `56px`.
        
    - **Radius:** `12px`.
        
    - **Hover State:** Transform `scale(1.05)` and `box-shadow: 0px 0px 20px rgba(168, 230, 207, 0.4)`.
- # 🦶 Section 6: The Footer (Final)

## Part 1: The Overview

**The Goal:** Minimalist but compliant. We include the mandatory legal links required for App Store approval.

**The Layout:** A clean Flexbox row.

- **Left:** Brand Identity.
    
- **Center:** Legal Links (Terms & Privacy).
    
- **Right:** Social Icons.
    

## Part 2: The Developer Specification

### 1. Section Container

- **Background:** Transparent.
    
- **Border Top:** `1px solid rgba(255, 255, 255, 0.05)` (Subtle separator).
    
- **Spacing:** `padding: 40px 0`.
    
- **Width:** Max-width 1200px (centered).
    

### 2. Content Layout (Flexbox)

- **Desktop:** `display: flex; justify-content: space-between; align-items: center;`
    
- **Mobile:** Stacked column. `flex-direction: column; gap: 32px; text-align: center;`
    

### 3. Left Side (Identity)

- **Logo Text:** **Shapyfy** (Bold, White, 18px).
    
- **Copyright:** "© 2025 Shapyfy." (Small, `--text-muted`, 14px).
    

### 4. Center (Legal Compliance)

- **Layout:** Row with `gap: 24px`.
    
- **Links:**
    
    1. **Terms & Conditions**
        
    2. **Privacy Policy**
        
- **Typography:**
    
    - Font: `--font-body`, Regular.
        
    - Size: 14px (Small but legible).
        
    - Color: `--text-muted` (Grey).
        
- **Interaction:**
    
    - Default: `text-decoration: none`.
        
    - Hover: Color changes to **White**. `transition: color 0.2s`.
        

### 5. Right Side (Connect)

- **Icons:** X (Twitter), Instagram, Email (mailto).
    
- **Style:** Simple vector icons (SVG). White fill.
    
- **Opacity:** Default 60%. Hover 100% + Color `--brand-mint`.


---
AFTER Off bonus 

---
# 🧬 Shapyfy Landing Page: The Master Specification

**Version:** 1.0 (Final) **Theme:** Premium Dark / Glassmorphism / Mint & Lavender **Core Philosophy:** "Offline Reliability meets AI Intelligence."

---

## 0. Global Design System (The "Physics")

**Instruction to Developer:** The site relies on specific "Materials" (Glass, Neon, Matte Metal). Define these tokens globally first.

CSS

```
:root {
  /* -- PALETTE -- */
  --bg-navy: #2D3142;         /* Standard Background (Hero, Founder) */
  --bg-navy-deep: #1F222E;    /* Deep Background (Problem, Solution, CTA) */
  --brand-mint: #A8E6CF;      /* Primary Action / Speed / Success */
  --brand-mint-glow: rgba(168, 230, 207, 0.4);
  --brand-lavender: #D4D0E8;  /* AI / Intelligence / Context */
  --alert-coral: #FF9999;     /* Problem / Pain Points */
  --text-white: #FFFFFF;      /* Headings */
  --text-muted: #CBD5E1;      /* Body Text */

  /* -- FONTS -- */
  --font-display: 'Montserrat', sans-serif; /* Headings (Weight: 600/700) */
  --font-body: 'Outfit', sans-serif;        /* Body (Weight: 300/400) */

  /* -- MATERIALS (GLASS) -- */
  --glass-bg: rgba(30, 34, 46, 0.6);
  --glass-border: 1px solid rgba(255, 255, 255, 0.08);
  --glass-blur: blur(20px);
  
  /* -- SHADOWS -- */
  --shadow-float: 0px 20px 50px -10px rgba(0, 0, 0, 0.5);
  --shadow-glow-mint: 0px 0px 40px rgba(168, 230, 207, 0.15);
}
```

---

## 1. Section 1: The Hero (The Hook)

**Concept:** Airy, Floating, Premium. **Background:** `--bg-navy` with a subtle "Aurora" teal gradient top-center and a fading "Engineering Grid" overlay.

### A. The Layout

- **Desktop:** Split 50/50. Text (Left), Visual (Right).
    
- **Mobile:** Vertical Stack. Visual centered below text. CTA sticky bottom.
    

### B. The Copy & Typography

1. **Tagline (Top):** `OFFLINE-FIRST STRENGTH TRAINING`
    
    - _Style:_ Small Caps, `--brand-mint`, Tracking: 2px.
        
2. **Headline (H1):**
    
    - Line 1: **The Speed of a Tracker.** (White).
        
    - Line 2: **The Brain of a Coach.** (Linear Gradient: White → Mint).
        
    - _Font:_ `--font-display`, Bold, 64px.
        
3. **Subhead (H2):** "Shapyfy is the first offline-first strength tool that adapts to your progress. Get personalized AI recommendations without the loading screens."
    
    - _Style:_ `--text-muted`, Regular, 18px.
        
4. **CTA Button:** **[ Request Alpha Access ]**
    
    - _Style:_ Pill shape, Mint Background, Dark Text.
        
    - _Effect:_ CSS "Shimmer" animation (white streak moves across every 4s).
        

### C. The Visual Asset (Crucial)

**The 3D Composition:**

- **Device:** iPhone 15 Pro (Matte Black Titanium), Tilted 15° Left, 10° Back.
    
- **Screen Content:** **High-Res Looped VIDEO/GIF.**
    
    - _Action:_ User taps "100" on the Numpad -> Taps "Done" -> Row turns Green instantly.
        
    - _Message:_ Proves the "Speed" claim.
        
- **Environment:**
    
    - **Backlight:** Soft Mint rim light behind the phone.
        
    - **Floating Elements:** Two Glass Cards hovering next to the phone.
        
        - _Card 1 (Top Right):_ "🏆 New 1RM"
            
        - _Card 2 (Bottom Left):_ "Rest Timer: 01:30"
            

---

## 2. Section 2: The Problem (The Agitation)

**Concept:** Grounded, Heavy, Serious. **Background:** Transition to `--bg-navy-deep`. **Transition:** Smooth linear gradient fade from Section 1.

### A. Header

- **Label:** `THE GYM REALITY` (Centered, Mint opacity 60%).
    
- **Headline:** **"Smart apps need a signal. Dumb apps need a spreadsheet."** (Centered, White, Bold).
    

### B. The 3 Glass Cards (Grid Layout)

- **Material:** Dark Gradient Background (`linear-gradient(145deg)`), Glass Border.
    
- **Hover Effect:** Lift (`translateY(-8px)`), Shadow Deepens, Border turns faint Lavender.
    

#### Card 1: Connectivity

- **Icon:** `Signal Slash` (Stroke: 1.5px, Color: `--alert-coral`).
    
- **Icon Glow:** Radial gradient of Coral Pink behind icon.
    
- **Title:** **The Infinite Load**
    
- **Body:** "Gym basements are dead zones. Most AI apps leave you staring at a spinning wheel. We are 100% local."
    

#### Card 2: Friction

- **Icon:** `Spreadsheet Grid` (Color: `--alert-coral`).
    
- **Title:** **Manual Math**
    
- **Body:** "Notes apps offer freedom, but no guidance. Calculating warm-ups in your head breaks your focus. We do the math."
    

#### Card 3: Stagnation

- **Icon:** `Flatline Graph` (Color: `--alert-coral`).
    
- **Title:** **Passive Tracking**
    
- **Body:** "Basic loggers just record your plateaus. We analyze your history to prescribe the exact load for growth."
    

---

## 3. Section 3: The Solution (The Bento Engine)

**Concept:** High-Tech Dashboard. Structured. **Background:** `--bg-navy-deep`.

### A. Header (Left Aligned)

- **Headline:** **Engineered for flow.**
    
- **Subhead:** "Everything you need to get stronger. Nothing to slow you down."
    

### B. The Bento Grid (CSS Grid)

- **Desktop:** `grid-template-columns: 1.5fr 1fr; grid-template-rows: 280px 280px;`
    

#### Cell 1: The Core (Tall, Left)

- **Style:** Dark Glass. Mint Bottom Gradient.
    
- **Text:** `OFFLINE FIRST` / **Zero Latency.** / "Local-first architecture. No loading spinners, ever."
    
- **Visual:** Abstract 3D Graphic: A Database Cylinder morphing into a Green Checkmark.
    

#### Cell 2: The Input (Top Right)

- **Style:** Dark Glass.
    
- **Text:** `FAST INPUT` / **Custom Numpad.**
    
- **Visual:** **UI Crop.** A macro shot of the Shapyfy Numpad.
    
    - _Detail:_ The "Done" button is bright Mint Green.
        

#### Cell 3: The Brain (Bottom Right)

- **Style:** Dark Glass + **Lavender Tint** + Lavender Border.
    
- **Text:** `INTELLIGENCE` / **Auto-Regulation.**
    
- **Visual:** **UI Crop.** A macro shot of the AI Suggestion Banner ("Increase load +2.5kg?").
    

---

## 4. Section 4: The Founder's Note (The Connection)

**Concept:** Editorial, Human, Gritty. **Background:** Return to lighter `--bg-navy`. **Layout:** Asymmetric Split (Photo Left / Text Right).

### A. The Photo

- **Asset:** User's Photo (Teams.jpg).
    
- **Treatment:** **Black & White Filter.** High Contrast.
    
- **Styling:** Rounded Rectangle (`border-radius: 24px`).
    
- **Accent:** Thin Mint Green ring/border (`1px solid rgba(168, 230, 207, 0.3)`).
    

### B. The Letter

- **Headline:** **"For 3 years, my progress was trapped in a spreadsheet."** (Serif or Bold Sans).
    
- **Body:** "I was stuck in Excel, noting and switching digits. It felt like data entry, not training. Those numbers didn't tell me a story... I built Shapyfy because I wanted the speed of a notepad with the intelligence of a coach... I use it for every single workout."
    
- **Sign-off:**
    
    - Name: **[Your Name]** (Bold White).
        
    - Title: Founder & Lifter (Mint Green).
        

---

## 5. Section 5: The CTA (The Invitation)

**Concept:** Exclusive Portal. Scarcity. **Background:** `--bg-navy-deep`.

### A. The Environment

- **Glow:** A massive Mint Green radial gradient positioned _behind_ the CTA card.
    

### B. The Glass Card

- **Badge (Top Right):** `FOUNDING MEMBER OFFER` (Capsule, Mint Border, Mint Text).
    
- **Headline:** **Build the future of strength.**
    
- **Subhead:** "Join the Alpha today and **lock in 70% OFF for life**. Help us shape the ultimate offline training tool."
    

### C. The Scarcity Counter (Interactive)

- **Placement:** Above the input field.
    
- **Visual:** 🔥 **412 / 500 Alpha Spots Taken** (Small, Bold).
    
- _Animation:_ The number '412' creates a subtle "pulse" effect.
    

### D. The Input

- **Field:** Transparent background, White Border. Placeholder: "Enter email".
    
- **Button:** **[ Join the Waitlist ]** (Solid Mint, Dark Text).
    
- **Success State:** On click -> Card contents fade out -> "Ticket" graphic fades in confirming the 70% discount.
    

---

## 6. Section 6: The Footer (Legal)

**Concept:** Minimalist Compliance. **Background:** Transparent. Top Border Separator.

### Layout (Flexbox)

1. **Left:** **Shapyfy** Logo + "© 2025. Offline First."
    
2. **Center:** [Terms & Conditions] | [Privacy Policy] (Links turn white on hover).
    
3. **Right:** Social Icons (X, Instagram, Mail). White SVGs at 60% opacity.
    

---

## 📂 Visual Assets Checklist (For Designer)

_You need to create these 7 specific files:_

1. **Hero_Video_Loop.mp4:** iPhone mockup showing the Numpad flow (fast speed).
    
2. **Card_Icon_Wifi.svg:** Coral Pink stroke.
    
3. **Card_Icon_Grid.svg:** Coral Pink stroke.
    
4. **Card_Icon_Graph.svg:** Coral Pink stroke.
    
5. **Bento_Graphic_Database.png:** Abstract Mint "Database/Checkmark" 3D render.
    
6. **Bento_UI_Numpad_Crop.png:** High-res crop of the Numpad UI.
    
7. **Bento_UI_AI_Crop.png:** High-res crop of the AI Recommendation Banner.