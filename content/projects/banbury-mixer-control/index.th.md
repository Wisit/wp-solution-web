---
title: "ระบบควบคุม Banbury Mixer 75L: ยกระดับเครื่องผสมยางด้วย Hybrid PC-Based Control"
slug: banbury-mixer-control
date: 2026-01-27
author: ทีมงาน WP Solution
categories: [Case Study]
tags: [C#, PLC Mitsubishi, RS-485, Batch Process, Smart Factory]
description: "เปลี่ยนเครื่องจักรระบบเก่าให้เป็น Smart Machine ด้วยสถาปัตยกรรม Hybrid PC+PLC เชื่อมต่ออุปกรณ์ต่างยี่ห้อและเก็บ Data เพื่อการตรวจสอบย้อนกลับสมบูรณ์แบบ"
cover:
    image: "banbury-mixer-control-cover.jpg"
    alt: "หน้าจอควบคุมเครื่อง Banbury Mixer"
    relative: true
---

# Digitalization of Batch Process: กรณีศึกษาเครื่องผสมยาง

**โจทย์ที่ได้รับ:**
ลูกค้าต้องการปรับปรุงกระบวนการผสมยาง (Rubber Mixing) ของเครื่อง **Banbury Mixer ขนาด 75 ลิตร** ปัญหาเดิมคือระบบควบคุมไม่ยืดหยุ่น การจัดการสูตรการผลิต (Recipe) ทำได้ยาก และเมื่อเกิดปัญหาสินค้าไม่ได้คุณภาพ (Defect) ไม่สามารถตรวจสอบย้อนกลับ (Traceability) ได้ว่าเกิดจากอุณหภูมิผิดพลาดหรือสัดส่วนผสมที่ไม่ถูกต้อง

**ความท้าทาย:**
ความยากของโปรเจกต์นี้คือ **"ความหลากหลายของอุปกรณ์" (Interoperability)** หน้างานประกอบด้วยฮาร์ดแวร์ต่างยี่ห้อและต่างยุคสมัย:
* เครื่องชั่ง Ishida (RS-232)
* ตัวควบคุมอุณหภูมิ Delta (Modbus RS-485)
* มิเตอร์วัดกระแสไฟฟ้า และ PLC Mitsubishi (Ethernet)
การทำให้อุปกรณ์ทั้งหมดนี้ "คุยกันรู้เรื่อง" ในหน้าจอเดียว คือหัวใจสำคัญ

## แนวทางการแก้ปัญหา: Hybrid PC-Based Supervisory Control

เราเลือกใช้สถาปัตยกรรมแบบลูกผสม เพื่อดึงจุดเด่นของทั้ง PC และ PLC ออกมาใช้:

1.  **PC as a Brain (สมองสั่งการ):** พัฒนาซอฟต์แวร์ด้วย **C# (WPF)** เพื่อจัดการ Logic ขั้นสูง เช่น การเทียบสูตรผสม (Recipe Management) ที่ซับซ้อน และเก็บข้อมูล (Data Logging) ปริมาณมหาศาลลง Database
2.  **PLC as Muscle (กล้ามเนื้อ):** ใช้ **Mitsubishi FX3U** รับคำสั่งจาก PC ไปขับเคลื่อนมอเตอร์และอ่านค่า Sensor หน้างาน เพื่อความเสถียรและปลอดภัยสูงสุด

![แผนผังระบบ Hybrid PC-PLC](banbury-mixer-control-diagram.jpg)

### เทคโนโลยีที่ใช้ (Tech Stack)
* **Custom C# Application:** ซอฟต์แวร์ควบคุมหลัก มีความยืดหยุ่นสูงกว่า HMI สำเร็จรูปทั่วไป
* **Universal Gateway Logic:** เขียน Driver เชื่อมต่อทั้ง RS-232, RS-485 และ Ethernet ให้ทำงานพร้อมกัน
* **Batch Process Control:** ระบบควบคุม Step การทำงานแบบอัตโนมัติ โดยใช้เงื่อนไขของ เวลา (Time) และ อุณหภูมิ (Temp) เป็นตัวตัดจบขั้นตอน

## ผลลัพธ์ที่ได้ (Business Impact)
* ✅ **Quality Assurance (QA):** มีระบบ Traceability เก็บกราฟอุณหภูมิและกระแสไฟฟ้าย้อนหลัง ใช้เป็นหลักฐานวิเคราะห์ปัญหาเมื่อมีการเคลมสินค้า
* ✅ **Cost Efficiency:** ไม่ต้องทิ้งอุปกรณ์เก่า (Legacy Devices) เพราะซอฟต์แวร์ของเราสามารถเชื่อมต่อเข้ากับระบบใหม่ได้ทันที
* ✅ **Remote Monitoring:** แบ่งโปรแกรมเป็นส่วน Control (หน้าเครื่อง) และ Monitor (ห้องผู้จัดการ) ทำให้ฝ่ายบริหารตรวจสอบสถานะการผลิตได้แบบ Real-time โดยไม่ต้องลงไปหน้างาน

> **เกร็ดความรู้จากหน้างาน:**
> สำหรับงานที่มีสูตรการผลิตซับซ้อนและต้องการเก็บ Data ละเอียด การใช้ PLC เพียงอย่างเดียวมักติดข้อจำกัดเรื่อง Memory การขยับมาใช้ **PC-Based Control** ร่วมกับ PLC จะช่วยปลดล็อกขีดจำกัดนี้ ทำให้เครื่องจักรเก่ากลายเป็น Smart Machine ได้จริง

---
**ต้องการที่ปรึกษาระบบ Automation สำหรับเครื่องจักรเก่า?**
ติดต่อเรา: wisit.paewkratok@gmail.com | Line: wisit.p
