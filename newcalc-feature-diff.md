# CASA Peptides Protocol Planner — Feature Diff
**File comparison:** `newcalc.html` vs. `oldcalc.html`
**Purpose:** Features present in `newcalc.html` that are NOT in `oldcalc.html` — for production implementation.

---

## 1. Dark / Light Theme Toggle

`newcalc.html` adds a full light/dark mode system. `oldcalc.html` was dark-only.

- A circular toggle button is rendered in the top-right corner of the app (`.theme-toggle` class, positioned absolute within `.app`).
- Clicking it calls `toggleTheme()`, which flips the `data-theme` attribute on `<html>` between `"dark"` and `"light"`.
- The user's preference is persisted to `localStorage` under the key `casa-theme`.
- An inline `<script>` in `<head>` reads `localStorage` and sets the theme attribute *before* CSS renders, preventing a flash of the wrong theme on page load.
- The toggle button icon updates dynamically: sun icon in dark mode, moon icon in light mode (via `updateThemeIcon()`).
- A complete `[data-theme="dark"]` CSS block is defined alongside the default (light) `:root` block. Both use CSS custom properties, so the entire UI adapts.

---

## 2. Dramatically Expanded Peptide Library (~19 → ~100+ entries)

`oldcalc.html` had 19 total peptide entries (one per product).
`newcalc.html` has ~100+ entries covering multiple vial sizes per peptide.

### New peptides added (not present in old version at all):

**Recovery & Healing:**
- LL-37 (5mg)
- GLOW Blend (70mg — TB+BPC+GHK-Cu combo)
- KLOW Blend (80mg — TB+BPC+GHK+KPV combo)
- KPV (10mg)
- Thymalin (10mg)
- Glutathione 600mg (pre-mixed, oral)
- Glutathione 1500mg (pre-mixed, oral)

**Fat Loss & Metabolic:**
- Cagrilintide (5mg, 10mg)
- Cagrilintide + Semaglutide Combo (10mg)
- Adipotide (5mg)
- AICAR (50mg)
- 5-Amino-1MQ (5mg oral, 50mg oral)
- Mazdutide (10mg)
- Survodutide (10mg)

**Hormone & Growth:**
- CJC-1295 DAC (2mg, 5mg) — distinct from existing CJC-1295 no DAC
- CJC+Ipamorelin Combo (10mg)
- GHRP-6 (5mg, 10mg)
- Hexarelin (5mg)
- Tesamorelin (2mg, 5mg, 10mg, 20mg)
- HGH 191AA (10IU, 12IU, 15IU, 24IU, 36IU)
- IGF-1 LR3 (0.1mg, 1mg)
- MGF (2mg)
- PEG-MGF (2mg)

**Cognitive & Neuro:**
- Semax (5mg, 10mg)
- DSIP (5mg, 10mg, 15mg)
- Cerebrolysin (60mg)
- MOTS-C (10mg, 40mg)
- SS-31 (10mg, 50mg)
- FOXO4-DRI (10mg)
- Dermorphin (5mg)

**Sexual Health & Fertility** *(entire new category)*:
- PT-141 (10mg)
- Melanotan II (10mg)
- Melanotan I (10mg)
- HCG 5000IU
- HCG 10000IU
- HMG (75IU — oral/MD protocol)
- Gonadorelin (2mg)
- KissPeptin-10 (5mg, 10mg)
- Oxytocin (2mg, 5mg, 10mg)

**Specialty & Anti-Aging** *(entire new category)*:
- NAD+ (100mg, 500mg, 1000mg)
- Melatonin Injectable (10mg)
- SNAP-8 (10mg)
- Slupp 332 (5mg)
- VIP (10mg)

### Existing peptides now offered in additional vial sizes:

