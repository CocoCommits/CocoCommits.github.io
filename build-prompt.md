# Portfolio Build Prompt — Neha Bendale

Build a complete single-file portfolio website: `index.html` with all CSS and JS inline. No frameworks, no build tools. Must deploy directly to Cloudflare Pages as a static file.

---

## File Structure

```
index.html          — single file, all CSS and JS inline
assets/
  light/            — light theme screenshots (jpg)
  dark/             — dark theme screenshots (jpg)
  photo.jpg         — profile photo (placeholder if missing)
  resume.pdf        — resume download (placeholder if missing)
```

---

## Tech Constraints

- Pure HTML + CSS + vanilla JS only
- No React, Vue, Tailwind, Bootstrap, or any CDN framework
- Google Fonts via `<link>` in `<head>`: Inter + Playfair Display (Latin subset)
- All images: `loading="lazy"`
- No flash of wrong theme: apply theme class in an inline `<script>` in `<head>` before body renders
- Must pass Lighthouse mobile score > 90
- Must work on Chrome, Safari, Firefox

---

## Theme System

### Auto-detection + toggle
```js
// In <head> before body — prevents flash
const saved = localStorage.getItem('theme');
const system = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
document.documentElement.setAttribute('data-theme', saved || system);
```

- Toggle is a pill-shaped slider in nav top-right with sun/moon icons
- On toggle: flip `data-theme` on `<html>`, save to `localStorage`
- All colors via CSS custom properties scoped to `[data-theme="light"]` and `[data-theme="dark"]`
- Screenshot images swap src on theme change: `data-light="assets/light/x.jpg"` and `data-dark="assets/dark/x.jpg"`

### Light Theme Tokens
```css
[data-theme="light"] {
  --bg-primary:    #FAF8F4;
  --bg-secondary:  #F0EDE6;
  --bg-accent:     #E8E4D9;
  --text-primary:  #1A1A1A;
  --text-secondary:#5A5A5A;
  --text-muted:    #9A9A8A;
  --accent:        #7A8C6E;
  --accent-hover:  #5C6E52;
  --accent-light:  #C8D4BE;
  --border:        #E0DDD6;
  --shadow:        rgba(0,0,0,0.06);
}
```

### Dark Theme Tokens
```css
[data-theme="dark"] {
  --bg-primary:    #141412;
  --bg-secondary:  #1E1E1B;
  --bg-accent:     #28281F;
  --text-primary:  #F0EDE6;
  --text-secondary:#A8A89A;
  --text-muted:    #6A6A5A;
  --accent:        #8FA882;
  --accent-hover:  #A8C09A;
  --accent-light:  #2A3326;
  --border:        #2E2E28;
  --shadow:        rgba(0,0,0,0.3);
}
```

---

## Typography

```css
/* Hero / section headings */
font-family: 'Playfair Display', Georgia, serif;

/* All UI, body, nav, pills */
font-family: 'Inter', system-ui, sans-serif;
```

| Role | Font | Weight | Desktop | Mobile |
|---|---|---|---|---|
| Hero name | Playfair Display | 700 | 56px | 36px |
| Section headings | Playfair Display | 600 | 32px | 26px |
| Card titles | Inter | 600 | 20px | 18px |
| Body | Inter | 400 | 16px | 15px |
| Meta/captions | Inter | 400 | 13px | 12px |
| Pill tags | Inter | 500 | 12px | 11px |
| Impact numbers | Playfair Display | 700 | 48px | 32px |
| Nav links | Inter | 500 | 14px | 14px |

---

## Visual Style

| Property | Value |
|---|---|
| Max content width | 1100px centered |
| Card border radius | 16px |
| Pill border radius | 999px |
| Button border radius | 8px |
| Card hover | translateY(-4px) + deeper shadow |
| Transition | 200ms ease on all interactive elements |
| Section padding | 80px 0 desktop / 48px 0 mobile |

---

## Page Sections — Build in This Order

### 1. STICKY NAV
- Left: "NB" monogram or "Neha Bendale" text logo
- Center: Work · About · Contact (smooth scroll anchors)
- Right: [Resume ↗] button + theme toggle slider
- Desktop: full horizontal bar, `position: sticky; top: 0; z-index: 100`
- Mobile: name left + hamburger right; hamburger opens full-screen overlay menu
- Background: `var(--bg-accent)` with 1px bottom border `var(--border)`
- Add subtle backdrop-filter blur on scroll

