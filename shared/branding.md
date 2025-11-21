
## 1. Shapyfy Brand Palette – Final Structure

Top-level structure:

- **Brand core hues**
    - Shapyfy Green (brand mint + functional primary)
    - Coral (secondary accent)
    - Lavender (tertiary / info)
- **Neutrals**
- **Semantic colors**
    - Success, Warning, Error, Info

I’ll include a short “usage rule” for each.

---

## 2. Brand Core Hues

### 2.1 Shapyfy Green – Primary

We distinguish **brand mint** (light) from **functional primary** (dark), but they’re one family.

**Functional Primary (UI)**
- Hex: **#2D7A5F**
- Role:
    - Primary buttons
    - Key icons
    - Links / key interactive elements
    - Main “progress” color in charts when strong contrast is needed
- Notes:
    - Treat this as the **main primary** for functional UI.
    - Works on light backgrounds; for dark backgrounds, add white text.

**Brand Mint (Light Primary)**
- Hex: **#A8E6CF**
- Role:
    - Background tints (cards, sections)
    - Soft progress indicators, tags, chips
    - Illustration fills and soft highlights
- Notes:
    - Don’t use as text on white/light.
    - Works great as a “brand mood” color in marketing visuals.

**Soft Mint Variant (Disabled / Subtle)**
- Hex: **#C1DFD1**
- Role:
    - Disabled buttons with green context
    - Very subtle backgrounds in green sections
- Notes:
    - Lower contrast/energy version of mint.

**Gradient / Highlight Mint**
- Hex: **#CBFFC9**
- Role:
    - Secondary stop in gradients
    - Very soft backgrounds in marketing
- Notes:
    - Use sparingly; can easily become “too pastel” if overused.

---

### 2.2 Coral – Secondary Accent

Used both as **secondary accent** and part of **warning semantics**.

**Core Coral Accent**
- Hex: **#FF9999**
- Role:
    - Secondary buttons (where not critical)
    - Highlights in charts
    - Supporting icons / tags
    - Decorative elements in illustrations (e.g. small shapes, secondary elements)
- Notes:
    - This is *not* error. It is warm, playful brand accent.

**Light Coral**
- Hex: **#FFB3BA**
- Role:
    - Soft backgrounds for coral-accented components
    - Subtle chips / tags
- Notes:
    - Also used as **warning background** (see semantics).

**Soft Coral**
- Hex: **#FFC4C8**
- Role:
    - Very soft accents, backgrounds
- Notes:
    - Use in marketing, cards, subtle UI states.

---

### 2.3 Lavender – Tertiary / Info

**Dark Lavender**
- Hex: **#B8A5D9**
- Role:
    - Info icons
    - Tertiary accent elements (less important than green/coral)
- Notes:
    - Use for “structure/brain” visuals, not “effort/strength”.

**Medium Lavender**
- Hex: **#D4D0E8**
- Role:
    - Subtle backgrounds
    - Info tags

**Light Lavender**
- Hex: **#E8E3F3**
- Role:
    - Info background
    - Very subtle panels, cards, sections

---

## 3. Neutrals

Simplified and clarified as you requested.

**White**
- Hex: **#FFFFFF**
- Role:
    - Cards
    - Main surfaces in light UI
    - Landing backgrounds

**Page Background**
- Hex: **#F3F3F8**
- Role:
    - Default app and web background
- Notes:
    - Slight lavender tint to connect with brand.

**Surface**
- Hex: **#F0F0F5**
- Role:
    - Elevated surfaces, secondary cards, side panels

**Border**
- Hex: **#E5E5E5**
- Role:
    - Dividers
    - Input borders
    - Card outlines

**Disabled Neutral**
- Hex: **#CCCCCC**
- Role:
    - Neutral disabled states (when not specifically tied to mint)

**Text – Primary**
- Hex: **#1A1A1A**
- Role:
    - Body text
    - Headings
- Notes:
    - Use this instead of pure black in almost all cases.

**Text – Secondary**
- Hex: **#666666**
- Role:
    - Secondary text
    - Labels, less-emphasized descriptions

**Text – Tertiary / Placeholder**
- Hex: **#999999**
- Role:
    - Placeholder text
    - Very low emphasis text

(We keep `#2D2D2D` out of the core spec for now to simplify – if you need it later as an intermediate, we can re-introduce it.)

---

## 4. Semantic Colors

### 4.1 Success

**Success Accent**
- Hex: **#2D7A5F** (functional primary)
- Role:
    - Success icons
    - Text representing success state
