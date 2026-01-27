---
title: "ระบบบันทึกค่า Linear Scale และเทคนิคจัดการไฟล์ Excel ขนาดใหญ่ (CPK/Stats)"
slug: linear-scale-data-logger
date: 2026-01-27
author: ทีมงาน WP Solution
categories: [Case Study]
tags: [C#, RS-232, SQLite, Excel Automation, Smart Factory]
description: "เปลี่ยนเครื่องมือวัดธรรมดาให้เป็น IoT และเทคนิคพิเศษในการเขียนข้อมูลลงไฟล์ Excel Template ขนาดใหญ่ที่มีสูตรซับซ้อน โดยไม่ทำให้ไฟล์พัง"
cover:
    image: "linear-scale-cover.jpg"
    alt: "Linear Scale Data Logger System"
    relative: true
---

# เนื้อหาบทความ

**โจทย์ที่ได้รับ:**
ลูกค้าต้องการเปลี่ยนกระบวนการ QC จากการ "อ่านค่าด้วยตาแล้วจดลงกระดาษ" มาเป็นระบบ Digital (Digitization of Metrology) เพื่อลดความผิดพลาดจากคน (Human Error) และต้องการให้ออก Report เป็น Excel ที่มีสูตรคำนวณสถิติ (CPK, Yield) ที่ซับซ้อนได้ทันที

**ความท้าทาย (The Real Bottleneck):**
ความยากไม่ใช่แค่การอ่านค่าจาก Linear Scale แต่คือ **"ไฟล์ Template Excel ของลูกค้ามีขนาดใหญ่มาก"** ภายในเต็มไปด้วยสูตรคำนวณทางสถิติและกราฟที่ผูกกันหลายชีท
* การใช้ Library ทั่วไปเขียนข้อมูลเข้าไปตรงๆ ทำให้โปรแกรมค้าง (Crash) หรือทำงานช้ามาก
* บ่อยครั้งที่เขียนเสร็จ ไฟล์ปลายทางเสียหาย (Corrupted) เพราะโครงสร้างสูตรคำนวณพัง

## แนวทางการแก้ปัญหาของเรา

เราพัฒนาระบบ **Linear Scale Data Logger** ที่ทำงานแบบครบวงจร:

1.  **Smart Connectivity:** เชื่อมต่อ Linear Scale ผ่านพอร์ต **RS-232** ดึงค่าเข้าคอมพิวเตอร์โดยตรง ไม่ต้องพิมพ์เอง
2.  **Active QC:** ระบบมี Spec/Tolerance ในตัว พนักงานวัดปุ๊บ รู้ผล **Pass/Fail** ทันทีด้วย Visual Indicator (สีเขียว/แดง)
3.  **Advanced Excel Manipulation (เทคนิคพิเศษ):** แก้ปัญหาไฟล์ Excel บวมด้วยการจัดการระดับโครงสร้างไฟล์:
    * **Unpack:** โปรแกรมจะแตกไฟล์ `.xlsx` (ซึ่งจริงๆ คือ Zip) ออกมาเป็นไฟล์ XML ย่อยๆ
    * **Inject:** เข้าไปแก้ไขข้อมูลในไฟล์ XML เนื้อหาโดยตรง (Direct XML Manipulation) โดยไม่แตะต้องส่วนที่เป็นสูตรคำนวณ
    * **Repack:** รวมไฟล์กลับเป็น `.xlsx` ที่สมบูรณ์
    * **Result:** สามารถสร้าง Report จาก Template ขนาดใหญ่ได้รวดเร็ว โดยที่สูตร CPK ยังทำงานได้ถูกต้อง 100%

![แผนผังระบบ](linear-scale-diagram.jpg)

### เทคโนโลยีที่ใช้ (Tech Stack)
* **C# (.NET Windows Forms):** พัฒนาโปรแกรมหลักที่เสถียรและตอบสนองไว
* **Serial Port (RS-232):** เชื่อมต่อ Hardware เครื่องมือวัด
* **SQLite:** ฐานข้อมูล Local สำหรับเก็บประวัติการวัด (Traceability) และป้องกันข้อมูลหายเมื่อไฟดับ
* **Custom XML/Zip Algorithm:** หัวใจสำคัญในการจัดการไฟล์ Excel ขนาดใหญ่

## ผลลัพธ์ที่ได้ (Business Impact)
* ✅ **Eliminate Human Error:** ขจัดความผิดพลาดจากการจดค่าและการคีย์ข้อมูล 100%
* ✅ **Performance:** ลดเวลาการทำ Report จบกะ จากเดิมใช้เวลาเป็นชั่วโมง เหลือเพียงไม่กี่วินาที
* ✅ **Data Integrity:** ข้อมูลถูกต้อง เชื่อถือได้ และสอบย้อนกลับได้ (Traceability)

> **เกร็ดความรู้จากหน้างาน:**
> การจัดการไฟล์ Excel ในงานอุตสาหกรรมที่มีสูตรเยอะๆ ห้ามใช้การเปิด Application Excel (Interop) ขึ้นมาเขียนเด็ดขาด เพราะจะทำให้เครื่องค้าง วิธีที่ดีที่สุดคือการแก้ไขที่ไส้ในของไฟล์ (XML Structure) ซึ่งเร็วกว่าและปลอดภัยกว่ามาก

---
**ต้องการที่ปรึกษาระบบ Automation และ Data Logging?**
ติดต่อเรา: wisit.paewkratok@gmail.com | Line: wisit.p