# CASA Peptides Protocol Planner

A comprehensive, single-file web application for planning peptide dosing protocols — from peptide selection and reconstitution math through injection scheduling, vial tracking, and shareable protocol cards. Built for [CASA Peptides](https://casapeptides.com).

**Live App →** [casa-peptides-calculator.vercel.app](https://casa-peptides-calculator.vercel.app)

---

## Features

- **119+ Peptide Library** — Extensive catalog spanning six categories: Recovery & Healing, Fat Loss & Metabolic, Hormone & Growth, Cognitive & Neuro, Sexual Health & Fertility, and Specialty & Anti-Aging. Multiple vial sizes per peptide where available.
- **4-Step Guided Workflow** — Choose → Mix → Protocol → Track stepper interface walks users through the entire protocol planning process.
- **Reconstitution Calculator** — Computes concentration (mcg/unit), draw units per dose, syringe fill level, and doses per vial based on user-configurable BAC water volume and vial size.
- **Microdose Toggle** — Halves each injection dose and doubles frequency, keeping the same weekly total. Carries through all downstream calculations.
- **Protocol Scheduling** — Per-peptide frequency controls (per-day or per-week), total injection counts, vials-needed estimates, and reorder date projections for configurable cycle lengths.
- **Titration Schedules** — Smart BAC water calculation and titration schedule support for gradual dose ramp-up.
- **Vial Expiry Tracker** — Date-based tracking for reconstituted vial shelf life.
- **Order Cart** — Per-vial pricing with direct checkout links to casapeptides.com.
- **Shareable Protocol Card** — Canvas-rendered branded PNG image of your full protocol, downloadable or shareable.
- **Dark / Light Theme** — Toggle between a dark mode and a warm, luxury-clinical light theme. Preference persists via `localStorage`.
- **Grouped Size Selection** — Peptide size variants are collapsed into a single card on the selection screen, with an in-mix vial size switcher for quick changes.
- **Fully Responsive** — Optimized for desktop and mobile with breakpoints at 540px and 500px.

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Markup & Logic** | Single-file HTML (`index.html`) — HTML + CSS + vanilla JavaScript |
| **Frameworks** | None — zero dependencies, no build system, no package manager |
| **Fonts** | [Red Hat Display](https://fonts.google.com/specimen/Red+Hat+Display) (headings) + [Inter](https://fonts.google.com/specimen/Inter) (body) via Google Fonts |
| **Hosting** | [Vercel](https://vercel.com) |
| **Dev Tooling** | [Claude Code](https://claude.ai/code) (AI-assisted development) |

---

## Getting Started

### Prerequisites

Any modern web browser (Chrome, Firefox, Safari, Edge). That's it — no Node.js, no build tools, no dependencies.

### Installation

```bash
# Clone the repository
git clone https://github.com/TomsTools11/casa-peptides-calculator.git

# Navigate into the project
cd casa-peptides-calculator
```

### Running Locally

Simply open `index.html` in your browser:

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

Or use any local static server:

```bash
# Python
python3 -m http.server 8000

# Node (if you have npx)
npx serve .
```

Then visit `http://localhost:8000` in your browser.

---

## Usage

1. **Choose** — Browse the peptide library by category. Select one or more peptides (size variants are grouped into single cards).
2. **Mix** — For each selected peptide, configure vial size, BAC water volume, and dose. The calculator shows concentration, draw units, syringe visualization, and doses per vial. Toggle microdose mode if desired. Switch between size variants using the pill buttons.
3. **Protocol** — Set injection frequency (per-day or per-week) for each peptide. Review total injections, vials needed, and reorder dates for your cycle length.
4. **Track** — Monitor vial expiry dates, review your order cart with pricing, and generate a shareable protocol summary card as a PNG image.

---

## Project Structure

```
casa-peptides-calculator/
├── .claude/
│   └── launch.json          # Claude Code dev server configuration
├── CLAUDE.md                 # AI development guidance (project architecture docs)
├── PLAN-redesign.md          # Visual redesign plan (dark → warm light theme)
├── newcalc-feature-diff.md   # Feature diff documentation (old vs. new calculator)
└── index.html                # The entire application (HTML + CSS + JS)
```

### Architecture (`index.html`)

The application is a self-contained single file organized into three sections:

- **`<style>`** — CSS custom properties in `:root` define the full theme (colors, fonts, radii, shadows). Component styles reference these variables. A `[data-theme="dark"]` block provides the alternate theme. Responsive breakpoints at 540px and 500px.
- **`<body>` HTML** — Four step pages (`step-0` through `step-3`) toggled via the `hidden` class. A share card modal overlays the page. The stepper nav is always visible.
- **`<script>`** — Vanilla JS organized into labeled sections: `DATA` (peptide library, shop URLs), `STATE` (application state management), `STEPPER` (navigation), and step-specific rendering/logic for Choose, Mix, Protocol, and Track.

---

## Contributing

Contributions are welcome! Here's how to get started:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/my-feature`)
3. Make your changes to `index.html` (remember — it's a single-file app)
4. Test in a browser by refreshing the page
5. Commit your changes (`git commit -m 'Add my feature'`)
6. Push to your branch (`git push origin feature/my-feature`)
7. Open a Pull Request

Since the entire app lives in one file, please include clear commit messages describing which section (CSS / HTML / JS) and which step (Choose / Mix / Protocol / Track) your changes affect.

---

## License

[TODO] — No license file currently specified. Please add a `LICENSE` file to clarify usage terms.

---

## Acknowledgments

- Built for [CASA Peptides](https://casapeptides.com)
- Developed with assistance from [Claude Code](https://claude.ai/code) by Anthropic
- Deployed on [Vercel](https://vercel.com)
