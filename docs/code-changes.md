# Code Changes

The original WeighMyBru² firmware needs a few small changes to work with 
the hardware used in this build.

---

## Required Changes

### 1. Display Controller — `config.h` (line 20)

Change from:

```cpp { .no-copy }
#define DISPLAY_CONTROLLER DISPLAY_CONTROLLER_SSD1306
```

To:

```cpp
#define DISPLAY_CONTROLLER DISPLAY_CONTROLLER_SH1106
```

---

### 2. Display Height — `config.h` (line 24)

Change from:

```cpp { .no-copy }
#define DISPLAY_HEIGHT DISPLAY_HEIGHT_32
```

To:

```cpp
#define DISPLAY_HEIGHT DISPLAY_HEIGHT_64
```

---

### 3. Display Rotation — `SH1106Driver.h` (line 23)

Change from:

```cpp { .no-copy }
oled.setRotation(2);
```

To:

```cpp
oled.setRotation(0);
```

!!! note "Display upside down?"
    If the display appears upside down after flashing, change the value 
    back from `0` to `2`.

---

## Optional Changes

### Always-On Battery Percentage — `DisplayLayout.h`

By default the battery percentage is not permanently shown on the display. 
To add it, find line 152 in `DisplayLayout.h`:

```cpp { .no-copy }
// display.print("WI");
```

Add the following block immediately after it:

```cpp
// BATTERY PERCENTAGE
        display.setFont(&FreeMonoBold9pt7b);
        display.setTextSize(1);
        String batteryStr = String(st.batteryPercent) + "%";

        // Calculate text width for right alignment
        int16_t x1, y1;
        uint16_t w, h;
        display.getTextBounds(batteryStr, 0, 0, &x1, &y1, &w, &h);

        // Right-align: set cursor so the text ends at DISPLAY_WIDTH (128)
        int cursorX = DISPLAY_WIDTH - w - 2;
        display.setCursor(cursorX, 57);

        // Blink if battery < 10%
        if (st.batteryPercent < 10) {
            if ((millis() / 500) % 2 == 0) { // Show only every other 500ms
                display.print(batteryStr);
            }
        } else {
            display.print(batteryStr);
        }
```

This renders the battery percentage right-aligned at the bottom of the 
display, and blinks it when the battery drops below 10%.