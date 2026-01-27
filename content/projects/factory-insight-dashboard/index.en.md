---
title: "Factory Insight: Enterprise Energy & Production Monitoring System"
slug: factory-insight-dashboard
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [Vue.js, Node-RED, IoT, Energy Monitoring, Smart Factory]
description: "A comprehensive web-based platform for real-time energy monitoring and production tracking, powered by Node-RED and Quasar Framework."
cover:
    image: "factory-insight-cover.jpg"
    alt: "Factory Insight Energy Dashboard"
    relative: true
---

# Visualizing Invisible Energy Costs

**Problem Statement:**
In large-scale manufacturing, electricity bills are a major operating cost, yet they often remain a "Black Box." [cite_start]Factory managers struggle with manual meter readings, lack of real-time visibility into power consumption trends, and inability to correlate energy spikes with specific production processes [cite: 23-24].

**The Challenge:**
The client needed a centralized system to collect data from multiple **Digital Multimeters** distributed across the factory. The system had to handle high-frequency data (Voltage, Current, Power, THD), process it for cost analysis, and present it on a responsive web dashboard accessible from anywhere.

## Our Solution: Event-Driven IoT Platform
We architected **"Factory Insight,"** a robust monitoring solution built on the **Node-RED** ecosystem for reliable data acquisition and **Vue.js (Quasar Framework)** for a high-performance frontend user interface.

![System Architecture Diagram](factory-insight-diagram.jpg)

### Key Features
1.  [cite_start]**Real-time Telemetry:** Live monitoring of electrical parameters including **Voltage (V), Current (A), Power (kW), Power Factor, and Frequency (Hz)** [cite: 61-66].
2.  [cite_start]**Power Quality Analysis:** Monitors **Total Harmonic Distortion (THD)** to detect "Dirty Power" that could damage sensitive machinery[cite: 67].
3.  [cite_start]**Cost Intelligence:** Automatically calculates estimated electricity costs ($/THB) based on consumption, helping in budget forecasting[cite: 87, 191].
4.  [cite_start]**Advanced Reporting:** Generates Daily, Weekly, and Monthly energy reports with "Export to Excel/CSV" functionality for further audit [cite: 83-85, 230].

### Key Technologies Implemented
| Technology | Role in System |
| :--- | :--- |
| **Vue.js 3 (Quasar)** | Responsive & Fast Frontend Dashboard |
| **Node-RED** | IoT Gateway, Data Aggregation, and API Backend |
| **MySQL** | Historical Data Storage |
| **Modbus RTU** | [cite_start]Communication protocol with Power Meters [cite: 239] |

## Business Results
* **Data Integrity:** Implemented "Gap Filling" logic (`aggregateBackfillSequential.js`) to ensure continuous data streams even during network hiccups.
* [cite_start]**Operational Efficiency:** Replaced manual logbooks with automated digital reports, saving hours of engineering time per week[cite: 24].
* [cite_start]**Predictive Maintenance:** THD monitoring allows early detection of motor or drive anomalies before failure occurs[cite: 67].

> **Expert Insight:**
> Reliability is key in data logging. We utilized **Node-RED** not just as a pass-through, but as a smart aggregator that pre-processes raw Modbus data into hourly and daily summaries, significantly reducing the query load on the MySQL database for long-term reporting.

---
**Ready to optimize your factory's energy usage?**
Contact us at: wisit.paewkratok@gmail.com