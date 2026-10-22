---
name: Civic Restraint
colors:
  surface: '#faf9f6'
  surface-dim: '#dbdad7'
  surface-bright: '#faf9f6'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f4f3f0'
  surface-container: '#efeeeb'
  surface-container-high: '#e9e8e5'
  surface-container-highest: '#e3e2df'
  on-surface: '#1b1c1a'
  on-surface-variant: '#494740'
  inverse-surface: '#2f312f'
  inverse-on-surface: '#f2f1ee'
  outline: '#7a776f'
  outline-variant: '#cbc6bd'
  surface-tint: '#605e5b'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#1c1b19'
  on-primary-container: '#868380'
  inverse-primary: '#cac6c2'
  secondary: '#625e57'
  on-secondary: '#ffffff'
  secondary-container: '#e6ded6'
  on-secondary-container: '#67625b'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#002110'
  on-tertiary-container: '#499268'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#e6e2de'
  primary-fixed-dim: '#cac6c2'
  on-primary-fixed: '#1c1b19'
  on-primary-fixed-variant: '#484644'
  secondary-fixed: '#e9e1d9'
  secondary-fixed-dim: '#ccc5bd'
  on-secondary-fixed: '#1e1b16'
  on-secondary-fixed-variant: '#4a4640'
  tertiary-fixed: '#a7f3c3'
  tertiary-fixed-dim: '#8cd6a8'
  on-tertiary-fixed: '#002110'
  on-tertiary-fixed-variant: '#005230'
  background: '#faf9f6'
  on-background: '#1b1c1a'
  surface-variant: '#e3e2df'
typography:
  amount-display:
    fontFamily: Inter
    fontSize: 40px
    fontWeight: '600'
    lineHeight: 48px
    letterSpacing: -0.03em
  headline-lg:
    fontFamily: Inter
    fontSize: 26px
    fontWeight: '600'
    lineHeight: 34px
    letterSpacing: -0.015em
  headline-sm:
    fontFamily: Inter
    fontSize: 20px
    fontWeight: '600'
    lineHeight: 28px
    letterSpacing: -0.01em
  title-md:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '600'
    lineHeight: 24px
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: 26px
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: 22px
  body-sm:
    fontFamily: Inter
    fontSize: 13px
    fontWeight: '400'
    lineHeight: 20px
  label-caps:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '600'
    lineHeight: 16px
    letterSpacing: 0.06em
  label-nav:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '500'
    lineHeight: 14px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  gutter: 1rem
  margin: 1.5rem
  space-xs: 0.25rem
  space-sm: 0.5rem
  space-md: 1rem
  space-lg: 1.5rem
  space-xl: 2.5rem
---

## Brand & Style

This design system is engineered specifically for residential society administration, operational maintenance, and civic oversight in urban Indian housing contexts (such as Mumbai housing cooperative societies). The operational aesthetic rejects modern fintech gamification, playful illustrations, synthetic micro-animations, or heavy surface decoration. Instead, it embodies a quiet, utilitarian permanence—akin to a well-kept physical register, municipal signage, or a crisp printed receipt.

### Personality & Emotional Response
- **Sober & Honest:** Eliminates decorative anxiety. Residents viewing maintenance fees, water alerts, or committee minutes encounter absolute clarity and zero distractions.
- **Durable & Civic:** Prioritizes high legibility, strict contrast, and resilient vertical structures that comfortably host multilingual scripts (Devanagari for Hindi and Marathi alongside Latin scripts).
- **Direct Accountability:** Conveys immediate operational status without euphemistic visual flourishes. Success is silent and dark green; urgent alerts are grounded, un-saturated brick red.

### Aesthetic Methodology
The aesthetic style is strictly **Flat Modern Utility**:
- Zero drop shadows, zero ambient glow, zero blurred layers, zero skeuomorphic pill fills, and zero gradients.
- Visual hierarchy relies exclusively on 1px structural rules, distinct type weights, and calibrated empty space.
- No rounded chip tags, no badge count pills with dropshadows, no nested card-within-card enclosures, and strictly no non-standard iconography or emojis.

## Colors

The palette employs a natural, tactile substrate reminiscent of raw parchment or archival register paper, grounded by carbon black ink and subdued functional accents.

