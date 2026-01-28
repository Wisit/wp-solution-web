---
title: "คู่มือเขียน PLC ด้วย Structured Text (ST) Ep.3: การวนลูปและการจัดการข้อมูลชุด (Loops & Arrays)"
slug: st-programming-ep3-loops-arrays
date: 2026-01-30
author: Wisit P. (Senior Architect)
categories: [Technical Guide]
tags: [CODESYS, Structured Text, Loops, Arrays, Watchdog]
description: "ทำไมการเขียน Loop ใน PLC ถึงอันตราย? เรียนรู้วิธีจัดการ Arrays และ Loops (FOR, WHILE, REPEAT) อย่างปลอดภัย พร้อมรู้จักกับ Watchdog Timer ศัตรูตัวฉกาจของ Infinite Loop"
cover:
    image: "st-ep3-cover.jpg"
    alt: "Loops and Arrays in CODESYS"
    relative: true
# --- Theme Config ---
ShowToc: true
TocOpen: true
ShowReadingTime: true
---

ในฐานะ **CODESYS Solutions Architect** ผมขอแสดงความยินดีที่คุณเริ่มเห็นพลังของ **Structured Text (ST)** ครับ นี่คือจุดที่ C# Developer จะได้เปรียบอย่างมหาศาล เพราะการจัดการข้อมูลจำนวนมาก (Arrays) และการวนซ้ำ (Loops) ใน Ladder Logic นั้นค่อนข้างยุ่งยากและกินพื้นที่หน้าจอ แต่ใน ST คุณสามารถเขียนจบได้ในไม่กี่บรรทัดครับ

บทความนี้เราจะเจาะลึกเรื่องการวนลูป โดยเน้นเรื่อง **"ความปลอดภัยในระดับอุตสาหกรรม"** ที่ Dev สาย PC มักจะพลาดกันครับ

## 1. FOR Loop และการจัดการ Array (The Power of Iteration)
ใน C# คุณคุ้นเคยกับ `for (int i=0; i<n; i++)` ใช่ไหมครับ? ใน CODESYS ST ก็ทำงานเหมือนกัน แต่ Syntax จะออกไปทางภาษา Pascal ครับ

### จุดที่ต้องระวังสำหรับ C# Dev:
* **1-Based Indexing:** แม้เราจะประกาศ Array เริ่มที่ 0 ได้ แต่ในวงการ PLC นิยมประกาศเริ่มที่ 1 (เช่น `1..100`) เพื่อให้ตรงกับ Tag name หรือเอกสารทางไฟฟ้า
* **Boundaries:** CODESYS เคร่งครัดเรื่อง Array Bounds มาก ถ้าหลุด Range อาจเกิด Exception จน Controller หยุดทำงานได้ (ในโปรเจกต์จริงควรใช้ฟังก์ชัน `CheckBounds` ช่วย)

### ตัวอย่าง: การ Reset ค่าการผลิต 100 รายการ (Array Initialization)

**ส่วนประกาศตัวแปร (VAR):**
```pascal
VAR
    // ประกาศ Array ขนาด 100 ช่อง (Index 1 ถึง 100) เก็บค่าจำนวนเต็ม
    aProductionData : ARRAY [1..100] OF INT;

    // ตัวแปรสำหรับวนลูป (Iterator)
    iIndex : INT;

    // ตัวแปร Trigger เพื่อสั่ง Reset
    xResetCmd : BOOL;
END_VAR

```

**ส่วน Logic (ST):**

```pascal
// ถ้ามีคำสั่ง Reset ให้วนลูปเคลียร์ค่าเป็น 0
IF xResetCmd THEN

    // Syntax: FOR <Counter> := <Start> TO <End> BY <Step> DO
    // (BY 1 เป็น Default ไม่ต้องใส่ก็ได้)
    FOR iIndex := 1 TO 100 BY 1 DO
        aProductionData[iIndex] := 0;
    END_FOR;

    // Reset คำสั่งเมื่อทำเสร็จ (Auto-reset flag)
    xResetCmd := FALSE;

END_IF;

```

## 2. ความแตกต่างระหว่าง WHILE และ REPEAT Loops

สองตัวนี้ใช้เมื่อ **"ไม่รู้จำนวนรอบที่แน่นอน"** แต่รู้ **"เงื่อนไขที่จะหยุด"** ครับ

### A. WHILE Loop (Check First)