- Notes:
    - Reuse the main green as success accent to keep system consistent.

**Success Background**
- Hex: **#D4F4DD**
- Role:
    - Background for success banners, badges.

**Success Light Background**
- Hex: **#F0FFF0**
- Role:
    - Very subtle success context (e.g. “goal achieved” subtle highlight).

---

### 4.2 Warning (with Coral as part of it)

We keep coral integrated but add a clearer “amber” tone to avoid confusion with error.

**Warning Icon / Accent**
- Hex: **#FFB86C** (amber)
- Role:
    - Warning icons
    - Key warning indicators (exclamation marks, badges)

**Warning Background (Coral-based)**
- Hex: **#FFB3BA**
- Role:
    - Warning background for banners/panels

**Warning Border / Text (Coral Dark)**
- Hex: **#FF9999**
- Role:
    - Borders and text inside warning elements

Notes:
- This makes **amber (#FFB86C)** the main “danger-ish but not error” accent.
- Coral remains strongly tied to brand but also clearly signals “pay attention” when combined with amber.

---

### 4.3 Error

**Error Accent**
- Hex: **#FF6B6B**
- Role:
    - Destructive actions (delete)
    - Error icons

**Error Dark**
- Hex: **#E05A5A**
- Role:
    - Error text
    - Stronger emphasis in error components

**Error Background (Implied)**
- Suggestion: `#FFECEC` (not originally given, but simple to standardize)
    - Role:
        - Error background for banners etc.

We can add that explicitly if you want a full spec.

---

### 4.4 Info

**Info Accent**
- Hex: **#B8A5D9** (dark lavender)
- Role:
    - Info icons
    - Info emphasis text

**Info Background**
- Hex: **#E8E3F3**
- Role:
    - Info banners
    - Info cards

---

## 5. Quick “Branding Table” View

Here’s a compact table-style summary you can reuse in docs.

### Brand Core

| Token                    | Hex      | Role                                                                 |
|--------------------------|----------|----------------------------------------------------------------------|
| Primary / UI functional  | #2D7A5F  | Primary buttons, key icons, main actions, strong progress           |
| Brand Mint               | #A8E6CF  | Brand accent, soft progress, backgrounds, illustrations             |
| Mint Soft                | #C1DFD1  | Disabled green states, subtle backgrounds                           |
| Mint Highlight           | #CBFFC9  | Gradient stop, very soft backgrounds                                |
| Coral Core               | #FF9999  | Secondary buttons, charts, accent icons                             |
| Coral Light              | #FFB3BA  | Soft backgrounds, tags, warning backgrounds                         |
| Coral Soft               | #FFC4C8  | Very subtle accents, marketing backgrounds                          |
| Lavender Dark            | #B8A5D9  | Info icons, tertiary accents                                        |
| Lavender Medium          | #D4D0E8  | Info-tag backgrounds, subtle surfaces                               |
| Lavender Light           | #E8E3F3  | Info backgrounds, subtle panels                                     |

### Neutrals

| Token                | Hex      | Role                                      |
|----------------------|----------|-------------------------------------------|
| White                | #FFFFFF  | Cards, primary surfaces                   |
| Background           | #F3F3F8  | Default app/web background                |
| Surface              | #F0F0F5  | Secondary surfaces, side panels           |
| Border               | #E5E5E5  | Dividers, outlines                        |
| Disabled Neutral     | #CCCCCC  | Neutral disabled elements                 |
| Text Primary         | #1A1A1A  | Main text, headings                       |
| Text Secondary       | #666666  | Secondary text, labels                    |
| Text Tertiary        | #999999  | Placeholder, very low emphasis            |

### Semantic

| Type    | Token           | Hex      | Role                                      |
|---------|-----------------|----------|-------------------------------------------|
| Success | Success Accent  | #2D7A5F  | Success icons/text (re-use primary)       |
|         | Success BG      | #D4F4DD  | Success background                        |
|         | Success BG Light| #F0FFF0  | Very subtle success context               |
| Warning | Warning Accent  | #FFB86C  | Warning icons/accent                      |
|         | Warning BG      | #FFB3BA  | Warning background                        |
|         | Warning Text    | #FF9999  | Warning text/borders                      |
| Error   | Error Accent    | #FF6B6B  | Error icons, destructive actions          |
|         | Error Dark      | #E05A5A  | Error text, stronger error                |
| Info    | Info Accent     | #B8A5D9  | Info icons/text                           |
|         | Info BG         | #E8E3F3  | Info backgrounds                          |
