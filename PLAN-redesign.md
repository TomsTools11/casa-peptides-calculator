# Plan: Redesign Peptide Calculator to Match casapeptides.shop

## Context

The CASA Peptides Protocol Planner (`peptide-calculator.html`) currently uses a dark, blue-accented tech aesthetic. The Casa Peptides website (casapeptides.shop) uses a warm, luxury-clinical light theme with earthy tones — cream/beige backgrounds, warm tan accents (`#d4ba99`), and muted natural colors. This redesign swaps the entire visual skin to match the website while keeping all functionality identical.

**File:** `/Users/tpanos/TProjects/current-projects/casa-peptide-calculator/peptide-calculator.html` (single file, ~1145 lines)

---

## Design Direction

**From:** Dark navy backgrounds, bright blue accent, neon category colors, Geist body font
**To:** Warm white/cream backgrounds, tan/mauve accent (`#d4ba99`), muted earthy category colors, Inter body font

### New Color Palette

| Role | Current | New |
|------|---------|-----|
| Page bg | `#0f1117` | `#ffffff` |
| Card bg | `#181b28` | `#ffffff` |
| Input bg | `#1e2235` | `#faf6f1` |
| Elevated bg | `#20243a` | `#f5efe8` |
| Secondary bg | `#1a1d27` | `#faf6f1` |
| Border | `#2a2f4a` | `#d4ba99` |
| Text primary | `#ffffff` | `#222222` |
| Text secondary | `#b8bdd8` | `#50473e` |
| Text muted | `#6b7280` | `#6d5f53` |
| Primary accent | `#407EC9` (blue) | `#d4ba99` (warm tan) |
| Accent hover | `#4f8fd4` | `#9a7e5e` |
| Accent glow | `rgba(64,126,201,0.15)` | `rgba(212,186,153,0.15)` |
| Success/Recovery | `#4ade80` / `#22c55e` | `#5a8a5e` / `#4a7a4e` |
| Fat Loss/Warning | `#fb923c` / `#fbbf24` | `#c47a2a` |
| Hormone/Purple | `#a78bfa` | `#b8927a` (dusty rose) |
| Cognitive/Teal | `#2dd4bf` | `#6b9e8a` (muted sage) |
| Error | `#f87171` | `#c44e49` |
| Pink | `#f472b6` | `#c4977e` |
| Shadow | `0 4px 24px rgba(0,0,0,0.4)` | `0 1px 3px rgba(0,0,0,0.08), 0 4px 12px rgba(0,0,0,0.04)` |

### Font Change
- Body font: `Geist` → `Inter` (Red Hat Display stays for headings — already used on both)

---

## Implementation Steps

### Step 1: Google Fonts link (line 9)
Replace `Geist:wght@400;500;600` with `Inter:wght@400;500;600;700` in the font import URL.

### Step 2: CSS Custom Properties (lines 11-24)
Replace the entire `:root` block with new warm palette values. This single change recolors ~70% of the UI since most styles use `var(--*)`.

New `:root` block:
```css
:root {
  --bg-primary:#ffffff; --bg-secondary:#faf6f1; --bg-elevated:#f5efe8;
  --bg-card:#ffffff; --bg-input:#faf6f1; --color-border:#d4ba99;
  --text-primary:#222222; --text-secondary:#50473e; --text-muted:#6d5f53;
  --blue:#d4ba99; --blue-hover:#9a7e5e; --blue-glow:rgba(212,186,153,0.15);
  --green:#5a8a5e; --green-dim:#4a7a4e; --teal:#6b9e8a;
  --amber:#c47a2a; --red:#c44e49; --purple:#b8927a; --pink:#c4977e;
  --orange:#c47a2a;
  --font-display:'Red Hat Display',system-ui,sans-serif;
  --font-body:'Inter',system-ui,sans-serif;
  --radius-sm:6px; --radius-md:10px; --radius-lg:16px; --radius-xl:20px;
  --shadow-card:0 1px 3px rgba(0,0,0,0.08), 0 4px 12px rgba(0,0,0,0.04);
  --tr:200ms ease;
}
```

### Step 3: CSS rules with hardcoded rgba() (lines 32-166)
Update all `rgba()` values in CSS rules that bypass custom properties. Key areas:
- `.header-badge` border → warm tan rgba
- `.step-circle.active` box-shadow → warm tan glow
- `.icon-*` backgrounds → muted earthy rgba versions
- `.peptide-card` hover/selected → warm tan rgba
- `.cat-*` category badge backgrounds → earthy rgba
- `input:focus` box-shadow → warm tan
- `.pill-*` status pills → muted earthy rgba
- `.summary-card` gradient → warm tan/rose gradient
- `.btn-purple` → warm mauve colors (`#b8927a`, hover `#9a7e5e`)
- `.btn-green:hover` → `#3d6b41`
- `.tip`, `.conc-badge` → warm tinted backgrounds
- `.modal-overlay` → lighter opacity (`rgba(0,0,0,.45)`) for light theme
- Separator colors: `rgba(255,255,255,.04/.05)` → `rgba(0,0,0,.06)` (dark-on-light)

### Step 4: Inline styles in static HTML (lines 168-295)
- Logo text `color:#fff` → `color:#222222` (lines 175-176)
- Protocol settings bar rgba blues → warm tan rgba (line 237)
- Protocol note purple border-left → warm mauve (line 253)

