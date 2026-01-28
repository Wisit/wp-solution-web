---
title: "คู่มือเขียน PLC ด้วย Structured Text (ST) Ep.5: สถาปัตยกรรม State Machine (The Sequence Logic)"
slug: st-programming-ep5-state-machine
date: 2026-02-01
author: Wisit P. (Senior Architect)
categories: [Technical Guide]
tags: [CODESYS, Structured Text, State Machine, Design Pattern, Best Practices]
description: "เปลี่ยนโค้ด Spaghetti ที่ยุ่งเหยิงให้เป็นระเบียบด้วย State Machine Design Pattern เทคนิคสำคัญที่จะเปลี่ยนคุณจากคนเขียน PLC ทั่วไป ให้เป็น Automation Software Engineer"
cover:
    image: "st-ep5-cover.jpg"
    alt: "State Machine Design Pattern"
    relative: true
# --- Theme Config ---
ShowToc: true
TocOpen: true
ShowReadingTime: true
---

ในฐานะ **CODESYS Solutions Architect** ผมขอแนะนำว่าคุณมาถูกทางแล้วครับ การเปลี่ยนจาก Logic แบบ **"If-This-Then-That"** ที่พันกันยุ่งเหยิง (Spaghetti Code) มาเป็น **State Machine Design Pattern** คือก้าวสำคัญที่จะเปลี่ยนคุณจาก "คนเขียน PLC ทั่วไป" ให้เป็น **"Automation Software Engineer"** ครับ

สำหรับ C# Developer แนวคิดนี้คือการนำ State Design Pattern มาใช้คู่กับโครงสร้าง `switch...case` เพื่อควบคุม Flow การทำงานของเครื่องจักรให้เป็นระเบียบและคาดเดาได้ครับ

## 1. Concept: State Variable & CASE Statement
ใน C# คุณอาจคุ้นเคยกับ `enum` และ `switch` เพื่อจัดการ State ของ Application ใน CODESYS เราก็ทำแบบเดียวกันครับ:

1.  **State Variable:** คือตัวแปรที่ทำหน้าที่เป็น "Pointer" บอกว่าตอนนี้เครื่องจักรอยู่ที่ขั้นตอนไหน (Step ไหน) โดยเรามักจะใช้ `INT` (0, 10, 20...) หรือ `ENUM` (Idle, Run, Error)
2.  **CASE Statement:** คือคำสั่งที่ใช้แยกการทำงานของแต่ละ State ออกจากกันอย่างเด็ดขาด ในแต่ละรอบสแกน (PLC Cycle) จะมีเพียง **1 State** เท่านั้น ที่ถูกประมวลผล

> **กฎเหล็กของ State Machine:**
> * **Do Action:** ใน State นี้ ให้ทำอะไร? (เช่น สั่งมอเตอร์หมุน)
> * **Check Transition:** เมื่อไหร่จะไป State ถัดไป? (เช่น เมื่อเซนเซอร์ทำงาน)
> * **Change State:** เปลี่ยนค่า State Variable ไปเป็นค่าถัดไป (เช่น `iState := 20;`)

## 2. ตัวอย่าง Code: Conveyor System (State Machine)
เพื่อให้ Code ดูเป็นมืออาชีพ (Clean Code) และอ่านง่าย ผมแนะนำให้ใช้ **ENUM** แทนตัวเลข 0, 10, 20 ครับ เพราะในอนาคตถ้าคุณกลับมาอ่าน `CASE 10` คุณอาจลืมว่ามันคืออะไร แต่ถ้าเจอ `CASE eState.RUNNING` คุณจะเข้าใจทันที

### Step 1: สร้าง ENUM (DUT)
คลิกขวาที่ Application -> Add Object -> DUT -> เลือก Enumeration ตั้งชื่อ `eConveyorState`

```pascal
{attribute 'qualified_only'}
{attribute 'strict'}
TYPE eConveyorState :
(
    STANDBY := 0,      // รอคำสั่ง Start
    RUNNING := 10,     // สายพานกำลังหมุน
    DETECTED := 20,    // เจอชิ้นงาน (หยุดรอ)
    EJECT := 30        // ดีดชิ้นงานออก
);
END_TYPE

```

### Step 2: เขียนโปรแกรม (POU: PLC_PRG)

เราจะใช้ **Timer (TON)** ช่วยในขั้นตอน Eject เพื่อให้ก้านสูบทำงานค้างไว้สักพักก่อนกลับไปเริ่มใหม่ (ถ้าไม่หน่วงเวลา มันจะทำงานเร็วมากจนมองไม่ทัน)