ตรวจสอบเงื่อนไข **ก่อน** ทำงาน ถ้าเงื่อนไขเป็นเท็จตั้งแต่แรก จะไม่ทำคำสั่งภายในเลย (เหมือน `while` ใน C#)

> **Use Case:** ใช้เมื่อต้องตรวจสอบความพร้อมก่อนเริ่ม Process

```pascal
// ตัวอย่าง: ลดค่าลงเรื่อยๆ จนกว่าจะหมด (ถ้าค่าเริ่มเป็น 0 ก็จะไม่ทำเลย)
WHILE iCounter > 0 DO
    iCounter := iCounter - 1;
END_WHILE;

```

### B. REPEAT Loop (Do First)

ทำคำสั่งภายในก่อน 1 รอบ แล้วค่อยตรวจสอบเงื่อนไข **ทีหลัง** (คล้าย `do...while` ใน C#)

> **Use Case:** ต้องการให้ Code รันอย่างน้อย 1 ครั้งเสมอ เช่น การอ่านค่าจาก Buffer แล้วค่อยเช็คว่าหมดหรือยัง

```pascal
// ตัวอย่าง: เพิ่มค่าอย่างน้อย 1 ครั้ง แล้วเช็คว่าถึงเป้าหมายหรือยัง
REPEAT
    iValue := iValue * 2;
UNTIL iValue >= 100 
// ⚠️ ระวัง! UNTIL คือ "หยุดเมื่อเงื่อนไขเป็นจริง" 
// (ต่างจาก C# ที่ while คือ "ทำต่อเมื่อเงื่อนไขเป็นจริง")
END_REPEAT;

```

## 3. ⚠️ ARCHITECT WARNING: The "Infinite Loop" & Watchdog Timer

ในฐานะ C# Developer คุณอาจเคยเขียน `while(true)` เพื่อรอรับ Event ใน Thread แยกใช่ไหมครับ?
**ห้ามทำแบบนั้นใน PLC เด็ดขาด!** นี่คือกับดักที่อันตรายที่สุดสำหรับผู้ย้ายมาจากสาย PC ครับ

### ทำไมถึงอันตราย? (The Watchdog Concept)

PLC ทำงานเป็น **Cyclic Scan**:

1. Read Inputs
2. **Execute Code (Logic)** <--- Code ของคุณอยู่ตรงนี้
3. Write Outputs

ระบบปฏิบัติการของ PLC (Runtime) จะมี **Watchdog Timer** คอยจับเวลา (เช่น ตั้งไว้ 100ms)

* ถ้า Code ของคุณวน Loop นานเกิน 100ms (เพราะ Infinite Loop หรือคำนวณหนักเกินไป) PLC จะเข้าใจว่า **"ระบบค้าง"**
* **ผลลัพธ์:** PLC จะสั่ง **STOP ทันที (Exception Mode)** เครื่องจักรหยุดทำงาน Outputs ทั้งหมดดับลง ซึ่งอาจเกิดอุบัติเหตุหรือความเสียหายได้

### สิ่งที่ต้องระวัง:

1. **Loop ซ้อน Loop (Nested Loops):** การวน Loop 100 x 100 x 100 รอบ อาจกินเวลาเกิน Scan Time ได้ง่ายๆ
2. **Wait Logic:** ห้ามใช้ WHILE เพื่อรอ Sensor (เช่น `WHILE Sensor = FALSE DO ; END_WHILE`) เพราะมันจะบล็อก Scan Cycle ทำให้ PLC ตาย **ให้ใช้ IF เช็คในแต่ละรอบสแกนแทน**
3. **Infinite Condition:** การเขียนเงื่อนไขที่ไม่มีวันเป็นเท็จใน WHILE หรือ REPEAT จะทำให้ Watchdog ตัดทันที

> **Best Practice:**
> หากต้องประมวลผล Array ขนาดใหญ่มาก (เช่น 10,000 ตัว) **อย่าทำในรอบเดียว (One Cycle)** ให้แบ่งทำทีละส่วน (Slicing) ในแต่ละรอบสแกน เพื่อรักษา Cycle Time ให้ต่ำและระบบตอบสนองได้ไวครับ

---

**บทสรุป**
Loop คือเครื่องมือที่ทรงพลัง แต่ต้องใช้ด้วยความระมัดระวังสูงสุดในโลก Automation ในบทถัดไป **EP.4** เราจะไปดูเรื่อง **Standard Function Blocks (Timers & Triggers)** เครื่องมือที่จะช่วยให้คุณจัดการ "เวลา" และ "เหตุการณ์" ได้อย่างแม่นยำครับ
