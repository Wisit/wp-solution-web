---
title: "Ceramic Kiln HMI Modernization: Migrating to PC-Based Control"
slug: ceramic-kiln-hmi-retrofit
date: 2024-03-15
author: WP Solution Team
categories: [Case Study]
tags: [C#, .NET, Legacy Modernization, Omron PLC, Industrial PC, Serial Comm]
description: "Retrofitting a legacy ceramic kiln control system by replacing obsolete UniOP HMIs with a custom C# .NET PC-Based solution, enabling unlimited recipe storage and data analytics."
cover:
    image: "ceramic-kiln-hmi-retrofit-cover.jpg"
    alt: "Modern Industrial PC Control for Ceramic Kiln"
    relative: true
---

# Ceramic Kiln HMI Modernization

**Problem Statement:**
A leading ceramic tile manufacturer faced critical risks with their tunnel kiln control systems. The existing operation relied on obsolete **UniOP Touch Screens** and proprietary "Designer 6" software, which were no longer supported. Spare parts were scarce, and the system suffered from limited memory, preventing the storage of new production recipes. Furthermore, the legacy system lacked data logging capabilities, making it impossible to analyze fuel consumption or track alarm history efficiently.

**The Challenge:**
* **Legacy Hardware Dependency:** The kiln's core logic resided in older **Omron CQM1H/C200H PLCs** and **Ascon Temperature Controllers**. Replacing the entire PLC system would be cost-prohibitive and require extended downtime.
* **Protocol Complexity:** The new system needed to communicate directly with legacy protocols (Host Link, Modbus RTU) over RS232/RS485 without disrupting the existing control logic.

## Our Solution: PC-Based HMI & Mini-SCADA
We engineered a **PC-Based Control System** to replace the proprietary HMI hardware. By developing a custom application using **C# (.NET)**, we transformed the operator interface into a powerful, data-driven control unit.

![System Architecture Diagram](ceramic-kiln-hmi-retrofit-diagram.jpg)

### Key Technical Implementation
1.  **Custom Driver Development:** Instead of relying on off-the-shelf SCADA with expensive licensing tags, we developed a custom communication layer (Driver) in C# to interface directly with:
    * **Omron PLC:** Via Host Link Protocol (RS232/485).
    * **Ascon Controllers:** Via proprietary communication protocols.
2.  **Unlimited Recipe Management:** Leveraging the PC's storage, the new system supports unlimited production recipes, solving the legacy hardware's memory bottleneck.
3.  **Centralized Control Logic:** The software acts as a centralized brain, allowing operators to control complex parameters like fan speed and burner status directly from a unified interface.

### Technology Stack
| Technology | Role in System |
| :--- | :--- |
| **C# .NET (WPF)** | Core Application & UI/UX Development |
| **Host Link / Modbus** | Communication Protocols for Legacy Hardware |
| **SQL LocalDB / SQLite** | Data Logging, Alarm History, and Recipe Storage |
| **Industrial PC** | Ruggedized hardware for harsh factory environments |

## Business Results
* **Zero Hardware Dependency:** Eliminated the risk of obsolete HMI parts; the software now runs on standard Industrial PCs.
* **Enhanced Visibility:** Enabling **Consumption Tracking** and **Alarm History** analysis allows engineers to optimize fuel usage and reduce downtime.
* **Scalability:** The system is now ready for future integration with factory-wide ERP or IoT systems.

> **Expert Insight:**
> "Retrofitting doesn't always mean replacing everything. By using a 'Software-Defined' approach, we extended the life of the existing PLCs while providing modern capabilities like Data Logging and unlimited recipes, saving the client over 60% compared to a full system replacement."

---
**Facing issues with obsolete HMI or PLCs?**
Contact us for a modernization assessment: wisit.paewkratok@gmail.com