### 2. HERO
- Desktop: 2-col split, left text right photo, min-height 90vh
- Left col: tag line in small caps accent color → H1 name in Playfair → role subline → tagline paragraph → 2 CTA buttons
  - Primary CTA: "See My Work" (scrolls to #projects), filled accent bg
  - Secondary CTA: "Download Resume" (links to assets/resume.pdf), outlined
- Right col: profile photo, circular crop, subtle ring border in accent color
- Mobile: single column, photo centered on top, text below
- Subtle animated gradient orb in background (CSS only, no canvas)

### 3. IMPACT STRIP
- Full-width band, `var(--bg-secondary)` background
- 4 stats in a row (desktop) / 2x2 grid (mobile)
- Each stat: large Playfair number + Inter label below

| Number | Label |
|---|---|
| 1.2B+ | Annual Transactions |
| 90M+ | Users Served |
| 6 Years | in Fintech |
| 4 | Regulated Partners |

- Numbers animate count-up when scrolled into view (IntersectionObserver + vanilla JS)

### 4. ABOUT
- id="about"
- Desktop: 2-col, left bio text, right photo
- Left: section label (small caps) → H2 heading → bio paragraph → skills pills below
- Right: photo with rounded corners, slight rotation (-2deg), accent border
- Bio text:
  > Product Manager with 6 years across product engineering and product management in payments, lending, and financial ecosystems at Bajaj Finserv. I own systems processing 1.2B+ annual transactions for 90M+ users. I specialise in B2B2C platform products, API-led ecosystem integrations, and 0-to-1 product launches — working across engineering, design, risk, compliance, and commercial stakeholders. Currently building an AI-native personal wealth platform in private beta.
- Mobile: photo full width top, bio below

### 5. SKILLS
- Flowing pill badges, grouped by category
- Each category has a small label above its pills
- Pills: `var(--accent-light)` bg, `var(--accent)` text, 999px radius
- Categories and skills:

| Category | Skills |
|---|---|
| Payments & FinTech | Payment Gateway · Checkout · UPI · BBPS · Co-Lending · Settlement |
| Compliance & Risk | KYC · Audit Trail · RBI Compliance · Risk Controls · Regulatory Reporting |
| AI & Emerging Tech | LLMs · Generative AI · AI Agents · Prompt Engineering |
| Technical | API Design · REST APIs · SQL · Data Analytics · Mixpanel |
| Product Management | PRD · OKRs & KPIs · 0-to-1 · Roadmaps · User Stories |
| Leadership | Cross-functional Teams · Partner Ecosystems · Executive Communication |

- Mobile: entire section horizontally scrollable as a single row of pills

### 6. PROJECTS
- id="projects"
- Section heading: "Work" in Playfair
- Desktop: 2-column card grid
- Mobile: horizontal swipe carousel — card width 85vw, CSS scroll-snap, next card peeks at right edge

#### Project Card Structure (each card)
```
┌─────────────────────────────────────────────┐
│ [TAG1] [TAG2] [TAG3]              Company·Yr │
│                                              │
│ Project Title                                │
│ One-line summary                             │
│                                              │
│ • Outcome bullet 1                           │
│ • Outcome bullet 2                           │
│                                              │
│ ──────────────────────────────────────────── │
│ 📈 Metric 1    💡 Metric 2    👥 Metric 3    │
│                                              │
│ [Read more ↓]                                │
└─────────────────────────────────────────────┘
```

- Card bg: `var(--bg-secondary)`, border: 1px `var(--border)`, radius 16px
- Hover: translateY(-4px) + shadow deepens, 200ms ease
- Tags: small pills in `var(--accent-light)`
- Metrics row: 3 stat chips with icon + value, separated by subtle divider
- "Read more" expands an inline detail panel below the card (accordion, not modal)

#### Expanded detail panel (on "Read more")
- Slides open below card with CSS max-height transition
- For Bajaj Finserv cards: full description text + additional bullet points
- For NEESH and NEESHSECRETS cards: stacked screenshot viewer (see below) + full feature list
- "Close" button at bottom of panel

#### 6 Project Cards Content

**Card 1 — Unified Enterprise Platform**
- Tags: Enterprise · Platform · 0-to-1
- Title: Unified Enterprise Platform
- Company/Year: Bajaj Finserv · 2025–Present
- Summary: Consolidating 40+ fragmented enterprise apps into one platform for 100,000+ employees.
- Bullets: Defined API contracts and shared data models · Shaped roadmap using usage analytics and KPI dashboards
- Metrics: 40+ Apps · 100K+ Users · 25%+ SLA Improvement

**Card 2 — Co-Lending Partner Platform**
- Tags: API Platform · Regulated FinTech · B2B
- Title: Strategic Co-Lending & Partner Platform
- Company/Year: Bajaj Finserv · 2024–2025
- Summary: Owned REST API contracts and KYC verification gates across 4 regulated partner institutions.
- Bullets: Step-by-step onboarding: spec review → KYC gate → sandbox → compliance sign-off · Zero regulatory escalations across all active integrations
- Metrics: 40% Faster Onboarding · 60% Less Engineering Effort · Zero Escalations

**Card 3 — Device Financing**
- Tags: Lending · Risk · 0-to-1
- Title: Device Financing: 0-to-1 Product
- Company/Year: Bajaj Finserv · 2023–2024
- Summary: 3-tier CIBIL scoring model with graduated MDM enforcement based on Days-Past-Due thresholds.
- Bullets: Signal-based risk tiers: approve / MDM controls / reject · Analyst tooling used by 300+ store ops teams
- Metrics: 60% NPA Reduction · 40% Faster Investigations · 45% Fewer Escalations

**Card 4 — Payments Platform**
- Tags: Consumer Payments · Scale · UPI
- Title: Payments Platform & Checkout Optimization
- Company/Year: Bajaj Finserv · 2021–2023
- Summary: Owned full consumer payments stack — UPI, BBPS, wallets, EMI — for 90M+ users across 25+ partners.
- Bullets: Managed SLA contracts, routing logic, and NPCI/BBPS regulatory reporting · Segmentation-driven collections nudges
- Metrics: 25% Auth Improvement · 35% NPS Lift · 10Cr+ Monthly Transactions

**Card 5 — NEESH**
- Tags: AI · Self-Built · Live · 0-to-1
- Title: NEESH — Family Wealth Tracker
- Company/Year: Independent · Live Beta
- Summary: Self-hosted personal finance tracker for Indian families. 11 asset classes, live NSE/BSE/AMFI feeds, XIRR, LTCG/STCG auto-computation, AI statement import.
- Bullets: Transaction engine: buy/sell/bonus/split/dividend → auto holding recomputation · AI import: upload statements, AI extracts transactions
- Metrics: 11 Asset Classes · 20-User Beta · AI-Powered Import
- Live link badge: "Live — Private Beta" linking to https://neesh.pages.dev
- Expanded panel: stacked screenshot viewer with theme-matched images

**Card 6 — NEESHSECRETS**
- Tags: Security · Self-Built · Live · Zero-Knowledge
- Title: NEESHSECRETS — Family Vault
- Company/Year: Independent · Live
- Summary: Zero-knowledge self-hosted password manager. AES-256-GCM in browser. Server is completely blind to plaintext.
- Bullets: Family inheritance scheme: structured vault access for trusted family recovery · Per-user private vaults, fully offline instant search
- Metrics: AES-256-GCM · Zero Cloud · Family Inheritance Scheme
- Live link badge: "Live" linking to https://neeshsecrets.share.zrok.io/
- Expanded panel: stacked screenshot viewer with theme-matched images

---

### 7. STACKED SCREENSHOT VIEWER

Used only in expanded panels for Card 5 and Card 6.

```
Visual at rest:
  ┌──────────────────────┐  ← card 1 (top, full opacity)
   ┌──────────────────────┐ ← card 2 (6px down, scale 0.97)
    ┌──────────────────────┐← card 3 (12px down, scale 0.94)

On click/tap:
  card 1 slides left and fades out
  card 2 animates to top position
  dot indicator advances
```

- Container: `position: relative`, fixed height
- Each image: `position: absolute`, `border-radius: 12px`, `box-shadow`
- z-index stack: top card highest
- CSS transitions: `transform 300ms ease, opacity 300ms ease`
- Dot indicators: row of small circles below stack, filled = current
- Mobile: swipe left gesture also advances (touch event listener)
- JS function `advanceStack(stackId)` cycles cards and updates dots
- On theme change: swap all `src` attributes using `data-light` / `data-dark`

Image files:
```
NEESH dark:    assets/dark/neesh_01_dashboard.jpg
               assets/dark/neesh_02_portfolio.jpg
               assets/dark/neesh_03_holdings.jpg
               assets/dark/neesh_04_analytics.jpg
               assets/dark/neesh_05_feature.jpg

NEESH light:   assets/light/neesh_01_dashboard.jpg
               assets/light/neesh_02_analytics.jpg
               assets/light/neesh_03_portfolio.jpg
               assets/light/neesh_04_holdings.jpg
               assets/light/neesh_05_feature.jpg

Secrets dark:  assets/dark/secrets_01_list.jpg
               assets/dark/secrets_02_detail.jpg

Secrets light: assets/light/secrets_01_login.jpg
               assets/light/secrets_02_search.jpg
```

---

### 8. EXPERIENCE TIMELINE
- id="experience" — linked from nav as part of About
- Desktop: horizontal scrollable timeline, overflow-x auto, snap scrolling
- Mobile: vertical stacked list
- Each node: dot on line → year range → role title → company + location
- Current role dot: filled accent color; past roles: outlined

| Period | Role | Company |
|---|---|---|
| May 2025–Present | Product Manager II — Platform Modernization | Bajaj Finserv, Bangalore |
| Mar 2024–Apr 2025 | Product Manager II — Co-Lending Platform | Bajaj Finserv, Pune |
| Feb 2023–Feb 2024 | Product Manager II — Device Financing | Bajaj Finserv, Pune |
| Sep 2021–Jan 2023 | Product Manager — Payments Platform | Bajaj Finserv, Pune |
| Jul 2020–Aug 2021 | Software Engineer → Product Manager | Bajaj Finserv, Pune |

---

### 9. ACHIEVEMENTS
- Small strip between timeline and contact
- 3 achievement chips in a row (desktop) / stacked (mobile)
- Each chip: trophy icon + bold text
  - A+ rating · 3 consecutive years
  - 10+ awards from MD and CTO
  - Overseas Incentive Award · 60% above target

---

### 10. CONTACT
- id="contact"
- Clean centered section, dark accent bg band
- Heading: "Let's talk."
- Subtext: "Open to PM roles, collaborations, and interesting problems."
- 3 links in a row (desktop) / stacked (mobile):
  - Email: nehabendale308@gmail.com
  - LinkedIn: https://www.linkedin.com/in/nehabendale/
  - Location: Bangalore, India
- Resume download button: "Download Resume" → assets/resume.pdf
- Education + certs in small text at bottom:
  - B.Tech Electronics & Telecommunications, COEP Pune
  - Google Cloud: Generative AI Learning Path
  - IBM AI Product Management
- Footer: "Built by Neha Bendale · 2025"

---

## JavaScript Modules (all inline in index.html)

### 1. Theme system
```js
// In <head>: no-flash theme init
// In body: toggle handler, localStorage, image src swap on theme change
```

### 2. Count-up animation
```js
// IntersectionObserver on impact strip
// Animate from 0 to target value over 1200ms when scrolled into view
// Handle + suffix (1.2B+, 90M+)
```

### 3. Mobile hamburger menu
```js
// Toggle full-screen overlay nav
// Close on link click or outside tap
```

### 4. Project card expand/collapse
```js
// Click "Read more" → toggle max-height on detail panel
// Rotate chevron icon
// Smooth CSS transition (max-height: 0 → max-height: 800px)
```

### 5. Stacked screenshot viewer
```js
function advanceStack(stackId) {
  // Move top card to bottom of stack
  // Update z-index, transform, opacity for all cards
  // Update dot indicators
}
// Touch: swipeStart / swipeEnd with 50px threshold
// Theme change: swap all data-light / data-dark src
```

### 6. Mobile project carousel
```js
// CSS scroll-snap handles most of it
// Optional: dot indicators synced to scroll position via IntersectionObserver
```

### 7. Smooth scroll
```js
// All nav anchor links use smooth scroll
// Offset for sticky nav height (~64px)
```

---

## Accessibility

- All images have descriptive `alt` text
- Nav links have `aria-label`
- Theme toggle has `aria-label="Toggle theme"` and `role="switch"`
- Color contrast ratio >= 4.5:1 for all text on backgrounds
- Focus-visible styles on all interactive elements
- Skip-to-content link at top of page

---

## Placeholder Handling

If `assets/photo.jpg` does not exist, show a styled initials avatar: "NB" in accent color on accent-light background.

If `assets/resume.pdf` does not exist, hide the resume download button gracefully (do not show a broken link).

For Bajaj Finserv project cards (Cards 1–4): no screenshot images — use a styled gradient banner instead, using the accent color palette, with the company name and role title as text overlay.

---

## Output

Produce a single file: `index.html`
- Fully self-contained
- All CSS in a `<style>` block in `<head>`
- All JS in a `<script>` block before `</body>` except the no-flash theme snippet which goes in `<head>`
- No external dependencies except Google Fonts `<link>`
- Ready to drop into a Cloudflare Pages repo and deploy
