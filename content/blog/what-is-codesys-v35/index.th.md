---
title: "เจาะลึก CODESYS V3.5 คืออะไร? ทำไมถึงเป็นมาตรฐานใหม่ของวงการ Automation"
slug: what-is-codesys-v35
date: 2026-01-28
author: Wisit P. (Senior Architect)
categories: [Technical Guide]
tags: [CODESYS, IEC 61131-3, IIoT, Automation, SoftPLC]
description: "รู้จักกับ CODESYS V3.5 แพลตฟอร์มที่เปลี่ยนโลก Automation ด้วยมาตรฐาน IEC 61131-3 และฟีเจอร์ที่ครบจบในตัวเดียว ตั้งแต่เขียน Logic, HMI ไปจนถึง Motion Control"
cover:
    image: "codesys-v35-cover.jpg"
    alt: "CODESYS V3.5 Ecosystem"
    relative: true
# --- Theme Config ---
ShowToc: true
TocOpen: true
ShowReadingTime: true
---

**CODESYS V3.5** (ย่อมาจาก **Co**ntroller **De**velopment **Sys**tem) คือแพลตฟอร์มซอฟต์แวร์สำหรับงาน Industrial Automation ที่พัฒนาโดยบริษัท **3S-Smart Software Solutions GmbH** จากประเทศเยอรมนี ซึ่งได้รับการออกแบบมาเพื่อแก้ปัญหาความยุ่งยากในการเขียนโปรแกรม PLC หลากหลายยี่ห้อ โดยยึดตามมาตรฐานสากล **IEC 61131-3** อย่างเคร่งครัด

เพื่อให้เข้าใจภาพรวมเชิงลึก เราสามารถแบ่งองค์ประกอบและจุดเด่นออกเป็นหัวข้อสำคัญดังนี้:

## 1. สถาปัตยกรรมระบบ (System Architecture)
CODESYS แบ่งการทำงานออกเป็น 2 ส่วนหลักที่ทำงานร่วมกัน:
* **CODESYS Development System (IDE):** เป็นซอฟต์แวร์สำหรับเขียนโปรแกรมที่รันบน PC ซึ่งแจกจ่ายให้ใช้งานได้ฟรี มีเครื่องมือครบครันทั้ง Debugger, Library Management และ Visual Editors
* **CODESYS Runtime System (RTS):** เปรียบเสมือน "หัวใจ" ที่เปลี่ยนฮาร์ดแวร์ทั่วไป (เช่น Industrial PC, Embedded Board หรือแม้แต่ Raspberry Pi) ให้กลายเป็น PLC ที่ทรงพลัง ผู้ผลิตฮาร์ดแวร์กว่า 500 แบรนด์ทั่วโลกได้นำ Runtime นี้ไปติดตั้งในอุปกรณ์ของตน ทำให้โค้ดที่เขียนบน CODESYS สามารถนำไปรันบนฮาร์ดแวร์เหล่านี้ได้ทันที (Hardware-independent)

## 2. ภาษาที่รองรับ (Programming Languages & OOP)
CODESYS V3.5 รองรับภาษามาตรฐาน **IEC 61131-3** ครบทั้ง 5 ภาษา และยังมีภาษาเพิ่มเติมที่เป็นส่วนขยายด้วย:
* **Text-based:** Structured Text (ST) และ Instruction List (IL)
* **Graphical:** Ladder Diagram (LD), Function Block Diagram (FBD) และ Sequential Function Chart (SFC)
* **Extension:** Continuous Function Chart (CFC) ซึ่งเป็นรูปแบบ FBD ที่ยืดหยุ่นกว่ามาตรฐานกำหนด
* **Object-Oriented Programming (OOP):** จุดเด่นสำคัญของ V3.5 คือการรองรับ OOP เต็มรูปแบบ (Class, Interface, Method, Inheritance) ช่วยให้การเขียนโปรแกรมมีความยืดหยุ่นและนำกลับมาใช้ใหม่ได้ง่ายขึ้น (Reusable)