```pascal
PROGRAM PLC_PRG
VAR
    // --- Inputs ---
    xStartBtn   : BOOL;
    xSensor     : BOOL;

    // --- Outputs ---
    xConveyor   : BOOL;
    xPiston     : BOOL;
    xLampReady  : BOOL;

    // --- Internal Logic ---
    CurrentState : eConveyorState := eConveyorState.STANDBY; // State Variable
    fbEjectTimer : TON; // Timer สำหรับหน่วงเวลาดีดของ
END_VAR

// =============================================================
// State Machine Logic
// =============================================================
CASE CurrentState OF

    // ---------------------------------------------------------
    // State 0: Standby - รอคนกดปุ่ม Start
    // ---------------------------------------------------------
    eConveyorState.STANDBY:
        // Action:
        xConveyor  := FALSE;
        xPiston    := FALSE;
        xLampReady := TRUE; // ไฟสถานะพร้อมทำงานติด

        // Transition: ถ้ากดปุ่ม Start ให้ไป Running
        IF xStartBtn THEN
            xLampReady := FALSE;
            CurrentState := eConveyorState.RUNNING;
        END_IF

    // ---------------------------------------------------------
    // State 10: Running - เดินสายพานรอของมา
    // ---------------------------------------------------------
    eConveyorState.RUNNING:
        // Action:
        xConveyor := TRUE;

        // Transition: ถ้าเซนเซอร์เจอของ ให้หยุด (ไป State Detect)
        IF xSensor THEN
            CurrentState := eConveyorState.DETECTED;
        END_IF

    // ---------------------------------------------------------
    // State 20: Detected - ของมาถึงแล้ว หยุดสายพาน
    // ---------------------------------------------------------
    eConveyorState.DETECTED:
        // Action:
        xConveyor := FALSE;

        // Transition: หยุดแล้วเปลี่ยนไป Eject ทันที (หรือจะหน่วงเวลาก่อนก็ได้)
        CurrentState := eConveyorState.EJECT;

    // ---------------------------------------------------------
    // State 30: Eject - ดีดของออก (หน่วงเวลา 1 วินาที)
    // ---------------------------------------------------------
    eConveyorState.EJECT:
        // Action: สั่ง Piston ทำงาน
        xPiston := TRUE;

        // Timer Logic: เริ่มจับเวลาเมื่อเข้ามาใน State นี้
        fbEjectTimer(IN := TRUE, PT := T#1S);

        // Transition: เมื่อเวลาครบ 1 วินาที
        IF fbEjectTimer.Q THEN
            // Cleanup: รีเซ็ต Timer และ Piston ก่อนออก
            fbEjectTimer(IN := FALSE);
            xPiston := FALSE;

            // Loop กลับไปรอรอบใหม่
            CurrentState := eConveyorState.STANDBY;
        END_IF

    // ---------------------------------------------------------
    // Defensive Programming: กรณี State หลุด (Safety)
    // ---------------------------------------------------------
    ELSE
        CurrentState := eConveyorState.STANDBY;
        xConveyor := FALSE;
        xPiston := FALSE;

END_CASE;

```

## 3. สรุปข้อดี: State Machine vs. Spaghetti Code

ในมุมมองของ Architect การใช้ State Machine ช่วยแก้ปัญหา Classic ของงาน PLC ได้ดังนี้:

| คุณสมบัติ | Spaghetti Code (เรียง IF ต่อกันยาวๆ) | State Machine (CASE) |
| --- | --- | --- |
| **การทำงาน (Execution)** | PLC จะเช็คทุก IF ในทุกรอบสแกน แม้ไม่จำเป็น (กิน CPU) | PLC ประมวลผล **เฉพาะ State ปัจจุบัน** เท่านั้น (Efficient) |
| **การแก้บั๊ก (Debugging)** | หายากมากว่าตอนนี้เครื่องจักรติดเงื่อนไขบรรทัดไหน | รู้ทันทีว่าติดที่ Step ไหน (ดูค่าตัวแปร `CurrentState`) |
| **ความยืดหยุ่น (Scalability)** | จะแทรกขั้นตอนตรงกลางต้องรื้อ Logic เก่าที่มี Interlock พันกัน | แค่เพิ่ม `CASE` ใหม่ (เช่น State 15) แล้วเปลี่ยนตัวเลข Transition ก็จบ |
| **ความปลอดภัย (Safety)** | เสี่ยงที่จะสั่ง Output ชนกัน (เช่น สั่งเดินหน้าพร้อมถอยหลัง) | แต่ละ State แยกขาดจากกัน ทำให้ควบคุม Output ได้แม่นยำ ไม่ตีกันเอง |

> **คำแนะนำเพิ่มเติม:**
> สำหรับงานจริงที่ซับซ้อนกว่านี้ คุณสามารถใช้ **Methods** ในการเขียน Action ของแต่ละ State แยกออกไป เพื่อให้ Main Program สะอาดตาขึ้น ซึ่งตรงกับหลักการ **Single Responsibility Principle** ใน OOP ที่คุณคุ้นเคยครับ

---

**บทสรุปซีรีส์:**
ยินดีด้วยครับ! คุณได้เรียนรู้พื้นฐานสำคัญของ Structured Text ครบทั้ง 5 บทแล้ว ตั้งแต่ Syntax, Logic, Loops, Function Blocks จนถึง State Machine Design Pattern ตอนนี้คุณพร้อมแล้วที่จะลุยงาน Automation ระดับมืออาชีพด้วยมาตรฐานสากลครับ
