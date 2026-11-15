# 05 - Calibration Settings, CSV Format & Report Calculations

This document specifies the exact data recording format, sensor calibration formulas, automated test-sequence stages, and report mathematical calculations.

---

## 1. CSV Data Logging Specification

All recorded test sessions automatically export timestamped rows to `.csv` files stored in the `/logs/` directory.

### CSV Header Structure & Column Definitions

```csv
Timestamp,Elapsed_Seconds,Sample_Index,Channel_1_Raw,Channel_1_Calibrated,Channel_2_Temp,Status_Flags,Pass_Fail
```

| Column Name | Data Type | Unit | Description | Example |
| :--- | :--- | :--- | :--- | :--- |
| `Timestamp` | String (ISO 8601) | - | Wall-clock date and time (YYYY-MM-DDTHH:MM:SS.sss) | `2026-11-15T19:20:00.125` |
| `Elapsed_Seconds` | Float | Seconds (`s`) | Time elapsed since test start | `12.450` |
| `Sample_Index` | Integer | - | Monotonically increasing sequence number | `125` |
| `Channel_1_Raw` | Integer | ADC counts | Raw 12-bit/16-bit analog-to-digital converter reading | `2048` |
| `Channel_1_Calibrated` | Float | Engineering unit (e.g. `V`, `N`, `mm`) | Value after applying gain and offset | `2.502` |
| `Channel_2_Temp` | Float | Degrees Celsius (`°C`) | Ambient or device temperature | `27.4` |
| `Status_Flags` | Hex String | Bitmask | Firmware status flags (`0x00` = OK, `0x01` = Limit reached) | `0x00` |
| `Pass_Fail` | String | - | Instantaneous check against tolerance bounds (`PASS` / `FAIL`) | `PASS` |

---

## 2. Sensor Calibration Settings & Formula

Sensors convert physical phenomena into raw electrical signals. The PC application and firmware translate raw counts into calibrated physical values using a linear two-point or multi-point calibration equation:

### Linear Calibration Formula:
$$Y = (X - \text{Offset}) \times \text{Gain}$$

Where:
* $X$: Raw sensor reading from firmware (e.g. raw ADC bits).
* $\text{Offset}$: Zero-point reading when no physical load or stimulus is applied.
* $\text{Gain}$ (Sensitivity / Scale factor): Physical quantity per ADC count.
* $Y$: Calibrated output in engineering units.

### Calibration Parameter Table
| Sensor Channel | Description | Default Offset | Default Gain | Engineering Unit | Safe Operating Range |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Channel 1** | Primary Sensor (e.g., Load/Displacement) | `0.000` | `0.001221` | mm / N | `0.0` to `50.0` |
| **Channel 2** | Secondary Sensor (e.g., Temperature) | `-10.000` | `0.050000` | °C | `0.0` to `80.0` |
| **Channel 3** | Voltage Monitor | `0.000` | `0.003300` | V | `0.0` to `5.0` |

### Step-by-Step Calibration Procedure:
1. **Zero Point Calibration:**
   - Remove any physical load/excitation from the sensor.
   - Click `CALIBRATE` -> select channel -> click `Set Zero / Offset`.
   - The system records the current raw reading as the `Offset`.
2. **Span / Gain Calibration:**
   - Apply a known reference standard (e.g., a certified 10.0 mm gauge block or 1.000V precision source).
   - Enter the known reference value into the prompt.
   - The application computes:
     $$\text{Gain} = \frac{\text{Known Reference Value}}{X_{\text{loaded}} - \text{Offset}}$$
   - Save configuration to `configs/sample_config.json`.

---

## 3. Test-Sequence Steps

Automated tests follow a sequential state machine to guarantee repeatable and safe results:

```
+------------+       +------------+       +------------+       +------------+       +------------+
| 1. PRE-TEST| ----> | 2. TARE /  | ----> | 3. RAMP /  | ----> | 4. DWELL / | ----> | 5. COMPLETE|
| HEALTH-CHK |       | ZERO-CAL   |       | STIMULUS   |       | MEASURE    |       | & REPORT   |
+------------+       +------------+       +------------+       +------------+       +------------+
```

1. **Stage 1: Pre-Test Health Check**
   - Verify serial port connection.
   - Confirm microcontroller ping responds with `STATUS_OK`.
   - Ensure temperature and voltage sensors are within safe initial limits.
2. **Stage 2: Tare / Zero-Cal**
   - Apply tare offset to zero baseline readings.
3. **Stage 3: Stimulus / Ramp**
   - Command actuator or test profile to ramp up toward the target value at specified rate.
4. **Stage 4: Dwell & Data Logging**
   - Maintain target value for specified dwell duration (e.g., 60 seconds).
   - Record readings at specified sampling interval (e.g., 100 ms) into the CSV buffer.
5. **Stage 5: Test Completion & Safe Park**
   - Command hardware to turn off / park in idle mode.
   - Compute report summary metrics.
   - Auto-save CSV file and display final Pass/Fail badge to operator.

---

## 4. Report Calculations & Summary Metrics

At the conclusion of each test sequence, the software calculates summary statistics over the recorded data array:

| Metric | KaTeX Formula | Description |
| :--- | :--- | :--- |
| **Mean (Average)** | $$\bar{y} = \frac{1}{N} \sum_{i=1}^{N} y_i$$ | Average reading across steady-state test duration |
| **Peak-to-Peak (Span)** | $$\Delta y = y_{\max} - y_{\min}$$ | Absolute variation / noise level during steady-state |
| **Standard Deviation** | $$\sigma = \sqrt{\frac{1}{N-1}\sum_{i=1}^{N}(y_i - \bar{y})^2}$$ | Dispersion / measurement repeatability indicator |
| **Percent Error** | $$\text{Error} \% = \left| \frac{\bar{y} - y_{\text{target}}}{y_{\text{target}}} \right| \times 100\%$$ | Accuracy compared against set target value |
| **Pass/Fail Decision** | $\text{Result} = (\text{Error} \% \le \text{Tolerance}) \land (\Delta y \le \text{Threshold})$ | Overall outcome marked in report |
