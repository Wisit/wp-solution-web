---
title: "Visual Inspection Machine with Adaptive Master Template"
slug: honda-inspection-vision
date: 2026-01-27
author: WP Solution Team
categories: [Case Study]
tags: [LabVIEW, Machine Vision, Quality Control, Arduino, Smart Factory]
description: "Automated QC system for stamped metal parts, featuring a dynamic template update function to handle tooling wear and production variations."
cover:
    image: "honda-vision-cover.jpg"
    alt: "Metal Part Inspection System"
    relative: true
---

# Adaptive Visual Inspection for Stamped Parts

**Problem Statement:**
In automotive part manufacturing, "Stamped Characters" (serial numbers/codes) are critical for traceability. However, the physical stamping process is inconsistent. Factors like die wear, hydraulic pressure changes, or slight positioning shifts cause the characters to look different over time.

**The Challenge:**
A traditional Vision System uses a static "Master Image." When the production line's stamping tool degrades slightly, the characters no longer match the static Master perfectly. This leads to a high rate of **False Rejects** (Good parts being rejected), causing frequent machine downtime and requiring constant code adjustments by engineers.

## Our Solution: Dynamic Template Management
We developed a **Visual Inspection Machine** using **LabVIEW** and **NI Vision Module** with a specialized feature: **"Supervisor Calibration Mode."**

This feature empowers line supervisors to manually update the "Master Template" and adjust matching thresholds directly via the UI, without needing a programmer or stopping the line for extended periods.

![System Architecture Diagram](honda-vision-diagram.jpg)

### Key Features
1.  **Dual View Inspection:** Two cameras simultaneously inspect the **Top and Bottom** of the part to ensure 100% coverage.
2.  **User-Defined Master:** A dedicated "Calibrate & Setting" menu allows the supervisor to capture a current "Good Sample" and set it as the new Master Template instantly.
3.  **Real-time Scoring:** The system displays a similarity score (0-1000). If the score trends downwards due to tool wear, the supervisor can proactively update the Master before failures occur.

### Key Technologies Implemented
| Technology | Role in System |
| :--- | :--- |
| **LabVIEW** | Main Vision Processing Software & UI |
| **NI Vision Pattern Matching** | Core algorithm for character verification |
| **Arduino Controller** | Low-cost I/O interface for Pneumatic Jigs & Tower Lamps |
| **State Machine Architecture** | Managing complex sequences (Move In -> Snap -> Process -> Move Out) |

## Business Results
* **Reduced Downtime:** Supervisors can resolve "False Reject" issues in under 1 minute by updating the Master.
* **Cost Efficiency:** Using an Arduino-based controller for I/O significantly reduced the hardware cost compared to industrial PLCs.
* **Flexibility:** The system accommodates natural variations in the manufacturing process without compromising quality standards.

> **Expert Insight:**
> In Vision Systems involving metal stamping, "Lighting" is 50% of the solution. We used controlled LED lighting (Top/Bottom) triggering via Relay to create high-contrast images, minimizing the effect of ambient light reflections on the metal surface.

---
**Struggling with false rejects in your Vision System?**
Contact us at: wisit.paewkratok@gmail.com