---
title: "ระบบดึงข้อมูลเครื่องทดสอบเกียร์ (Legacy Machine) โดยไม่แก้โปรแกรมเดิม"
slug: gearset-matching-data-collection
date: 2026-01-27
author: ทีมงาน WP Solution
categories: [Case Study]
tags: [Legacy Machine, C#, MSSQL, Smart Factory, Automotive]
description: "โซลูชันเชื่อมต่อเครื่อง Oerlikon T60 เพื่อเก็บค่า Matching Gearset ลงฐานข้อมูลอัตโนมัติ ด้วยเทคนิค File Parsing และ DAQ Trigger"
cover:
    image: "gearset-cover.jpg"
    alt: "ระบบเก็บข้อมูลเครื่อง Oerlikon T60"
    relative: true
---

# ระบบเก็บข้อมูลการจับคู่เฟืองท้าย (Gearset Matching)

**โจทย์ที่ได้รับ:**
ลูกค้ารายใหญ่ผู้ผลิตชิ้นส่วนยานยนต์ในนิคมฯ ปลวกแดง ระยอง ต้องการเก็บค่าผลการทดสอบชิ้นส่วน "เฟืองท้าย" (Ring & Pinion) จากเครื่องทดสอบ **Oerlikon T60** เข้าสู่ระบบฐานข้อมูลกลาง (Server) เพื่อใช้ในการตรวจสอบย้อนกลับ (Traceability)

**ความท้าทาย (Pain Point):**
* **เครื่องจักรระบบปิด:** เครื่อง Oerlikon เป็นเครื่องจักรเฉพาะทางที่มีมูลค่าสูง การเข้าไปแก้ไข Program หรือ Logic ภายในเครื่องเป็นเรื่องต้องห้าม (เสี่ยงเครื่องรวนและหมดประกัน)
* **ข้อมูลเป็น Text File:** เครื่องทำได้เพียง Gen ไฟล์ผลลัพธ์ (`*.prt`) เก็บไว้ในเครื่องตัวเอง ไม่มีการส่งข้อมูลออกข้างนอก ทำให้ฝ่าย QC ตรวจสอบข้อมูลย้อนหลังได้ยาก

## แนวทางการแก้ปัญหาของเรา: เทคนิค "Sidecar Integration"

ทีมงาน WP Solution เลือกใช้วิธีติดตั้งระบบประกบ (External System) โดยทำหน้าที่เป็นเพียง "ผู้สังเกตการณ์" และดึงข้อมูลออกมาเมื่อเครื่องทำงานเสร็จ โดย **ไม่ยุ่งเกี่ยวกับ Source Code เดิมของเครื่องจักร**

![แผนผังการทำงานของระบบ](gearset-workflow.jpg)

### ขั้นตอนการทำงาน (System Workflow)

1.  **Scan & Check:** พนักงานสแกน Barcode ของชิ้นงาน (Ring/Pinion) ระบบจะวิ่งไปเช็คกับ **MSSQL Server** ว่าชิ้นงานนี้ผ่านกระบวนการก่อนหน้ามาถูกต้องหรือไม่
2.  **Machine Testing:** ปล่อยให้เครื่อง Oerlikon ทำงานทดสอบตามปกติ
3.  **Signal Trigger (จุดสำคัญ):**
    * เราติดตั้งคอมพิวเตอร์เสริม 1 ชุด พร้อม **การ์ด DAQ (Data Acquisition)**
    * ทำการ Wiring สัญญาณ "Test Complete" จากเครื่องจักร เข้ามาที่ DAQ เพื่อบอกโปรแกรมของเราว่า "ทดสอบเสร็จแล้วนะ เตรียมอ่านข้อมูลได้"
4.  **File Parsing:** เมื่อได้รับสัญญาณ โปรแกรม C# ของเราจะทำการ:
    * วิ่งผ่าน Network (Map Drive) เข้าไปที่ Path เก็บไฟล์ของเครื่อง (`\\xxx\TEST_xxxx\Protocols\*.prt`)
    * ค้นหาไฟล์ล่าสุด และอ่านข้อมูล Text ภายใน
    * ถอดรหัส (Parse) ค่าที่ต้องการ เช่น ค่า Backlash หรือ Pattern การขบกันของเฟือง
5.  **Save to DB:** บันทึกค่าที่อ่านได้ คู่กับ Serial Number ลงฐานข้อมูล MSSQL ทันที

### เทคโนโลยีที่ใช้ (Tech Stack)

* **Software:** C# (.NET WPF) สำหรับทำหน้าจอและ Logic การอ่านไฟล์
* **Hardware:** Industrial PC + DAQ Card สำหรับรับสัญญาณ Trigger
* **Database:** Microsoft SQL Server
* **Integration:** Network File Sharing (SMB) & Log File Parsing

## ผลลัพธ์ที่ได้ (Business Impact)

* ✅ **Real-time Traceability:** ข้อมูลการผลิตถูกส่งขึ้น Server ทันทีที่ผลิตเสร็จ ผู้บริหารดู Dashboard ได้ Real-time
* ✅ **Risk Free:** ไม่มีความเสี่ยงต่อเครื่องจักรเดิม เพราะเราใช้วิธี "อ่าน" ไฟล์ และ "รับ" สัญญาณเท่านั้น ไม่มีการส่งคำสั่งไปกวนการทำงานของเครื่อง
* ✅ **Data Integrity:** ลดความผิดพลาดจากการจดบันทึกด้วยมือ (Manual Recording) ได้ 100%

> **เกร็ดความรู้จากหน้างาน:**
> การทำ Automation กับเครื่องจักรเก่า (Retrofit) ไม่จำเป็นต้องรื้อระบบ PLC เสมอไป การใช้วิธีดักจับสัญญาณ (IO Monitoring) ผสมกับการอ่าน Log File เป็นวิธีที่ "Play Safe" ที่สุด และประหยัดงบประมาณกว่าการเปลี่ยน Controller ใหม่ทั้งระบบ

---
**ต้องการเชื่อมต่อข้อมูลจากเครื่องจักรเก่า?**
ติดต่อเรา: wisit.paewkratok@gmail.com | Line: wisit.p