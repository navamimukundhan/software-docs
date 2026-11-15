# 10 - Software Handover & Sign-Off Checklist

This checklist confirms that all software, documentation, firmware binaries, and test artifacts are properly finalized and verified prior to project handover or evaluation.

---

## 📋 Pre-Handover Verification Matrix

### 1. Codebase & Repository Integrity
- [ ] Source code is cleanly formatted and committed to the main branch.
- [ ] No hardcoded personal file paths (e.g. `C:\Users\username\...`) exist in scripts or configs; all paths are relative.
- [ ] `requirements.txt` contains pinned package versions without missing dependencies.
- [ ] `.gitignore` is properly configured (excludes `.venv`, `__pycache__`, `.pio`, temporary `.csv` files).
- [ ] All sensitive credentials or tokens (if any) are excluded from the repository.

### 2. Firmware Deliverables
- [ ] Source code compiles with `0 errors` and `0 warnings`.
- [ ] Compiled binary (`.bin` or `.hex`) is saved in the `/firmware/bin/` directory for direct flashing.
- [ ] Flashing commands and instructions are documented and tested on a clean machine.
- [ ] Default baud rate (`115200`) and pin assignments are documented.

### 3. Documentation Completeness
- [ ] **Setup Guide:** Verified that a new developer can clone and run the software using only document `01`.
- [ ] **Architecture Diagram:** Mermaid diagram accurately reflects the final implemented modules.
- [ ] **ICD / Command Spec:** All command opcodes and response payloads match the actual running firmware.
- [ ] **UI Controls Guide:** Every button, slider, and plot is explained in beginner-friendly terms.
- [ ] **Calibration & CSV Spec:** Column names and units match actual exported CSV outputs.
- [ ] **Bugs & Decisions:** All solved issues are logged with root causes and resolutions.
- [ ] **Known Limitations:** Edge cases, maximum sampling limits, and electrical boundaries are noted.
- [ ] **Quick-Start Guide:** Verified step-by-step in under 5 minutes.

### 4. Testing & Quality Assurance
- [ ] All unit and integration test cases passed (`TC-01` through `TC-07`).
- [ ] Sample CSV output validated in Microsoft Excel.
- [ ] Emergency stop feature tested under live load.
- [ ] Sample configuration file (`configs/sample_config.json`) validated.

---

## ✍️ Handover Sign-Off Form

| Role | Name | Signature / Approval | Date |
| :--- | :--- | :--- | :--- |
| **Documentation Maintainer** | *[Your Name]* | ___________________ | 2026-11-15 |
| **PC Software Lead** | *[Name]* | ___________________ | 2026-11-15 |
| **Firmware Lead** | *[Name]* | ___________________ | 2026-11-15 |
| **System / Project Lead** | *[Name]* | ___________________ | 2026-11-15 |
| **Client / Evaluator** | *[Name]* | ___________________ | 2026-11-15 |
