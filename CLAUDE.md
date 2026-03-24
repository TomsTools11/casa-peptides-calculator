# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

CASA Peptides Protocol Planner — a single-file HTML application (`peptide-calculator.html`) for planning peptide dosing protocols. Built for CASA Peptides (casapeptides.com). No build system, no dependencies, no package manager — just open the HTML file in a browser.

## Development

Open `peptide-calculator.html` directly in a browser. There are no build steps, tests, or linting tools. To preview changes, refresh the browser.

## Architecture

Everything lives in one file with three sections:

1. **`<style>`** (lines 10–166) — CSS custom properties in `:root` define the dark theme (colors, fonts, radii, shadows). All component styles use these variables. Responsive breakpoints at 540px and 500px.

2. **`<body>` HTML** (lines 168–322) — Four step pages (`step-0` through `step-3`) toggled by adding/removing the `hidden` class. A share card modal (`share-modal`) overlays the page. The stepper nav is always visible.

3. **`<script>`** (lines 324–1142) — Vanilla JS, no framework. Organized into sections marked with `═══` comment banners:
   - **DATA** — `PEPTIDES` array (each entry has id, name, category, vial size, dose, frequency, stability) and `SHOP_URLS` mapping peptide IDs to casapeptides.com product URLs.
   - **STATE** — Single `state` object tracks current step, selected peptide IDs (Set), per-peptide configs, active mix tab, and tracker dates. The `cfg(id)` function lazily initializes per-peptide config (vial, BAC water, units, dose, microdose toggle, frequency mode/input).
   - **STEPPER** — `goStep(n)` handles navigation, renders the target step, and updates stepper circle states (active/done).
   - **STEP 0: CHOOSE** — Peptide selection grid, grouped by category (recovery, fat, hormone, cognitive).
   - **STEP 1: MIX** — Reconstitution calculator with tabbed panels per peptide. Computes concentration, draw units, syringe fill, doses per vial. Has a microdose split toggle that halves each dose and doubles injection frequency.
   - **STEP 2: PROTOCOL** — Per-peptide frequency controls (per-day or per-week), calculates total injections, vials needed, and reorder dates for a configurable cycle length.
   - **STEP 3: TRACK** — Vial expiry tracker (date-based), order cart with per-vial pricing and checkout links, and full protocol summary.
   - **SHARE CARD** — Canvas-based image generator that renders the protocol as a branded PNG for sharing/downloading.

## Key Patterns

- **Peptide ID convention**: Short lowercase IDs (e.g., `bpc157`, `sema`, `tirze`) used as keys everywhere — DOM element IDs, state maps, shop URLs.
- **Frequency model**: `freq` in PEPTIDES is injections per day (fractional for weekly: 0.14 ≈ 1/week). The UI lets users toggle between per-day and per-week input via `freqMode`/`freqInput` in config.
- **Microdose**: Halves units per injection, doubles frequency — same weekly total. Affects calculations across mix, protocol, and summary steps.
- **DOM IDs follow patterns**: `mv-{id}`, `mb-{id}`, `mu-{id}` for mix inputs; `pfreq-{id}` for protocol frequency; `td-{id}` for tracker dates; `orow-{id}` for order rows.
- **Fonts**: Red Hat Display (headings/display) and Geist (body) loaded from Google Fonts.
