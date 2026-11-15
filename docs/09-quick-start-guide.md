# 09 - Quick-Start Guide (5-Minute Operator Walkthrough)

Follow these simple steps to run the software and perform a test sequence from start to finish.

---

## ⏱️ Pre-requisites (Check before starting)
- [ ] Microcontroller hardware is plugged into a USB port on your PC.
- [ ] Python 3.10+ (or required runtime) is installed.
- [ ] Python dependencies are installed (`pip install -r requirements.txt`).

---

## 🚀 5-Step Quick Run

### Step 1: Launch the PC Application
Open a terminal (PowerShell or Command Prompt) and run:
```bash
python main.py
```
*The main graphical user interface will appear on your desktop.*

---

### Step 2: Connect to the Hardware
1. Look at the top-left toolbar: find the **Port Selection** dropdown.
2. Select your device's COM port (e.g., `COM3` or `COM4`).
   *(If you don't see it, click **Refresh**).*
3. Click the **CONNECT** button.
4. Verify that the indicator turns **Green** and reads **"Connected"**.

---

### Step 3: Check Hardware Baseline & Calibrate
1. Ensure the sensor is clear of any load.
2. Click the **CALIBRATE** button.
3. Select **Channel 1** and click **Tare / Set Zero**.
4. Confirm the live reading displays `0.00 ± 0.05`.

---

### Step 4: Run a Test Sequence
1. In the **Control Panel**, choose your desired test preset from the **Test Profile** dropdown (e.g. *Standard 60s*).
2. Enter the target parameter (e.g., `25.0`).
3. Click **START TEST**.
4. Observe the live data curve plotting in real time on the graph.
5. The progress bar will fill until the sequence completes.
   *(In an emergency, click the red **STOP** button to immediately halt).*

---

### Step 5: Export & Review the Report
1. When the test completes, the status badge will indicate **PASS** or **FAIL**.
2. Click **EXPORT CSV**.
3. Choose your destination folder or accept the default path (`logs/`).
4. Open the CSV file in Microsoft Excel or Google Sheets to inspect recorded data columns and summary statistics.
