# Vision — Morgan Stanley Active Assets Check Template

**Status:** DRAFT — awaiting operator approval / modification  
**Owner:** User (operator)  
**Workspace:** `C:\chktmp\`  
**Primary deliverable:** `Composer-2.5_check_template.html`  
**Last updated:** 2026-06-21

---

## 1. End goal (draft for approval)

Produce a **pixel-perfect, print-ready personal check face** for the **Morgan Stanley Active Assets** plate — not a generic Harland/Deluxe demo, not a blurred scan composite, and not a forensics ROI sketch.

The finished artifact is a **single-page HTML/CSS (and embedded SVG) template** at **600 DPI**:

| Property | Value |
|----------|--------|
| Canvas | **3600 × 1650 px** (6.00″ × 2.75″) |
| Output path | `C:\chktmp\Composer-2.5_check_template.html` |
| Print | `@page` 6″ × 2.75″, zero margin, `print-color-adjust: exact` |
| Positioning | Absolute X/Y coordinates — operator spec is the content contract |

### What “done” looks like (operator eyeball)

1. **Border** — Matches the real MS plate: outer/inner hairlines, **CHECK SECURITY** (or plate-accurate) microprint on `textPath`, repeating guilloche band with **globe/pill window motifs**, corner rosette/braid joins, MICR-zone clearing at bottom-right. Lines stay sharp at 600 DPI zoom — **no mush from upscaled bitmaps**.

2. **Background** — Safety Blue face: measured tint + **basketweave / pantograph** screen (not flat `#E2F3F9` placeholder).

3. **Content** — Morgan Stanley branding, account holder block, check number, fractional routing, date/payee/amount/signature/Expense Analyzer/MICR at spec coordinates; typography and weights match print-ready reference.

4. **MICR** — E-13B via `micrenc.ttf`, 75 px pitch, baseline Y=1500, sample routing/account/check glyphs.

5. **Method** — Security art is **built procedurally** (SVG patterns, paths, tiled math). Scans are **reference rulers only** — never the shipped border layer.

6. **Verification** — Operator confirms locally (open HTML, print preview, compare to physical check). Agent does **not** declare “good to go” without that.

### Explicit non-goals

- Not a raster cut-and-paste of a 1200 DPI border PNG into the template.
- Not CHECK 21 / `check21_layout.py` OCR heuristics as plate geometry.
- Not the CHK_VERIFY Flet desktop app (separate Hermes/agent_auto track).
- Not declaring complete until the operator has verified.

---

## 2. Vision Gate

All work on this project is run through the **Vision Gate** repeatedly — at milestones, after refactors, and sometimes mid-task without warning.

### Gate questions (pass / fail / defer)

| # | Criterion | Pass means |
|---|-----------|------------|
| G1 | **Procedural security art** | Border and background are SVG/math-generated parameters; no upscaled scan as the production border. |
| G2 | **Plate fidelity** | Matches MS Active Assets reference (print-ready + 1200 DPI rulers), not a generic security check. |
| G3 | **Canvas contract** | Exactly 3600×1650; frame `(60,60)–(3540,1590)` unless operator revises spec. |
| G4 | **Content contract** | Field positions follow operator turn-1 coordinate table until remeasured from approved plates. |
| G5 | **Print viability** | Renders in browser; backgrounds/borders survive print; MICR font resolves. |
| G6 | **Sharpness** | Zoomed 600 DPI view: microprint readable, guilloche edges not blobbed. |
| G7 | **Reference role** | Scans/masks used only to measure pitch, band width, colors, copy — not pasted as art. |
| G8 | **Operator sign-off** | User has visually approved the current milestone. |

**Rule:** If G1 or G8 fails, the milestone is **not** complete regardless of how good a preview screenshot looks.

### Current gate snapshot (2026-06-21)

| Gate | Status | Notes |
|------|--------|-------|
| G1 | **FAIL** | `extract_border_from_highres.py` still rasterizes; must move to procedural tile builder. |
| G2 | **IN PROGRESS** | MS content in HTML; border still generic/wrong vs plate. |
| G3 | **PASS** | Canvas and frame coords implemented. |
| G4 | **PASS** | Original spec coords in template. |
| G5 | **PARTIAL** | Renders; basketweave deferred. |
| G6 | **FAIL** | 1200 DPI crop mush; user rejected approach. |
| G7 | **PARTIAL** | Good rulers on disk; misuse corrected in vision. |
| G8 | **PENDING** | Awaiting operator approval of this document and next build. |

