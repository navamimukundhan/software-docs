# Software Documentation & User Guide

> **Project Name:** NIDAR 2  
> **Repository / Codebase:** https://github.com/navamimukundhan/software-docs  
> **Documentation Maintainer:** Navami P M  
> **Last Updated:** 2026-11-15  
> **Status:** Active / Under Continuous Development

---

## 📌 Overview

This repository contains the complete technical documentation, architecture specifications, communication protocols, user manual, calibration records, and handover checklists for the software system.

The system consists of two primary components:
1. **PC Application (Host GUI / Controller):** Operator interface for controlling tests, visualising data, setting calibration values, and logging records.
2. **Firmware (Target Hardware):** Microcontroller/embedded software responsible for sensor acquisition, actuator control, and real-time execution.

---

## 📂 Documentation Directory

| Document | Description | Status |
| :--- | :--- | :--- |
| [**01. Setup & Installation**](docs/01-setup-and-installation.md) | Languages, tools, library versions, environment setup, build & flash guides. | 🟢 Ready |
| [**02. System Architecture**](docs/02-system-architecture.md) | High-level system diagram, module breakdown, data flows, and team ownership. | 🟢 Ready |
| [**03. Command & Data Format**](docs/03-command-and-data-format.md) | Protocol specification between PC app and Firmware (packet structure, opcodes). | 🟢 Ready |
| [**04. UI & Operation Guide**](docs/04-user-interface-and-operation.md) | Beginner-friendly explanation of all UI controls, buttons, graphs, and settings. | 🟢 Ready |
| [**05. Calibration & Data Specs**](docs/05-calibration-and-data-spec.md) | CSV schema definitions, calibration formulas, test sequence steps, calculations. | 🟢 Ready |
| [**06. Testing & Validation**](docs/06-testing-and-validation.md) | Test cases, verification records, test evidence, and sample configs. | 🟢 Ready |
| [**07. Bugs & Decision Log**](docs/07-bugs-and-decision-log.md) | Chronological log of bugs encountered, architectural decisions, and fixes. | 🟢 Ready |
| [**08. Known Issues & Limitations**](docs/08-known-issues-and-limitations.md) | Known bugs, hardware limitations, edge cases, and workarounds. | 🟢 Ready |
| [**09. Quick-Start Guide**](docs/09-quick-start-guide.md) | Step-by-step 5-minute operator walkthrough to run the system from scratch. | 🟢 Ready |
| [**10. Handover Checklist**](docs/10-handover-checklist.md) | Final deliverable verification, code clean-up, and handover sign-off checklist. | 🟢 Ready |

---

## ⚡ Daily Maintenance Rule

> **Important:** *Documentation is continuous: update it every working day, not just at the end.*

- **Every morning:** Review any pull requests, firmware updates, or protocol changes.
- **During development:** If a command packet changes or a new button is added to the UI, immediately update `03-command-and-data-format.md` or `04-user-interface-and-operation.md`.
- **End of day:** Add resolved bugs and key technical decisions to `07-bugs-and-decision-log.md`.
