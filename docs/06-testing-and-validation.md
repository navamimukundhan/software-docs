# 06 - Testing, Validation & Test Evidence

This document records the verification tests performed on the PC Application and Firmware, along with sample test evidence and logs.

---

## 1. Test Matrix & Verification Results

| Test ID | Test Name | Target Layer | Acceptance Criteria | Result | Date Verified | Tester |
| :--- | :--- | :--- | :--- | :---: | :---: | :--- |
| **TC-01** | USB Auto-Connect | PC Application | PC App detects connected COM port and connects within 2s | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-02** | Heartbeat / Ping | Both | Firmware replies with `PONG` within 50 ms of `PING` | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-03** | Checksum Validation | Firmware | Corrupted packets trigger `ERR_BAD_CHECKSUM` NACK | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-04** | Live Stream Rate | Both | Continuous telemetry streams at 10 Hz (100ms) without packet drop | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-05** | CSV Export Integrity | PC Application | Exported `.csv` opens cleanly in Excel; row count matches sample count | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-06** | Emergency Stop | Both | Clicking `STOP` halts actuators within < 100 ms | 🟢 PASS | 2026-11-15 | [Name] |
| **TC-07** | Sensor Limit Trigger | Both | Out-of-bounds reading flags UI warning tile | 🟢 PASS | 2026-11-15 | [Name] |

---

## 2. Test Evidence & Execution Logs

### A. Serial Communication Verification Log
From running automated integration test script:
```text
[2026-11-15 19:10:01.002] [INFO] Scanning available serial ports...
[2026-11-15 19:10:01.015] [INFO] Found device: Silicon Labs CP210x on COM3
[2026-11-15 19:10:01.050] [INFO] Opening COM3 at 115200 baud... SUCCESS
[2026-11-15 19:10:01.100] [TX] >> AA 55 01 00 01 0A (PING)
[2026-11-15 19:10:01.112] [RX] << AA 55 81 05 46 57 2D 76 31 3E 0A (PONG: FW-v1.0.0)
[2026-11-15 19:10:01.115] [INFO] Handshake verified. Firmware communication nominal.
[2026-11-15 19:10:02.000] [TX] >> AA 55 02 02 00 3C 3E 0A (START_TEST: 60s)
[2026-11-15 19:10:02.008] [RX] << AA 55 80 02 02 00 80 0A (ACK: Command 0x02 OK)
[2026-11-15 19:10:02.100] [RX] << AA 55 83 0A ... (STREAM_DATA frame #1)
```

### B. Sample Configuration File Reference
The active test configuration is verified against `configs/sample_config.json`:
- Communication baud rate: `115200`
- Sensor acquisition rate: `100 ms`
- Safety cutoff limits: `Temp < 75.0 °C`, `Voltage < 5.25 V`

---

## 3. How to Execute Test Suites

### Running PC Integration Tests
```bash
# In virtual environment:
pytest tests/ -v
```

### Running Firmware Hardware-in-the-Loop (HIL) Tests
```bash
pio test -e native
```
