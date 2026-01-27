---
title: "Rubber Compounding & Precision Weighing Control System"
slug: rubber-weighing-mixing-control
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [C#, MySQL, SQLite, Mettler Toledo, PLC, Smart Factory]
description: "A precision weighing and mixing control system for MB/CMB lines, featuring offline resilience (SQLite) and strict formula enforcement."
cover:
    image: "rubber-weighing-mixing-control-cover.jpg"
    alt: "Rubber Compounding Control System Interface"
    relative: true
---

# Rubber Compounding & Precision Weighing Control System

**Problem Statement:**
In the Master Batch (MB) and Carbon Master Batch (CMB) production lines of a leading automotive rubber manufacturer, manual chemical weighing processes were prone to human error. Deviations from the specific rubber formula resulted in quality issues, waste, and a lack of digital traceability.

**The Challenge:**
* **Precision Requirement:** The system must interface directly with high-precision scales to ensure chemical weights fall within strict tolerance levels.
* **Network Instability:** Production cannot stop even if the factory network/server goes down.
* **Inventory Control:** Need real-time stock deduction and validation to prevent incorrect material usage (wrong lot/wrong type).

## Our Solution: Smart Weighing & Interlock System

We developed a robust Windows Forms Application (C# .NET) that acts as the "Process Controller" for the mixing line. The system integrates IT and OT layers to enforce strict process control.

![System Architecture Diagram](rubber-weighing-mixing-control-diagram.jpg)

### Key Features
1.  **Formula & Plan Management:** Retrieves production plans from the central MySQL Server. The system strictly locks the machine; operators cannot proceed if the raw material weight or type does not match the formula.
2.  **Hybrid Database Architecture (Offline Mode):** A critical feature for reliability. The system caches data locally using **SQLite**. If the network fails, the production line continues seamlessly, syncing data back to the Master DB once connectivity is restored.
3.  **Hardware Interlock:**
    * **Mettler Toledo Integration:** Real-time weight reading (IND890).
    * **PLC Handshake:** The software sends "OK/NG" signals to the PLC to allow or block the mixing process.

### Key Technologies Implemented
| Technology | Role in System |
| :--- | :--- |
| **C# .NET (WinForms)** | Main control application and user interface |
| **MySQL** | Centralized Master Database (Formula, Plan, Stock) |
| **SQLite** | Local Backup Database for offline resilience |
| **Mettler Toledo** | High-precision weighing scale integration |
| **PLC Integration** | Machine control and status monitoring |

## Business Results
* **100% Traceability:** Every batch produced is tracked with exact chemical weights, lot numbers, and timestamps.
* **Zero Formula Error:** The system physically prevents adding incorrect materials or wrong weights via PLC interlocks.
* **High Availability:** The Local Caching (SQLite) feature eliminated downtime caused by network fluctuations.

> **Expert Insight:**
> In critical production lines like Rubber Compounding, never rely solely on a central server. Implementing a **"Local First"** architecture with SQLite ensures that the factory keeps running even when the IT infrastructure faces issues.

---
**Looking to optimize your batch production?**
Contact us at: wisit.paewkratok@gmail.com