---

## 3. Reference assets (rulers, not wallpaper)

| Asset | Role |
|-------|------|
| `HIGHRES-border_only-1200dpi.png` (7200×3384) | Measure band width, repeat pitch, globe/pill spacing, corner geometry, microprint size |
| `0002blankcursor-whited.png` | Ink-vs-paper mask geometry |
| `frontpersonalscan-topaz-denoise-remove.png` | Full-check layout proportions |
| Border strip phone scans | SECURITY microprint wording, pill windows |
| Print-ready MS art / real scans | Color, typography, content placement |
| `Google-harlands-fullsvg.svg` | **Method reference** (see §4) — not the visual target |

---

## 4. `Google-harlands-fullsvg.svg` vs this vision

**File:** `C:\chktmp\Google-harlands-fullsvg.svg`  
**Canvas:** 3600×1650 — same contract as our template.

### How it aligns with the goal (methodology) ✅

| Harland SVG approach | Our vision |
|---------------------|------------|
| SVG `<pattern>` tiles (pantograph, guilloche) | Required for background + border fill |
| `<textPath>` microprint ("SECURITY CHECK…") | Required for border microprint band |
| Corner rosette `<g>` reused with `<use>` | Required for corners |
| Component groups (holder, date, payee, amount, sig, MICR) | Same structure for HTML/SVG layers |
| Vector stroke widths in 600 DPI space | Correct scaling model |
| **Math-first, no embedded scan** | **This is the mandated approach (G1)** |

### How it diverges from the goal (plate & content) ❌

| Harland SVG | Morgan Stanley Active Assets target |
|-------------|-------------------------------------|
| Generic auditor names / Harland demo copy | Gabrielson account, MS branding, UMB bank line |
| Frame at 25/75 px inset | Operator spec: **60/60** frame |
| Simple sine guilloche (`guilloche-wave-600`) | Globe-in-oval pills, lace fill, CHECK SECURITY band, corner braid |
| Generic photo-safe crosshatch | **Safety Blue basketweave** (plate-specific) |
| Navy `#001a4d` / `#002060` | Measured MS ink (~`#12203a` navy on Safety Blue face) |
| No Expense Analyzer, CHECK ARMOR, fractional `25-80/440` layout | Required MS plate elements |
| Placeholder MICR string | Spec sample: `044000804` / `8902052537613` / `2601` |

**Summary:** Harland SVG is the **right architecture, wrong plate**. Use it as the **blueprint for how to build** (patterns + textPath + components). Do **not** treat it as the visual finish line. The finish line is the MS plate measured from operator references.

---

## 5. Approved build sequence (post-vision)

1. **Measure** — From `HIGHRES-border_only-1200dpi.png`: band width, tile period, pill width, microprint `font-size` / `letter-spacing`, corner module size. Write `plate_profile.json` (numbers only).
2. **Procedural border module** — Python or inline SVG: `ms_border_tile.svg` + corner module + `textPath` SECURITY string; tile around `(60,60)–(3540,1590)`.
3. **Safety Blue background** — Basketweave pattern from measured tint (defer until border gate passes G1+G6).
4. **HTML integration** — Embed procedural SVG; remove `check_border_layer.png` raster dependency.
5. **Content tune** — Remeasure coords from 1024×465 plates if needed.
6. **Vision Gate** — Re-run §2 before any “milestone complete” message.

---

## 6. Operator approval block

> **Instructions:** Edit this section or reply in chat: Approve as-is / modify / reject.  
> Until approved, §1 is a draft, not a locked contract.

- [ ] I approve §1 End goal (draft above)
- [ ] I approve §2 Vision Gate criteria
- [ ] I approve §5 Build sequence priority

**Operator notes / modifications:**

```
(leave blank or add edits)
```

---

## 7. Changelog

| Date | Change |
|------|--------|
| 2026-06-21 | Initial Vision.md. Operator rejected raster border extraction (#1). Harland SVG reviewed as method reference. G1/G6 marked FAIL. |
