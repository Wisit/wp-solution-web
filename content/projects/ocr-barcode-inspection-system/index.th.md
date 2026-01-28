---
title: "ระบบตรวจสอบความถูกต้องฉลาก (OCR & Barcode) บนเครื่องจักร Reel-to-Reel"
slug: ocr-barcode-inspection-system
date: 2026-01-28
author: ทีมงาน WP Solution
categories: [Case Study]
tags: [Machine Vision, LabVIEW, QC System, Smart Factory, Basler]
description: "ระบบ Vision Inspection ตรวจสอบคุณภาพการพิมพ์และ Cross-check ข้อมูล Barcode เทียบกับตัวเลข (OCR) แบบ Real-time บนสายพานความเร็วสูง"
cover:
    image: "ocr-barcode-inspection-system-cover.jpg"
    alt: "หน้าจอระบบตรวจสอบ Barcode และ OCR"
    relative: true
---

# ระบบตรวจสอบคุณภาพฉลากและบาร์โค้ด (OCR & Barcode Inspection)

**โจทย์ที่ได้รับ:**
โรงงานผลิตฉลากสินค้า (Label Converter) ต้องการแก้ปัญหา "ฉลากหลุด QC" ซึ่งเกิดจากการพิมพ์ข้อมูลผิดพลาด เช่น รหัสบาร์โค้ดไม่ตรงกับตัวเลขที่พิมพ์ (Data Mismatch) หรือหมึกพิมพ์จางจนอ่านไม่ได้ การตรวจสอบด้วยตาเปล่าบนเครื่องม้วน (Reel-to-Reel) นั้นทำได้ยากและมีความผิดพลาดสูง

**ความท้าทาย:**
ระบบต้องทำงานสัมพันธ์กับเครื่องจักรเดิมที่มีความเร็วสูง ต้องอ่านค่าทั้ง **OCR (ตัวเลข)** และ **Barcode** พร้อมกันในเสี้ยววินาที และต้องสั่งหยุดเครื่องทันทีที่เจอของเสีย (NG) เพื่อไม่ให้สินค้าที่ผิดพลาดหลุดไปถึงมือลูกค้า

## แนวทางการแก้ปัญหาของเรา

WP Solution พัฒนาระบบ **Machine Vision** โดยใช้ **LabVIEW** เป็นแกนหลัก เชื่อมต่อกับกล้องอุตสาหกรรม **Basler** เพื่อจับภาพฉลากแต่ละดวงขณะเคลื่อนที่

จุดเด่นคือฟีเจอร์ **Dual Validation** ที่ทำการ "Cross-check" ข้อมูล คืออ่าน Barcode แล้วนำไปเทียบกับตัวเลข OCR ที่อ่านได้ ถ้าไม่ตรงกัน ระบบจะตัดสินเป็น NG และสั่ง I/O Board ให้หยุดมอเตอร์ทันที

![แผนผังระบบ](ocr-barcode-inspection-system-diagram.jpg)

### เทคโนโลยีที่ใช้ (Tech Stack)

* **LabVIEW Vision Module:** ใช้ในการเขียน Algorithm ทำ OCR และ Barcode Reading ที่มีความแม่นยำสูง
* **Basler Camera (GigE):** กล้องความเร็วสูงรุ่น acA1920-25gm ที่รองรับ Trigger Mode เพื่อภาพที่คมชัด
* **Backlight Illumination:** ใช้เทคนิคไฟส่องจากด้านล่างสายพาน เพื่อดูความทึบแสงและเงาของฉลาก ทำให้ Trigger ตำแหน่งภาพได้แม่นยำแม้เปลี่ยนอาร์ตเวิร์ก
* **Program Recipe:** ระบบรองรับการ Save/Load การตั้งค่าผ่านไฟล์ `.ini` ทำให้เปลี่ยน Product ได้รวดเร็วโดยไม่ต้อง Calibrate ใหม่ทุกครั้ง

## ผลลัพธ์ที่ได้ (Business Impact)

* ✅ **Zero Defect:** ป้องกันความผิดพลาดเรื่องข้อมูล (Mixing/Missing Data) ได้ 100% ก่อนส่งมอบงาน
* ✅ **Real-time Monitoring:** โอเปอเรเตอร์เห็นภาพสดและค่า Process Time (ms) พร้อมจำนวน OK/NG ทันทีหน้าเครื่อง
* ✅ **Production Control:** ควบคุมยอดการผลิตได้แม่นยำตาม Order ระบบหยุดเองเมื่อครบจำนวน

> **เกร็ดความรู้จากหน้างาน:**
> การตั้งค่า **Camera Calibration** ในไฟล์ `.icd` (Exposure Time, Gain) เป็นหัวใจสำคัญของความคมชัด ห้ามแก้ไขไฟล์นี้โดยตรง ควรปรับผ่านโปรแกรม NI MAX เท่านั้นเพื่อความเสถียรของระบบ

---
**ต้องการที่ปรึกษาระบบ Vision Inspection?**
ติดต่อเรา: wisit.paewkratok@gmail.com | Line: wisit.p