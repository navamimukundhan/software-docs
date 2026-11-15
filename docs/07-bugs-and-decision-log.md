# 07 - Dated Bug & Technical Decision Log

> **Rule:** *Maintain a dated bug/decision log and record how each issue was tested or resolved.* Update this document whenever a bug is solved or a key engineering decision is agreed upon.

---

## 1. Bug Tracking Log

| Date | Bug ID | Component | Severity | Description of Problem | Root Cause | Resolution & Verification | Status |
| :---: | :---: | :---: | :---: | :--- | :--- | :--- | :---: |
| 2026-11-10 | **BUG-001** | PC App | High | Application froze when opening COM port while firmware was streaming data. | Serial read loop ran on the main GUI thread, blocking UI event loop. | Moved `SerialReader` to a dedicated background `QThread` with Qt signals for UI updates. Tested 10 consecutive connect/disconnect cycles without freezing. | 🟢 Closed |
| 2026-11-12 | **BUG-002** | Firmware | Medium | Occasional packet drop and checksum mismatch during high-speed sampling. | Serial buffer overflow caused by `printf` debugging statements inside the interrupt service routine (ISR). | Removed blocking print calls from ISR; buffered sensor data into a DMA circular ring buffer. Verified with 10,000 packets with 0 drops. | 🟢 Closed |
| 2026-11-14 | **BUG-003** | PC App | Low | CSV exporter crashed when file was open in Microsoft Excel. | File permission error (`PermissionError: [Errno 13]`) when attempting to overwrite locked file. | Implemented try/except block; if locked, auto-appends timestamp suffix `_copy_HHMMSS.csv` and warns user. Tested with open Excel window. | 🟢 Closed |
| *[YYYY-MM-DD]* | **BUG-004** | *[Comp]* | *[Sev]* | *[Describe issue]* | *[Root cause]* | *[How it was resolved and tested]* | 🟡 Open |

---

## 2. Technical Decision Log (ADR - Architecture Decision Records)

Record key architectural choices, tool decisions, and protocol agreements to preserve project rationale:

| Decision ID | Date | Topic | Decision Made | Rationale / Alternatives Considered | Approved By |
| :---: | :---: | :--- | :--- | :--- | :--- |
| **DEC-01** | 2026-11-05 | UI Framework | Adopt **PyQt6** | Selected over Tkinter (too basic) and Electron (heavy footprint) for fast native rendering of real-time sensor graphs. | Team Consensus |
| **DEC-02** | 2026-11-08 | Protocol Framing | Use **Binary Framed Packets** with XOR checksum | Selected binary framing over raw ASCII strings to reduce UART bandwidth overhead and prevent string parsing latency on the microcontroller. | PC & FW Leads |
| **DEC-03** | 2026-11-11 | Baud Rate | Standardize on **115,200 Baud** | Provides sufficient throughput for 100 Hz sensor telemetry without requiring high-frequency crystal clock tuning. | Firmware Lead |
| **DEC-04** | 2026-11-13 | Calibration Storage | Store calibration parameters in **JSON on PC** and mirror in Firmware EEPROM | Allows PC to operate seamlessly with new hardware units while preserving hardware calibration on-board. | Systems Lead |
