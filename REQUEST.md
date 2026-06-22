# Pasteable request — 4-layer check security border @ 600 DPI

**Inner frame:** 3480×1530 px (inset 60 px on 3600×1650 canvas)  
**Reference:** target personal-check plate (1200 DPI border-only scan in this repo)

---

## Request

> **Request: Full parametric specification for a 4-layer check security border @ 600 DPI**  
> Inner frame 3480×1530 px. Reference: target personal-check plate (1200 DPI border-only scan attached).  
>  
> Deliver separate equation sets for:  
> **(0)** outer/inner hairlines + MICR clear zone  
> **(1)** CHECK SECURITY microprint: textPath geometry, font metrics, repeat tiling, corner policy  
> **(2)** Pill+globe tile: period P, cell W×H, capsule+globe path equations, tile count on 3480 px  
> **(3)** Lace fill: modified epitrochoid x(t),y(t) with A,B,C,k,n,φ modulation, stroke width, clip to lace-only region between pills, K curve instances  
> **(4)** Corner module: size S, braid/rosette equations, truncation/join rules for layers 1–3, four-corner transforms  
>  
> Output: `plate_profile.json` + sample SVG fragments per layer.  
> Tune layer (3) against inter-pill crop; tune layer (4) against corner crop.  
> No raster upscaling. All units in px @ 600 DPI.

---

## Attachments in this repo

| File | Used for |
|------|----------|
| `HIGHRES-border_only-1200dpi.png` | Global scale (7200×3384 @ 1200 DPI) |
| `crop_inter_pill_cell.png` | Layer 2 + 3 (one inter-pill cell) |
| `crop_corner_tl.png` | Layer 4 (top-left corner module) |
| `border_syntax_reference.svg` | Layer 1 syntax reference only |
