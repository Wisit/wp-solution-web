---
title: "Smart AGV Fleet Management System (1.2 Ton)"
slug: agv-fleet-management
date: 2026-01-26
author: WP Solution Team
categories: [Case Study]
tags: [AGV, C#, MQTT, Traffic Control, Smart Factory]
description: "Advanced traffic control system for 1.2-ton Forklift AGVs in a tire manufacturing plant. Features real-time routing, collision avoidance, and seamless machine integration."
image: agv-fleet-cover.jpg
---
![ภาพปกระบบ AGV](images/agv-fleet-cover.jpg)

# Smart Fleet Management for 1.2 Ton Forklift AGVs

**Problem Statement:**
A leading tire manufacturer faced bottlenecks in transporting heavy rubber rolls (1.2 tons) between the Iron MC machine and the Liner/Stock rails. The manual process was slow, prone to accidents, and difficult to track in real-time.

**The Challenge:**
* **Heavy Load:** Handling 1.2-ton payloads requires precise and safe navigation.
* **Complex Traffic:** Multiple AGVs operating in shared aisles with human workers.
* **Legacy Integration:** Need to communicate with existing machines via varied protocols.

## Our Solution: Intelligent Traffic Control

WP Solution architected a comprehensive **Fleet Management System** that acts as the "Air Traffic Control" for the factory floor. The system manages job queues, calculates optimal routes, and prevents traffic jams using a dynamic node-based algorithm.

![System Architecture Diagram](agv-fleet-diagram.jpg)

### Key Features Implemented

* **Dynamic Routing Algorithm:** Calculates the shortest path and reserves nodes to prevent deadlocks.
* **Real-time Visualization:** A dashboard showing live AGV positions, battery status, and job progress.
* **Collision Avoidance:** Interlocks with Area Scanners (SICK) and localized E-Stops.
* **Auto-Charging Management:** Automatically sends AGVs to charging stations when battery levels are critical.

### Technology Stack

| Technology | Role in System |
| :--- | :--- |
| **C# (.NET)** | Core logic for Traffic Manager & Pathfinding |
| **MariaDB** | Storing job history, error logs, and performance metrics |
| **MQTT / TCP** | Low-latency communication between Server and AGVs |
| **Node-RED** | IoT Gateway for connecting with legacy PLC machines |

## Business Results

* ✅ **100% Traceability:** Every movement is logged for process optimization.
* ✅ **Zero Traffic Jams:** Intelligent routing eliminated deadlock situations.
* ✅ **24/7 Operation:** Automated battery management ensures continuous uptime.

> **Expert Insight:**
> "The key to successful AGV implementation isn't just the robot hardware; it's the **Traffic Logic**. We designed a 'Reservation Protocol' where AGVs must request permission before entering any intersection, ensuring absolute safety."

---
**Looking to automate your internal logistics?**
Contact us at: wisit.paewkratok@gmail.com