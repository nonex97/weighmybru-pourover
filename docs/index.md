# WeighMyBru Pour-Over

A custom variant of the [WeighMyBru²](https://weighmybru.com) DIY espresso scale, 
redesigned for pour-over brewing with a custom 3D printed enclosure, 
a 1.3" SH1106 OLED display, and a 3kg load cell.

---

## Credits

- **WeighMyBru²** original project by [031devstudios](https://weighmybru.com)
- **SH1106 display driver** implementation by 
  [mlodedrwale](https://github.com/mlodedrwale/weighmybru2/tree/SH1106) — 
  this build would not have been possible without their work on SH1106 support.

## What's Different in This Build

- **Display:** 1.3" SH1106 OLED
- **Load cell:** 3kg load cell
- **Enclosure:** Bigger body dimensions (140x100x29mm)

---

## Print Versions

There are two sets of choices when printing this build — pick one from each.

### Enclosure Mounting

| Version | Description | Best for |
|---------|-------------|----------|
| **Heat-set (not tested)** | Uses M3x5x4 brass heat-set inserts for threaded mounting | Stronger, survives repeated disassembly |
| **Self-tapping (tested)** | Screws drive directly into the plastic | Simpler build, no soldering iron or heat inserts needed |

### Screen Cutout Fit

| Version | Description | Best for |
|---------|-------------|----------|
| **Normal (tested)** | Standard fit for the OLED screen | Most printers |
| **Loose (not tested)** | 1mm extra clearance on all sides | If the screen is a tight fit on your print |

### Not Sure Which Fit to Use?

Before printing the full enclosure, print the **display fit test** first — 
it's a small piece that only takes around 20 minutes and lets you check whether 
the Normal or Loose fit works better for your specific screen and printer.

!!! tip "Always test before printing the full enclosure"
    Download the test print, check if your screen sits flush and stable, 
    then choose your version accordingly. The full top plate takes much 
    longer to print and filament is worth saving!

| Test Print | For |
|------------|-----|
| Display Fit Test - Normal | Standard fit |
| Display Fit Test - Loose | Loose fit |

STL files for the test prints are in the [`/step/display-fit-test`](https://github.com/nonex97/weighmybru-pourover/tree/main/step/display-fit-test) folder of this repo.

## Getting Started

1. [Parts List](parts-list.md) — everything you need with links
2. [Flashing Guide](flashing.md) — how to flash the firmware to your board
3. [Code Changes](code-changes.md) — what to configure before compiling from source