### Palette Architecture
- **Primary Ink (`#1C1B19`):** Carbon black for primary text, primary button fills, key numerals, and high-emphasis indicators. Provides near-total contrast against the background without the clinical harshness of `#000000`.
- **Secondary Ink (`#6F6A63`):** Subdued stone-grey for metadata, secondary labels, timestamps, flat table keys, and helper text. Ensures comfortable typographic hierarchy without competing with primary data.
- **Substrate / Background (`#F7F6F3`):** Warm off-white base layer applied uniformly across all screen surfaces. There are no secondary card surface tokens; screens remain flat, relying on 1px hairline rules for separation.
- **Structural Hairlines (`#E4E0D8`):** Restrained neutral-tinted border tone used exclusively for 1px row dividers, header baselines, and top sheet borders.
- **Alert / Overdue / Rejected (`#9C3B2E`):** Deep iron-oxide red. Used deliberately and sparingly for unpaid penalties, overdue maintenance notices, urgent water cuts, or denied approvals.
- **Verified / Confirmed / Success (`#1F6B45`):** Deep forest green. Applied only to confirm reconciled transactions, active society memberships, and cleared invoices.

### Color Rules
- Surfaces do not stack tinted elevations. Modals, bottom sheets, and viewports share `#F7F6F3`.
- No colored backgrounds behind text chips or alert containers. Alerts are rendered as plain text alongside a 1px `#9C3B2E` accent line or simple inline text label.

## Typography

Typography prioritizes clarity, vertical rhythm, and horizontal compactness. Inter is deployed across headlines, body copy, and figures, selected for its tall x-height, neutral letterforms, and robust glyph support that renders effortlessly alongside native Android Devanagari system typefaces (Noto Sans Devanagari) for Marathi and Hindi strings.

### Key Roles & Layout Applications
- **Amount Display (40px/48px SemiBold):** The focal anchor of the resident experience. Used to render dues (e.g., `₹ 14,250`). Paired with a reduced-size rupee symbol or native prefix, avoiding all ornamentation.
- **Headline Large (26px/34px SemiBold):** Page-level titles for committee dashboards, society roster registries, and bill histories.
- **Label Caps (11px/16px SemiBold Uppercase):** Category markers, receipt voucher metadata keys (e.g., `DUE DATE`, `SOCIETY REG NO`, `FLAT NUMBER`).
- **Body Large & Medium (16px and 14px):** Line heights are intentionally generous (+8px to +10px over font size) to prevent collision with Devanagari vowel marks (*matras*) and conjuncts (*samyuktakshars*).

## Layout & Spacing

The layout is built for a 390×844 portrait canvas (targeting modern high-density Android devices like the Google Pixel series). The system adheres to a single-column, baseline-driven layout anchored by fixed side margins.

### Boundary & Flow Rules
- **Side Padding (`margin`):** Strictly `24px` (`1.5rem`) on both left and right screen borders. Content never bleeds to the glass edge except for structural 1px dividers that extend the full viewport width when separating major screen chapters.
- **Top Safe Zone:** Status bar height (24px to 48px dynamic) followed by a 24px top screen margin.
- **Vertical Hierarchy:**
  - Amount displays are preceded by `space-xl` (`40px`) and followed by `space-md` (`16px`) to establish deliberate typographic breathing room.
  - Section blocks are separated by `space-xl` (`40px`).
  - Interior data rows (label-value pairs) utilize `space-md` (`16px`) vertical padding above and below dividing borders.

### Device Responsiveness
- While optimized for 390px widths, widths exceeding 480px maintain the 24px side padding while centering content within a maximum structural column of 440px to retain vertical reading posture.

## Elevation & Depth

This system operates entirely in flat two-dimensional space. Elevation is expressed via coordinate sequencing and 1px border demarcation, never through shadows, blurs, or tinted surface rises.

### Zero-Elevation Policy
- **No Drop Shadows:** No `box-shadow` or Android `elevation` attributes exist. All `elevation` tokens evaluate to `0dp`.
- **No Blurs or Glass:** Sheets, sticky headers, and navigation bars use 100% opaque `#F7F6F3`. 
- **1px Structural Hairlines:** Separation is created exclusively by 1px solid lines tinted to `#E4E0D8`. Sticky top app bars and bottom navigation docks are pinned directly above and below content using single 1px hairline borders (`border-bottom: 1px solid #E4E0D8` or `border-top: 1px solid #E4E0D8`).
- **Surface Containment:** Elements that traditionally sit inside floating cards instead sit between horizontal top and bottom dividers directly on the canvas substrate.

