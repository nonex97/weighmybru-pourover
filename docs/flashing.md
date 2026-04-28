# Flashing Guide

## Prerequisites

Before starting, make sure you have the following ready:

✅ VSCode installed

✅ PlatformIO extension installed

✅ Seeed Studio XIAO ESP32S3

✅ USB-C cable (data-capable, not charge-only)

---

## Step 1 — Install Git

=== "Windows"
    Download Git from [git-scm.com](https://git-scm.com/download/win)
    and run the installer with default settings. Then verify in PowerShell:

    ```powershell
    git --version
    ```

=== "macOS"
    Using Homebrew
    ```bash
    brew install git
    ```
    Or download from [git-scm.com](https://git-scm.com/install/mac)


=== "Linux (Ubuntu/Debian)"
    
    ```bash
    sudo apt update && sudo apt install git
    ```

---

## Step 2 — Clone the Repository

Open a terminal and run:

```powershell
cd ~/Documents # or wherever you keep projects
```
```
mkdir esp32-projects
```
```
git clone -b SH1106 https://github.com/mlodedrwale/weighmybru2.git
```
```
cd weighmybru2

```

---

## Step 3 — Open in VSCode with PlatformIO

1. Launch VSCode
2. Go to **File → Open Folder**
3. Navigate to and select the `weighmybru2` folder you just cloned
4. Click **Select Folder**
5. Wait for PlatformIO to initialize — you'll see activity in the bottom status bar

---

## Step 4 — Configure `platformio.ini`

Check that the `platformio.ini` in the root of the project matches this:

```ini
[env:esp32-s3-devkitc-1]
platform = espressif32@6.12.0
board = esp32-s3-devkitc-1
framework = arduino
monitor_speed = 115200
board_upload.flash_size = 4MB
board_build.filesystem = littlefs
board_build.partitions = huge_app.csv
upload_protocol = esptool
upload_speed = 460800
upload_flags = 
	--chip=esp32s3
	--before=default_reset
	--after=hard_reset
monitor_rts = 0
monitor_dtr = 0
build_flags = 
	-DARDUINO_USB_CDC_ON_BOOT=1
	-DBOARD_HAS_PSRAM
	-Os
	-DCORE_DEBUG_LEVEL=0
lib_deps = 
	robtillaart/HX711@^0.6.0
	https://github.com/me-no-dev/ESPAsyncWebServer.git
	https://github.com/me-no-dev/AsyncTCP.git
	h2zero/NimBLE-Arduino@^1.4.0
	adafruit/Adafruit SSD1306@^2.5.7
	adafruit/Adafruit GFX Library@^1.11.9
	adafruit/Adafruit SH110X@^2.1.14
```

---
## Step 5 — Apply Code Changes

--8<-- "docs/code-changes.md"
---

## Step 6 — Connect the Board

1. Plug the XIAO ESP32S3 into your computer via USB-C
2. If the board isn't detected, put it into flash mode manually:
    - Hold **BOOT** while plugging in the USB
    - Press and release **RESET**
    - Release **BOOT**



---

## Step 7 — Find Your COM Port

In VSCode, check the **blue bottom status bar** — the COM port is listed there (e.g. `COM3` on Windows, "/dev/ttyUSB0" on Linux).

If it's not visible:

- **PlatformIO sidebar** → Devices
- **Windows:** Device Manager → Ports (COM & LPT)
- **macOS/Linux:** `ls /dev/tty*`

---

## Step 8 — Build and Upload

=== "Status Bar (Easiest)"
    - Click **✓** in the blue bottom bar to build
    - Click **→** to upload

=== "PlatformIO Sidebar"
    1. Click the PlatformIO icon (alien head) in the left sidebar
    2. Expand **PROJECT TASKS → esp32-s3-devkitc-1 → General** 
    3. Click **Build** to compile the code first
    4. **Wait for successful build** (check terminal output)
    5. Click **Upload**

=== "Terminal"
    1. **Open PlatformIO terminal:** Terminal → New Terminal
    2. **Build the project:**
    ```bash
    pio run
    ```
    3. **Upload to ESP32:**
    ```
    pio run -t upload -t uploadfs
    ```

---

## Step 8b — Upload Filesystem Image

!!! warning "Don't skip this step"
    Flashing the code alone is not enough. The web interface files are stored 
    separately in the filesystem. If you skip this step, the scale will flash 
    successfully but `weighmybru.local` will not be reachable.

After uploading the code, you need to also upload the filesystem image:

=== "PlatformIO Sidebar"
    1. Click the PlatformIO icon (alien head) in the left sidebar
    2. Expand **PROJECT TASKS → esp32-s3-devkitc-1 → Platform   **
    3. Scroll down and click **Upload Filesystem Image**
    4. Wait for the upload to complete

=== "Terminal"
    ```bash
    pio run -t uploadfs
    ```

Once both uploads are done, press the **RESET** button on the board and 
`weighmybru.local` should become reachable after connecting to the scale's WiFi.

## Step 9 — Monitor Serial Output

Click the **plug icon** in the blue status bar, or run:

```bash
pio device monitor
```

---

## Troubleshooting

??? question "Upload failing"
    - Wrong COM port — check Device Manager and verify `platformio.ini`
    - Board not in flash mode — retry the BOOT + RESET procedure
    - Driver issue — install [ESP32 USB drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) from Silicon Labs

??? question "Build errors"
    - Missing libraries — they'll be named in the error output; PlatformIO installs them automatically on the next build
    - Wrong board config — confirm your `platformio.ini` matches the config above exactly

??? question "Device not found"
    List connected devices
    ```bash
    pio device list
    ```
    On Linux, you might need
    ```bash
    sudo chmod 666 /dev/ttyUSB0  # or your specific device
    ```

---

## Post-Flash Setup

1. **Connect to the ESP32's WiFi access point** (check serial monitor for details - AP IP :192.168.4.1)
2. **Access the web interface** for calibration and configuration
3. **Configure your home WiFi** through the web interface

---

## Success Indicators
✅ Build completes without errors

✅ Upload shows "SUCCESS" message

✅ Serial monitor shows boot messages

✅ ESP32 creates WiFi access point (for initial setup)

Your WeighMyBru scale should now be running! Check the serial monitor for the web interface URL and initial setup instructions.

## Next Steps — Calibration

With the firmware flashed and the scale booting correctly, the last step 
is to calibrate the scale.

The calibration process for this build is identical to the original 
WeighMyBru², so follow the official guide:

👉 [WeighMyBru Calibration Guide](https://weighmybru.com/guides/calibration-guide/)

## Hardcoding WiFi Credentials (Optional)

!!! note "Only do this if the access point is not showing up"
    In most cases the scale will create a WiFi access point on first boot 
    and you can configure credentials through the web interface. Only follow 
    these steps if no access point appears and the serial monitor is showing 
    WiFi-related errors on boot.

### Step 1 — Open `main.cpp`

In VSCode, open `src/main.cpp` and find the `#include` block at the top of 
the file. Add these two lines directly after it:

```cpp
static const char* hardcode_ssid = "YOUR_SSID";
static const char* hardcode_password = "YOUR_PASSWORD";
```

Replace `YOUR_SSID` and `YOUR_PASSWORD` with your actual WiFi network name 
and password.

### Step 2 — Find the WiFi Sleep Lines

Scroll down in `main.cpp` until you find these two lines (around line 108):

```cpp { .no-copy }
WiFi.setSleep(true);
Serial.println("WiFi power management enabled for battery optimization");
```

Add the following line immediately after them:

```cpp
saveWiFiCredentials(hardcode_ssid, hardcode_password);
```

### Step 3 — Re-flash

Save the file, then repeat **Step 7** (Build and Upload). The scale will now connect directly to your 
WiFi network on boot instead of creating an access point.