| Peptide | Old sizes | New sizes |
|---|---|---|
| BPC-157 | 5mg | 5mg, 10mg |
| TB-500 | 5mg | 2mg, 5mg, 10mg |
| BPC/TB Blend | 10mg | 10mg, 20mg |
| GHK-Cu | 50mg, 100mg | 50mg, 100mg (unchanged) |
| Semaglutide | 5mg | 5mg, 10mg, 15mg, 20mg, 30mg |
| Tirzepatide | 10mg | 5mg–120mg (10 size options) |
| Retatrutide | 10mg | 5mg–60mg (8 size options) |
| AOD-9604 | 5mg | 2mg, 5mg |
| Ipamorelin | 5mg | 5mg, 10mg |
| GHRP-2 | 5mg | 5mg, 10mg |
| Sermorelin | 3mg | 5mg, 10mg |
| Selank | 5mg | 5mg, 10mg |
| Epithalon | 10mg | 10mg, 50mg |
| CJC-1295 (no DAC) | 2mg | 2mg, 5mg, 10mg |

---

## 3. Two New Peptide Categories in the Selection UI

`oldcalc.html` had 4 categories on the Choose step:
- Recovery & Healing
- Fat Loss & Metabolic
- Hormone & Growth
- Cognitive & Neuro

`newcalc.html` adds 2 more:
- **Sexual Health & Fertility** (`grid-fertility`)
- **Specialty & Anti-Aging** (`grid-specialty`)

The `buildChooseStep()` function and the HTML grid containers both iterate over all 6 categories.

---

## 4. Peptide Size Grouping on the Selection Screen

In `oldcalc.html`, every vial size was its own card on the Choose screen.

In `newcalc.html`, size variants of the same peptide are **collapsed into one card**. Key implementation details:

- A `GROUPS` object is built from the peptide data, grouping variants by their base name (stripping the trailing `_Xmg` suffix from the ID).
- Helper functions: `groupKey(id)`, `groupBaseName(gk)`, `groupSizesLabel(gk)`, `selectedInGroup(gk)`.
- Clicking a group card calls `toggleGroup(gk)` instead of the old `togglePeptide(id)`.
- Cards use the ID prefix `gcard-` (vs. old `pcard-`).
- An `updateGroupCards()` function re-syncs the selected visual state on the cards when navigating back to Step 0. This is called from `goStep(0)`.

---

## 5. In-Mix Vial Size Switcher

When a selected peptide has multiple size variants, the Mix step now shows a row of pill buttons above the reconstitution inputs, letting the user switch between sizes without returning to Step 0.

- Built inside `buildMixPanel(id)` — a `sizeHTML` block is prepended to the panel when `GROUPS[gk].length > 1`.
- Each pill button calls `switchVariant(gk, newId)`.
- `switchVariant()` removes the old variant from `state.selections`, deletes its config, adds the new variant, updates `state.activeMixId`, and re-renders the Mix step.

---

## Summary Checklist for Developer

- [ ] Add dark/light theme toggle button and `toggleTheme()` / `updateThemeIcon()` logic
- [ ] Add `[data-theme="dark"]` CSS block with CSS custom properties
- [ ] Add inline `<script>` in `<head>` to apply saved theme before render
- [ ] Replace `PEPTIDES` array with expanded ~100-entry version
- [ ] Replace `SHOP_URLS` map with updated entries for all new peptide IDs
- [ ] Replace `CAT_COLORS` map to include `fertility` and `specialty` categories
- [ ] Add `grid-fertility` and `grid-specialty` containers to HTML (Step 0)
- [ ] Add `GROUPS`, `groupKey()`, `groupBaseName()`, `groupSizesLabel()`, `selectedInGroup()` logic
- [ ] Replace `buildChooseStep()` to render grouped cards (`gcard-` prefix, `toggleGroup()`)
- [ ] Add `toggleGroup()` and `updateGroupCards()` functions
- [ ] Update `goStep(0)` to call `updateGroupCards()`
- [ ] Update `buildMixPanel()` to render the size-switcher pill row when variants > 1
- [ ] Add `switchVariant(gk, newId)` function
