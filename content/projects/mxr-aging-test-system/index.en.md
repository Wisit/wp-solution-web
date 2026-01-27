---
title: "High-Density EV Module Burn-in Test System (20 Units/PC)"
slug: mxr-aging-test-system
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [High Performance, C#, Multithreading, CAN Bus, Mass Production]
description: "Managing simultaneous control of 20+ EV charging modules per PC with critical 10-second watchdog timer handling."
cover:
    image: "mxr-cover.jpg"
    alt: "Mass Production EV Module Testing Line"
    relative: true
---

# Mass Production Aging Test System

**Problem Statement:**
The client required a mass production testing facility for **MXR100030 (30kW)** and **MXR100050 (50kW)** charging modules. The scale involves testing **20 units simultaneously per workstation**.

**The Challenge: The 10-Second Deadline**
The most critical engineering constraint is the module's internal **Watchdog Timer**.
* If a module does not receive a valid communication packet within **10 seconds**, it automatically cuts off the DC power output.
* With 20 devices flooding the bus with telemetry data, network congestion could easily cause a "Heartbeat Miss," leading to a failed test and production downtime.

## Our Solution: High-Throughput CAN Architecture
We designed a **Multi-Threaded Control Application** capable of handling high-density traffic without latency. We moved away from a single daisy-chain loop to a **Tree Topology** using USB-CAN Hubs to distribute the bandwidth load.

![System Architecture Diagram](mxr-diagram.jpg)

### Network Topology Strategy
Instead of overloading a single CAN channel, we utilized a **USB Hub -> Multiple USB-CAN Adapters** architecture (as seen in the project schematic).
* **Load Balancing:** Splitting 20 units across multiple CAN channels prevents bus saturation.
* **Asynchronous Heartbeat:** A dedicated background thread ("Keep-Alive Service") ensures every unit receives its command packet within the 10s window, regardless of the UI's processing load.

### Key Technologies Implemented
| Technology | Role in System |
| :--- | :--- |
| **C# (TPL & Async/Await)** | Handling 20 concurrent state machines without UI freezing |
| **Priority Queueing** | Ensuring "Keep-Alive" packets have higher priority than logging data |
| **USB-CAN Aggregation** | Managing multiple COM ports as a single logical network |

## Business Results
* **Zero Downtime:** Eliminated false-positive cut-offs caused by communication timeouts.
* **Scalability:** The system successfully manages 20 units of MXR100030 per PC with stable millisecond-level response times.
* **Real-time Visibility:** Operators can monitor voltage/current of all 20 units on a single dashboard.

> **Expert Insight:**
> When controlling hardware with a strict Watchdog (like the 10s cutoff here), never rely on the main UI thread for communication. We implemented a **"Fire-and-Forget" UDP-style logic** strictly for the Keep-Alive signal to guarantee the modules stay active.

---
**Facing latency issues in your industrial network?**
Contact us at: wisit.paewkratok@gmail.com