## Shapes

The design system maintains an architectural, near-orthogonal geometric discipline. Shapes are functional enclosures rather than decorative flourishes.

### Enclosure Philosophy
- **Standard Radius (8px):** Applied strictly to the single primary interactive button (`border-radius: 8px`) and text input frames. This prevents harsh corners on touch targets without sliding into casual or bubbly roundedness.
- **Zero Radius (0px):** All list rows, alert callouts, header containers, tab panels, and bottom navigation frames use `0px` radius, terminating flat against horizontal partition rules.
- **No Pills:** Chips, tab selectors, and status indicators do not use pill (`9999px`) geometries.

## Components

### 1. Buttons & Interaction Tiers
- **Primary Button:**
  - Dimensions: Full width (`100%`), height `48px`.
  - Style: Background `#1C1B19`, label text `#F7F6F3`, weight 600 (15px), corner radius `8px`.
  - Behavior: Active state reduces opacity to `0.85`. No drop shadow on press.
  - Frequency: Maximum of **one** filled button per screen.
- **Secondary Action (Text Link Only):**
  - Second-order actions (e.g., "View Detailed Breakdown", "Download Receipt", "Cancel") must **never** be rendered as outlined, tinted, or secondary buttons.
  - Render as plain text links: Font size 14px, weight 500, color `#1C1B19`, with an active underline or plain hit target. Placed centrally below the primary button with a vertical separation of `16px`.

### 2. Lists & Flat Data Rows
- Replaces cards entirely.
- Sits flat on `#F7F6F3`.
- Bounded by 1px `#E4E0D8` top and bottom lines.
- Left-aligned title (`#1C1B19`, 15px) stacked above secondary label (`#6F6A63`, 13px). Right-aligned numerical value or status term.
- Vertical padding within list rows is strictly `16px`.

### 3. Text Fields & Forms
- Height `48px`, background transparent, border `1px solid #E4E0D8`, corner radius `8px`.
- Padding horizontal `16px`.
- Active focus state: Border color switches directly to `#1C1B19` (1.5px width). No glow or halo rings.
- Label sits directly above the field in `label-caps` (`#6F6A63`), never as floating animated placeholder text.

### 4. Checkboxes & Radio Selectors
- Checkbox: 18×18px square, 2px corner radius. Inactive: `1px solid #6F6A63`. Selected: Filled `#1C1B19` with `#F7F6F3` check.
- Radio: 18×18px circle. Inactive: `1px solid #6F6A63`. Selected: `#1C1B19` outer border with a solid 8px centered carbon dot.

### 5. Status Indicators (No Badges/Pills)
- Status is conveyed through raw text and alignment.
- Overdue maintenance: Rendered as `#9C3B2E`, 13px SemiBold uppercase text (e.g., `OVERDUE BY 12 DAYS`).
- Confirmed/Paid: Rendered as `#1F6B45`, 13px SemiBold uppercase text (e.g., `PAID ON 04 OCT`).

### 6. Navigation Archetypes

#### Archetype 1: Resident View
- **Bottom Navigation Bar:** Exactly 4 tabs (`Home`, `Bills`, `Help`, `Notices`). Height `56px`. Flat `#F7F6F3` background with a continuous `1px solid #E4E0D8` top divider.
- Tab style: Unselected labels in `#6F6A63` (11px). Active tab indicated by `#1C1B19` label with a 2px carbon black indicator line anchored to the top hairline divider.
- Hero module: High-prominence Amount Due (e.g., `₹ 12,400`), a single full-width primary button labeled "Pay Now", and a solitary text link below: "View Past Receipts".

#### Archetype 2: Managing Committee View
- **No Bottom Tabs.** Pure linear management mode.
- Top header: 1px hairline border at base. Left-aligned society title and wing/unit filter in 16px SemiBold.
- Content body: Flat ledger list of flats, maintenance compliance rows, and member requests, with immediate one-tap text actions (`Approve`, `Inquire`).

#### Archetype 3: FacilityCore Staff & Operations View
- Top status header displaying active society name and terminal role switcher.
- Dense single-column flat list for Society Roster, Guard Gate Logs, and Role Provisioning.
- High-contrast 1px toggles to immediately grant or revoke committee-level credentials without confirmation dialog layers.