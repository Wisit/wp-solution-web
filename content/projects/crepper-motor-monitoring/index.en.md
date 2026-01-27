---
title: "Real-time Crepper Motor Monitoring System (IoT Dashboard)"
slug: crepper-motor-monitoring
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [IoT, Node-RED, Vue.js, MySQL, Modbus RTU]
description: "Web-based SCADA system for monitoring heavy machinery motors using Node-RED and Vue.js, delivering real-time insights and historical data analysis."
cover:
    image: "crepper-cover.jpg"
    alt: "Crepper Motor Monitoring Dashboard"
    relative: true
---

# Real-time Crepper Motor Monitoring System

**Problem Statement:**
In a large-scale rubber processing plant, the "Crepper Machines" are the heart of production. [cite_start]The client, **Sri Trang Agro-Industry PCL**, needed to monitor the electrical current (Amps) of 3 critical motors in real-time[cite: 2, 4]. Previously, operators had to manually check readings from local panel meters, making it impossible to analyze historical trends or detect anomalies (like motor overload) instantly.

**The Challenge:**
* [cite_start]**Legacy Connectivity:** The existing meters were standard Digital Meters (Primus TCM-94N-AB) communicating via Modbus RTU over RS-485, requiring a reliable bridge to the IT network[cite: 4, 6].
* **Data Integrity:** The system needed to handle potential connection drops and ensure data continuity for accurate reporting.
* **User Accessibility:** The dashboard had to be accessible via a web browser on the local network without complex software installation on client machines.

## Our Solution: Hybrid IoT Gateway

WP Solution architected a **Web-based Monitoring System** that bridges the gap between the factory floor and the control room. We utilized **Node-RED** as a robust middleware to poll data from the meters and **Vue.js** for a high-performance frontend dashboard.

![System Architecture Diagram](crepper-diagram.jpg)

### System Workflow
1.  [cite_start]**Data Acquisition:** An Industrial PC connects to 3 Digital Meters via **RS-485 (Modbus RTU)**[cite: 6, 17].
2.  [cite_start]**Processing (Node-RED):** Node-RED polls current, voltage, and power data every second, processing and filtering signal noise[cite: 11].
3.  [cite_start]**Storage (MySQL):** Processed data is logged into a **MySQL Database (V8.x)** for long-term historical analysis[cite: 10].
4.  [cite_start]**Visualization (Vue.js):** A responsive web dashboard displays real-time gauges and historical line charts, allowing engineers to track motor health instantly[cite: 12].

### Key Technologies Implemented

| Technology | Role in System |
| :--- | :--- |
| **Node-RED** | [cite_start]IoT Gateway & Logic Controller (Low-code backend) [cite: 11] |
| **Vue.js** | [cite_start]Reactive Frontend Framework for smooth Dashboard UI [cite: 12] |
| **MySQL** | [cite_start]Relational Database for storing historical motor data [cite: 10] |
| **Modbus RTU** | [cite_start]Industrial Protocol for communicating with Digital Meters [cite: 6] |

## Business Results
* ✅ **Real-time Visibility:** Engineers can monitor motor load from the office, reducing inspection time.
* [cite_start]✅ **Predictive Analysis:** Historical data graphs allow for early detection of motor wear or abnormal load patterns[cite: 5].
* [cite_start]✅ **Cost Effective:** Built on Open Source technologies (Node-RED, Vue.js, MySQL), significantly reducing licensing costs[cite: 10, 11, 12].

> **Expert Insight:**
> "Using Node-RED as a middleware allowed us to rapidly prototype and deploy the Modbus communication logic. Combined with Vue.js, we delivered a 'SCADA-like' experience in a fraction of the time and cost of traditional industrial software."

---
**Ready to upgrade your factory?**
Contact us at: wisit.paewkratok@gmail.com