## 3. ฟีเจอร์ที่รวมมาในตัว (All-in-One Solution)
CODESYS V3.5 ไม่ได้เป็นแค่ตัวเขียน Logic แต่รวมเครื่องมืออื่นๆ ไว้ในหน้าต่างเดียว:
* **Visualization:** สามารถสร้างหน้าจอ HMI/SCADA ได้ในตัวซอฟต์แวร์เลย โดยรองรับทั้ง **TargetVisu** (แสดงผลบนจอของเครื่องจักร) และ **WebVisu** (แสดงผลผ่าน Web Browser)
* **Fieldbus Support:** รองรับการสื่อสารกับอุปกรณ์ภายนอกผ่าน Protocol มาตรฐานมากมาย เช่น EtherCAT, PROFINET, Modbus TCP/RTU, CANopen และ IO-Link โดยไม่ต้องใช้ซอฟต์แวร์แยก
* **Motion Control & CNC:** มีโมดูล **SoftMotion** สำหรับควบคุมการเคลื่อนที่ของ Servo Drive, การทำ Camming และควบคุมหุ่นยนต์
* **IIoT Connectivity:** รองรับ **OPC UA Server/Client** ในตัว ทำให้สามารถแลกเปลี่ยนข้อมูลกับระบบ IT หรือ Cloud ได้อย่างสะดวก

## 4. การจัดการและติดตั้ง (Management & Installation)
* **CODESYS Installer:** ในเวอร์ชันใหม่ๆ (เช่น V3.5 SP17 ขึ้นไป) จะมีเครื่องมือนี้สำหรับจัดการ Package และ Add-on ต่างๆ แยกต่างหาก ช่วยให้จัดการเวอร์ชันได้ง่ายขึ้น
* **Device Repository:** ใช้สำหรับจัดการไฟล์ Description ของอุปกรณ์ (เช่น XML, EDS) เพื่อให้ซอฟต์แวร์รู้จักฮาร์ดแวร์ใหม่ๆ ที่เราจะนำมาใช้งาน
* **Library Management:** มีระบบจัดการ Library ที่เข้มแข็ง สามารถสร้างและเรียกใช้ Function Block มาตรฐาน หรือ Library ที่ผู้ใช้งานสร้างขึ้นเองได้

## 5. ข้อสังเกตเพิ่มเติม (Migration & Compatibility)
* **V2.3 vs V3.5:** CODESYS V3.5 เป็นรุ่นสืบทอดจาก V2.3 (ซึ่งหยุดพัฒนาไปแล้วในปี 2019) โดยมีการปรับปรุงสถาปัตยกรรมใหม่หมด โปรเจกต์เก่าสามารถนำมาแปลง (Convert) เพื่อใช้บน V3.5 ได้ แต่อาจต้องมีการปรับแก้บางส่วน
* **Hardware Independence:** เนื่องจากแนวคิดคือการไม่ยึดติดกับฮาร์ดแวร์ ทำให้ผู้ผลิตเครื่องจักรสามารถเปลี่ยนยี่ห้อ PLC ได้โดยไม่ต้องเขียนโปรแกรมใหม่ทั้งหมด ช่วยลดต้นทุนและเพิ่มอำนาจการต่อรอง

> **บทสรุป**
> **CODESYS V3.5** คือ Ecosystem ที่สมบูรณ์สำหรับนักพัฒนา Automation ยุคใหม่ ที่ต้องการเครื่องมือที่ทรงพลัง ยืดหยุ่น และไม่ผูกขาดกับยี่ห้อใดนั่นเองครับ

---
**อ่านต่อบทความถัดไป:** [คู่มือเขียน PLC ด้วย Structured Text (ST) Ep.1: ปูพื้นฐาน Syntax และ Data Types](/blog/st-programming-ep1-basics) 