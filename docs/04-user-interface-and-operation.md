# 04 - User Interface (UI) Controls & Operation Guide

This guide describes every screen, button, input box, and indicator in the PC Application in beginner-friendly language. Operators and testers can refer to this document to run the system with zero guesswork.

---

## 1. Application Window Layout Overview

```
+------------------------------------------------------------------------------------+
|  [Logo] Project Title: Automated Test Station v1.0.0             [ - ] [ # ] [ X ] |
+------------------------------------------------------------------------------------+
| [ 1. Port Selection: COM3 v ] [ Refresh ] [ CONNECT / DISCONNECT ]  [ Status: Connected ] |
+------------------------------------+-----------------------------------------------+
|  2. CONTROL PANEL                  |  3. REAL-TIME DATA PLOT                       |
|                                    |   ^ Sensor Reading (V / °C / N)               |
|  Test Profile: [ Standard 60s  v ] |   |      /\                                   |
|  Target Value: [ 25.0        ] mm  |   |     /  \    /\                            |
|  Sample Rate:  [ 100         ] ms  |   |----+----\--/--\--------------------->     |
|                                    |   0          Time (seconds)             60s   |
|  [ START TEST ]   [ STOP / HALT ]  +-----------------------------------------------+
|                                    |  4. LIVE READINGS & GAUGES                    |
|  [ CALIBRATE ]    [ EXPORT CSV ]   |   Current Val: 24.87 mm  |  Max: 25.12 mm     |
|                                    |   Temperature: 28.4 °C   |  Limit: 30.0 °C [OK|
+------------------------------------+-----------------------------------------------+
| 5. LOG / CONSOLE OUTPUT WINDOW:                                                    |
|  [19:15:02] Connected to COM3 at 115200 baud. Device: Rev 1.2 Firmware             |
|  [19:15:10] Test sequence started. Logging to: /logs/20261115_test01.csv          |
+------------------------------------------------------------------------------------+
```

---

## 2. Detailed Explanation of UI Controls

### Section 1: Connection & Hardware Bar
* **COM Port Dropdown (`Port Selection`):**
  * **What it does:** Displays all USB serial ports detected by Windows.
  * **How to use:** Plug in your microcontroller board. If not listed, click **Refresh**. Select the port corresponding to the board (e.g. `COM3` or `COM4`).
* **Refresh Button:**
  * **What it does:** Scans Windows Device Manager for newly plugged in USB devices.
* **Connect / Disconnect Button:**
  * **What it does:** Opens or closes the serial communication channel with the firmware.
  * **Visual Feedback:** Turns **Green** with label *"Connected"* when communication is active; turns **Red** when disconnected.

### Section 2: Test Control Panel
* **Test Profile Dropdown:**
  * **What it does:** Selects preset test configurations (e.g., *Quick Check 10s*, *Full Calibration 60s*, *Endurance Run 1hr*).
* **Target Value Input:**
  * **What it does:** Sets the desired target parameter (e.g. target voltage, temperature, or motor position).
  * **Validation rule:** Must be a numerical value within the allowed safety bounds (e.g., `0.0` to `50.0`).
* **Sample Rate Input:**
  * **What it does:** Defines how often (in milliseconds) the firmware samples sensors and sends updates to the PC.
* **START TEST Button:**
  * **What it does:** Begins the automated test sequence. Prompts for a filename if logging is enabled.
* **STOP / HALT Button:**
  * **What it does:** Immediately aborts the current test and commands the hardware to return to a safe idle state.
* **CALIBRATE Button:**
  * **What it does:** Opens the Calibration Modal where zero-point (offset) and scale-factor (gain) values can be applied to sensor channels.
* **EXPORT CSV Button:**
  * **What it does:** Saves the current chart data and recorded test points into an Excel-compatible `.csv` file.

### Section 3: Real-Time Plot
* **Graph Area:**
  * Displays a live graph of incoming sensor values against elapsed time.
  * **Controls:**
    * Right-click graph: Reset view / Auto-scale.
    * Scroll wheel: Zoom in / Zoom out.
    * Left-click + drag: Pan across data history.

### Section 4: Live Readings & Gauges
* **Current Value Tile:** Large digital readout of the latest measurement.
* **Status Badges:**
  * 🟢 **NORMAL / PASS:** All parameters within acceptable engineering limits.
  * 🟡 **WARNING:** Parameter approaching upper safety threshold.
  * 🔴 **FAULT / ALARM:** Parameter exceeded limits; test automatically paused.

### Section 5: Console & Event Log
* Displays timestamped internal system events, firmware responses, file paths where logs are saved, and warnings.

---

## 3. Adding Screenshots to Documentation

> **Maintainer Instruction:** Whenever UI layout changes are made, capture a clean screenshot (`Win + Shift + S`) and save it to `assets/gui_main_window.png`. Then update the link below:

![Main Application Window](../assets/gui_main_window.png)
*(Figure 1: Main GUI window with connected microcontroller)*
