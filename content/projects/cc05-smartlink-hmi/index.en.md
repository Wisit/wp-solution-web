---
title: "Automated Cutting Machine HMI System (CC05 SmartLink)"
date: 2026-03-18
author: WP Solution Team
categories: [Case Study]
tags: [C#, WPF, .NET 8, Mitsubishi PLC, SQL Server, ZPL Printer]
description: "An integrated HMI system for an automated cutting machine, featuring barcode scanning, SQL database integration, FX5U PLC control, and direct Zebra ZPL printing to minimize operational errors."
cover:
    image: "cc05-smartlink-cover.jpg"
    alt: "CC05 SmartLink System Cover"
    relative: true
---

# Automated Cutting Machine HMI System (CC05 SmartLink)

**The Challenge:**
The client required a modern Human-Machine Interface (HMI) to automate the workflow of their CC05 Cutting Machine. The system needed to bridge the gap between operator barcode scanning, centralized production databases, machine PLCs, and label printers in a seamless manner. 

**Key Difficulties:**
* Establishing direct communication with Mitsubishi FX5U PLCs over a TCP/IP LAN without relying on expensive third-party OPC servers.
* Automating the label printing process by sending raw commands directly to Zebra printers without using standard Windows print drivers, ensuring zero delay.
* Providing robust offline capabilities for operation history logging and instant label reprinting in case of hardware or network issues.

## Solution Architecture
WP Solution developed **CC05 SmartLink**, an industrial-grade C# .NET 8 WPF application acting as a comprehensive middleware HMI. The workflow is entirely barcode-driven; operators simply scan a Control Number or Order Number to automatically retrieve production parameters (such as Cut Length, Project No, and Model) from a central SQL Server. Once the machine is commanded to start, the system communicates directly with the PLC, listens for the "Machine Complete" signal, and dynamically generates and prints Zebra ZPL labels.

![System Architecture Diagram](cc05-smartlink-diagram.jpg)

### Technology Stack
* **C# .NET 8 & WPF (MVVM):** Built the modern user interface using the MVVM pattern with `CommunityToolkit.Mvvm` for high responsiveness and maintainability.
* **Mitsubishi MC Protocol (3E Frame):** Developed a custom TCP Socket driver to read and write directly to the PLC's Data Registers (D) and internal relays (M) via SLMP.
* **Raw ZPL II API:** Implemented direct USB Spooler API printing using Raw ZPL II commands for fast, dynamic label generation.
* **SQL Server & SQLite:** Utilized SQL Server for centralized production data and a local SQLite database (`Logs.db`) for secure operator login management and persistent operation history logging.

## Business Impact
* ✅ **Reduced Human Error:** The Barcode-Driven Workflow ensures precise cut lengths and prevents the machine from operating with invalid parameters.
* ✅ **Cost Optimization:** Direct PLC Integration eliminated the need for costly third-party OPC server licenses.
* ✅ **Operational Resilience:** The Offline History feature allows operators to browse past records via a fast Date Filter and instantly reprint labels (including the original Operator ID) without querying the central server.
* ✅ **Enhanced Safety & Fail-Safe:** Prominent fullscreen alerts for SQL/PLC disconnections prevent material waste and equipment damage.

> **Engineering Insight:**
> By developing our own custom Mitsubishi MC Protocol (3E Frame) driver in C#, we not only bypassed OPC server licensing but also gained absolute control over the precise handshake timing. This allowed us to instantly detect the `M100 (Auto CutType)` or `M101 (Manual CutType)` completion flags and trigger the Zebra printer with millisecond accuracy.

---
**Looking for an Industrial Automation Architect?**
Contact us: wisit.paewkratok@gmail.com | Line: https://line.me/ti/p/~wisit.p