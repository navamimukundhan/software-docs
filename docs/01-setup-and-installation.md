# 01 - Setup, Environment & Build Instructions

This document records the exact software development environment, required tools, dependency versions, and the build/flash instructions for both the **PC Application** and the **Firmware**.

---

## 1. Programming Languages & Tools

### A. PC Application
| Tool / Component | Selection / Specification | Minimum Version | Notes |
| :--- | :--- | :--- | :--- |
| **Language** | Python / C# (.NET) / C++ / Node.js | e.g. Python 3.11.8 | Standardized across team |
| **GUI Framework** | PyQt6 / Tkinter / WPF / Electron | e.g. PyQt6 (v6.6.1) | Main user interface |
| **Package Manager** | `pip` / `poetry` / `npm` / `nuget` | pip v24.0 | For installing dependencies |
| **Recommended IDE** | Visual Studio Code / PyCharm / Visual Studio | VS Code 1.85+ | Recommended extensions below |

### B. Firmware
| Tool / Component | Selection / Specification | Version | Notes |
| :--- | :--- | :--- | :--- |
| **Target Controller** | STM32 / ESP32 / Arduino / RP2040 | e.g. ESP32-WROOM-32 | Microcontroller board |
| **Language / Standard**| C / C++ (C++17) | GCC ARM / xtensa-esp32-elf | Compiler toolchain |
| **Build Framework** | PlatformIO / STM32CubeIDE / Arduino CLI | PlatformIO Core 6.1+ | Cross-platform build system |
| **Hardware Programmer**| ST-Link v2 / USB-UART / J-Link | CP2102 / CH340 / ST-Link | Cable & driver for flashing |

---

## 2. PC Application Installation & Run Steps

### Step 1: Clone or Open Repository
```bash
git clone <repository_url>
cd <project_folder>/pc_app
```

### Step 2: Set up Virtual Environment (Python example)
```bash
# Create virtual environment
python -m venv venv

# Activate virtual environment
# On Windows PowerShell:
.\venv\Scripts\Activate.ps1
# On Windows Command Prompt:
.\venv\Scripts\activate.bat
```

### Step 3: Install Required Libraries
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

#### Typical `requirements.txt` Versions:
- `pyserial==3.5` (Serial COM port communication)
- `PyQt6==6.6.1` (GUI framework)
- `pyqtgraph==0.13.3` or `matplotlib==3.8.3` (Real-time plotting)
- `numpy==1.26.4` (Calibration & calculation logic)
- `pandas==2.2.1` (CSV data logging and report formatting)

### Step 4: Run the Application
```bash
python main.py
```

---

## 3. Firmware Build & Flashing Instructions

### Prerequisites
1. Install [VS Code](https://code.visualstudio.com/) with the **PlatformIO IDE** extension (or STM32CubeIDE / Arduino IDE).
2. Connect your microcontroller via USB.
3. Verify driver installation in Windows Device Manager under **Ports (COM & LPT)** (e.g., `Silicon Labs CP210x` or `CH340`).

### Method 1: Using PlatformIO (Recommended)
1. Open the firmware folder in VS Code (`File` -> `Open Folder` -> select `firmware/`).
2. Build the firmware binary:
   ```bash
   pio run
   ```
3. Flash (Upload) to target board:
   ```bash
   pio run --target upload
   ```
4. Open the Serial Monitor to verify boot:
   ```bash
   pio device monitor -b 115200
   ```

### Method 2: Using Arduino IDE / CLI
1. Open `firmware.ino` in Arduino IDE.
2. Under **Tools**, select:
   - **Board:** `[Select Board Name]`
   - **Port:** `COMx` (Check Windows Device Manager)
   - **Upload Speed:** `115200`
3. Click the **Upload** button (Arrow icon) or run:
   ```bash
   arduino-cli compile --upload -p COM3 --fqbn esp32:esp32:esp32 firmware.ino
   ```

---

## 4. Environment Verification Checklist

- [ ] PC Application opens without missing library errors.
- [ ] USB Serial Port is detected by Windows Device Manager without error icons.
- [ ] Firmware compiles with `0 errors, 0 warnings`.
- [ ] Firmware flashes successfully and prints boot banner in Serial Monitor.
- [ ] PC Application successfully opens the COM port.
