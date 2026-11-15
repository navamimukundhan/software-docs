# 08 - Known Issues, Limitations & Workarounds

This document outlines known operational boundaries, hardware constraints, and temporary workarounds to assist operators, developers, and future maintainers.

---

## 1. Known Software Issues & Workarounds

| Issue ID | Description | Impact | Workaround / Recommended Procedure | Target Fix Date |
| :---: | :--- | :--- | :--- | :---: |
| **ISSUE-01** | Unplugging the USB cable while a test is actively streaming causes an unhandled disconnect notification. | Moderate (Requires app restart) | Always click **STOP / DISCONNECT** before physically removing the USB cable from the PC. | Next Sprint |
| **ISSUE-02** | High DPI displays (> 150% scaling) may cause minor button clipping on smaller laptop screens. | Minor (Aesthetic) | Set Windows display scaling to 100% or 125%, or maximize the application window. | Next Release |
| **ISSUE-03** | Rapidly toggling the START/STOP button within < 200 ms can queue duplicate commands. | Low | Button is now disabled for 500 ms after click to prevent spam clicking. | Resolved |

---

## 2. Hardware & Operating Limitations

1. **Maximum Continuous Sampling Rate:**
   - The current serial link (115,200 baud) reliably supports telemetry rates up to **100 Hz** (10 ms period). Setting the sampling rate faster than 10 ms may lead to buffer overflow or missed frames.
2. **Voltage Input Thresholds:**
   - Analog inputs are rated for **0.0V to 3.3V DC**. Direct application of 5V or higher signals without an external voltage divider will damage the microcontroller ADC pin.
3. **USB Hub Latency:**
   - Using unpowered USB multiport dongles can introduce packet jitter. Connect directly to a motherboard USB port for critical calibration runs.
4. **Operating System Compatibility:**
   - Tested primarily on **Windows 10 / Windows 11 (64-bit)**.
   - Linux and macOS operation require verifying user permissions for serial access (`sudo usermod -a -G dialout $USER`).