### Step 5: JS template literals — inline styles (lines 473-601)
Update hardcoded colors in these functions (work function-by-function):
- **`buildMixPanel()`**: Microdose card glow, icon, SVG (6× `#a78bfa` → `#b8927a`), toggle OFF state grays (`#6b7280` → `#9a8e82`, `rgba(107,114,128` → `rgba(109,95,83`)
- **`syringeSVG()`** (line 577): All hardcoded fills/strokes — dark fills (`#2a2f4a` → `#e8ddd0`), blue strokes (`#407EC9` → `#d4ba99`), barrel bg (`#1e2235` → `#faf6f1`), liquid fill (`rgba(64,126,201,.55)` → `rgba(212,186,153,.55)`), tick marks/text (`#6b7280` → `#9a8e82`), needle tip (`#b8bdd8` → `#9a8e82`)
- **`toggleMicrodose()`**: ON state purples → warm mauve (`#a78bfa` → `#b8927a`), OFF state grays → warm grays
- **`mixUpdate()`**: Microdose info bar purples → warm mauve
- **`renderProtocol()`**: Day/week toggle separator (`rgba(255,255,255,.1)` → `rgba(0,0,0,.08)`), microdose banner purples
- **`renderOrderCard()`**: Selected row greens → muted sage, checkout hover (`#16a34a` → `#3d6b41`, `rgba(34,197,94,.4)` → `rgba(90,138,94,.3)`)
- **`openCardInTab()`** (line 1084-1096): Window bg `#111` → `#faf6f1`, button colors (`.dl` green → `#4a7a4e`, `.cp` dark → warm light)

### Step 6: Canvas share card (lines 911-1097)
Update `generateShareCard()` and related functions:
- Canvas background: keep `#f4efe6` or adjust to `#faf6f1`
- Logo/top bar color: `#0f1117` → `#222222`
- Subtitle/meta text: `#6b7280`/`#9ca3af` → `#6d5f53`/`#9a8e82`
- Category color map: `{'cat-recovery':'#5a8a5e','cat-fat':'#c47a2a','cat-hormone':'#b8927a','cat-cognitive':'#6b9e8a'}`
- Peptide name/stat text: `#111827` → `#222222`
- Accent fallback: `#407EC9` → `#d4ba99`
- Microdose tag: `rgba(167,139,250,.2)` → `rgba(184,146,122,.2)`, `#a78bfa` → `#b8927a`
- All `"Geist"` font references → `"Inter"` (8 occurrences in canvas/footer)

---

## Find-and-Replace Reference (rgba patterns)

| Find | Replace |
|------|---------|
| `rgba(64,126,201` | `rgba(212,186,153` |
| `rgba(167,139,250` | `rgba(184,146,122` |
| `rgba(34,197,94` | `rgba(90,138,94` |
| `rgba(74,222,128` | `rgba(90,138,94` |
| `rgba(251,146,60` | `rgba(196,122,42` |
| `rgba(251,191,36` | `rgba(196,122,42` |
| `rgba(248,113,113` | `rgba(196,78,73` |
| `rgba(244,114,182` | `rgba(196,151,126` |
| `rgba(45,212,191` | `rgba(107,158,138` |
| `rgba(107,114,128` | `rgba(109,95,83` |
| `rgba(124,58,237` | `rgba(154,126,94` |
| `rgba(255,255,255,.04)` | `rgba(0,0,0,.06)` |
| `rgba(255,255,255,.05)` | `rgba(0,0,0,.06)` |
| `rgba(255,255,255,.1)` | `rgba(0,0,0,.08)` |

## Hex Color Replacements

| Find | Replace | Context |
|------|---------|---------|
| `#a78bfa` | `#b8927a` | Hardcoded purple hex |
| `#7c3aed` | `#b8927a` | Purple button bg |
| `#6d28d9` | `#9a7e5e` | Purple button hover |
| `#407EC9` | `#d4ba99` | Blue accent (in SVG) |
| `#0f1117` | `#222222` | Dark navy (in canvas) |
| `#1e2235` | `#faf6f1` | Input bg (in SVG) |
| `#2a2f4a` | `#e8ddd0` | Border/fill dark (in SVG) |
| `#b8bdd8` | `#9a8e82` | Light text (in SVG needle) |
| `#6b7280` | `#9a8e82` | Muted text (in inline/JS only, not `:root`) |
| `#22c55e` | `#4a7a4e` | Green dim (canvas, openCardInTab) |
| `#16a34a` | `#3d6b41` | Green hover hex |
| `#9ca3af` | `#9a8e82` | Gray text (canvas) |
| `#111827` | `#222222` | Dark text (canvas) |
| `#fb923c` | `#c47a2a` | Orange hex (canvas color map) |
| `#2dd4bf` | `#6b9e8a` | Teal hex (canvas color map) |
| `color:#fff` (logo) | `color:#222222` | Logo text (lines 175-176) |
| `background:#111` | `background:#faf6f1` | openCardInTab bg |
| `"Geist"` | `"Inter"` | Font references in JS (8 occurrences) |
| `'Geist'` | `'Inter'` | Font reference in CSS var |

---

## Verification

1. Open `peptide-calculator.html` in browser
2. **Step 0 (Choose)**: Warm white bg, cream cards, tan selection border, muted category badge colors
3. **Step 1 (Mix)**: Tabs use warm tan active state, syringe SVG has warm colors, microdose toggle animates between warm mauve and neutral
4. **Step 2 (Protocol)**: Frequency toggle uses warm tan, tiles readable on light bg, microdose banners use mauve
5. **Step 3 (Track)**: Expiry bars use muted green/amber/red, order toggles work, summary card has warm gradient
6. **Share Card**: Modal opens with semi-transparent overlay, canvas renders with warm palette, correct fonts
7. **Text contrast**: `#222222` on `#ffffff` and `#50473e` on `#faf6f1` are readable
8. **All buttons**: Hover states produce visible effect on light background
