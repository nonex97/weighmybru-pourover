# Print Variations

## Choosing Your Version

Before printing, you need to make two choices:

### Mounting Style

| Version | Description |
|---------|-------------|
| **Heat-set** | M4x6x4 brass inserts pressed into the enclosure | 
| **Self-tapping** | Screws drive directly into the plastic |

### Screen Cutout Fit

| Version | Description |
|---------|-------------|
| **Normal** | Standard clearance for the SH1106 display |
| **Loose (+1mm)** | 1mm extra on all sides — use if the screen is too tight |

!!! tip "Print the fit test first"
    Before committing to the full top plate print, print the display fit 
    test piece first. It takes only a few minutes and lets you confirm 
    which fit version works best for your printer and tolerances.

## STEP Files

All files are available in the [`/step`](https://github.com/nonex97/weighmybru-pourover/tree/main/step) folder of this repository.

| File | Description |
|------|-------------|
| `Display Fit Test — Normal` | Test piece, standard fit |
| `Display Fit Test — Loose (+1mm)` | Test piece, loose fit |
| `WeighMyBru² - Top (0.8mm) - Heat insert` | Top plate, heat insert version |
| `WeighMyBru² - Top (0.8mm) - Heat insert [Loose fit]` | Top plate, heat insert, loose fit |
| `WeighMyBru² - Top (0.8mm) - Self Tapping` | Top plate, self-tapping version |
| `WeighMyBru² - Top (0.8mm) - Self Tapping [Loose fit]` | Top plate, self-tapping, loose fit |
| `WeighMyBru² - Bottom` | Bottom enclosure — enable supports in slicer |
| `WeighMyBru² - Bottom Supported` | Bottom enclosure — 0.1mm built-in support surface around screw holes, no slicer supports needed |

!!! info
    All remaining files are identical regardless of which 
    mounting style or fit version you choose.

## 3D Printing Settings

For print settings (layer height, infill, supports etc.), the original 
WeighMyBru² printing guide applies directly to this build:

👉 [WeighMyBru² 3D Printing Guide](https://weighmybru.com/guides/assembly-guide/)