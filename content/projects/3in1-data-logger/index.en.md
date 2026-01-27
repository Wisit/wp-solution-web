---
title: "3-in-1 Industrial Data Logger (Weight/Thickness/Length)"
slug: 3in1-data-logger
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [Industrial IoT, C#, WPF, Modbus TCP, Smart Factory]
description: "A real-time quality control data logger connecting PC to PLC via Modbus, replacing manual recording for critical production parameters."
cover:
    image: "3in1-cover.jpg"
    alt: "Industrial Data Logger Dashboard"
    relative: true
---

# Digitalizing Quality Control Data

**Problem Statement:**
Manual data recording in high-speed manufacturing is prone to "Human Error" and creates "Data Silos." At ETON INDUSTRY, operators had to manually write down critical parameters (Weight, Thickness, Length) for every finished product, leading to delayed reporting and inaccurate traceability.

**The Challenge:**
The system needed to interface directly with the existing machine's PLC to capture data exactly when the process finishes, without disrupting the operator's workflow. It also required a robust "Target Counter" to alert operators when a specific order quantity was met.

## Our Solution: Real-time Modbus Logger
We developed a **Windows Desktop Application (WPF)** that acts as a bridge between the Machine and the QA Department. The software polls the PLC via **Modbus TCP** and automatically logs data whenever the machine triggers the "Finish Signal" (Bit W1.1).

![System Architecture Diagram](3in1-diagram.jpg)

### Key Features
1.  **3-Parameter Logging:** Automatically captures **Weight (D20), Thickness (D22), and Length (D24)** from the PLC memory with precision decimal handling.
2.  **Production Counter Control:** A visual "PV / SV" counter (e.g., 19 / 21) that alerts the operator via a popup when the production target is reached.
3.  **Data Management:**
    * **Auto-Retention:** A background scheduler automatically deletes data older than 3 months to optimize storage.
    * **Flexible Export:** QA teams can export specific date ranges to **CSV/Excel** for reporting.

### Key Technologies Implemented
| Technology | Role in System |
| :--- | :--- |
| **C# (.NET / WPF)** | Responsive UI and Business Logic |
| **Modbus TCP** | Industrial Protocol for PLC Communication (tested with Omron CP2E) |
| **EasyModbus Lib** | Reliable driver for register reading and polling |

## Business Results
* **100% Data Accuracy:** Eliminated manual transcription errors by pulling raw data directly from sensors.
* **Real-time Visibility:** Supervisors can see the "Last Data" logged instantly on the dashboard.
* **Efficiency:** Reduced reporting time from hours to seconds with the one-click Export feature.

> **Expert Insight:**
> Implementing a "Polling Logic" requires careful latency management. We configured the system to check the Trigger Bit (W1.1) with low latency (<100ms) to ensure no production cycle is missed, even during high-speed operation.

---
**Need to connect your machines to IT systems?**
Contact us at: wisit.paewkratok@gmail.com