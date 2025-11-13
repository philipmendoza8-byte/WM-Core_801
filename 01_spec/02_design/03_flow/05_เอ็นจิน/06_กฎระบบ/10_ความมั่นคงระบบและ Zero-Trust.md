# 10_ความมั่นคงระบบและ Zero-Trust — WM-Core_801
System Integrity & Zero-Trust Layer  
โปรโตคอลรับรองความถูกต้องของระบบ + ความปลอดภัยแบบไม่เชื่อใจใคร (Zero-Trust Model)

---

## 1) วัตถุประสงค์ (Objectives)
Zero-Trust ใน WM-Core/801 ถูกใช้เพื่อ:
- ควบคุมไม่ให้ข้อมูลถูกแก้ไขระหว่างทาง  
- ป้องกันการบุกรุก / แทรกแซงจากภายนอก  
- ตรวจสอบตัวตนทุกครั้งแม้ในระบบภายใน  
- ยืนยันความบริสุทธิ์ของข้อมูลทุกชิ้นก่อนนำไปใช้  
- ทำให้ pipeline 801 มีมาตรฐานระดับองค์กร/รัฐ

---

## 2) หลักการ Zero-Trust ของ 801 (Core Principles)
WM-Core/801 ยึดหลัก 6 ข้อ:

1) **ไม่เชื่อใจใครก่อนตรวจสอบ**  
2) **ยืนยันสิทธิ์ทุกการเข้าถึง (continuous authentication)**  
3) **สิทธิ์แบบน้อยที่สุด (least privilege)**  
4) **แบ่งโซนข้อมูล (micro-segmentation)**  
5) **ตรวจทุกพฤติกรรมผิดปกติ (behavioral anomaly)**  
6) **ข้อมูลต้องมีที่มา-เส้นทาง-แฮชยืนยัน (origin–lineage–integrity)**

---

## 3) องค์ประกอบ Zero-Trust ของ WM-Core/801

### **ZT-1: Authentication Layer (ตัวตน)**  
- ทุก session ต้อง verify token  
- token อายุสั้น (short-lived token)  
- ถ้าพบ request แปลก → revoke token ทันที  
- ผู้ใช้ถูกจำกัดตามบทบาท: Custodian / Operator / AI-Internal

---

### **ZT-2: Authorization Layer (สิทธิ์เข้าถึง)**  
สิทธิ์ต้องกำหนดแบบ “Minimal Scope”:
- แก้ไขได้เฉพาะไฟล์ที่ user มีสิทธิ์  
- มองเห็นเฉพาะ log / pipeline ที่เกี่ยวข้อง  
- ไม่อนุญาตการคัดลอก/ดาวน์โหลดไฟล์ระบบโดยไม่มีเหตุผล

---

### **ZT-3: Data Integrity Layer (ความบริสุทธิ์ของข้อมูล)**  
ข้อมูลทุกชิ้นต้องมี:
- SHA-256 hash  
- trace_id  
- origin metadata  
- consent flag (ถ้ามีข้อมูลบุคคล)  
- lineage map  

ก่อนใช้ทุกครั้งต้องตรวจ hash:
- ถ้าไม่ตรง → ถือว่า **tampered**  
- ส่งเข้า **quarantine zone** ทันที

---

### **ZT-4: Micro-Segmentation (แบ่งโซนข้อมูล)**  
ระบบต้องแบ่งข้อมูลเป็นโซน:
- public zone  
- internal zone  
- sensitive zone  
- quarantine zone  

การเคลื่อนย้ายข้อมูลระหว่างโซน ต้องผ่าน:
- 1) PDPA Filter  
- 2) Hash Check  
- 3) Permission Check

---

### **ZT-5: Behavior Monitoring (จับพฤติกรรมผิดปกติ)**  
ระบบต้องตรวจจับพฤติกรรม เช่น:
- ส่งข้อมูลซ้ำหลายครั้งผิดปกติ  
- request แปลกเวลา  
- input ขนาดใหญ่ผิดจากปกติ  
- workflow ที่ไม่เคยใช้  
- ความพยายามเข้าถึงไฟล์ที่ user ไม่มีสิทธิ์  

ถ้าพบ:
- แจ้งเตือนระดับ 2  
- บันทึก log  
- ถ้าซ้ำ 3 ครั้ง → escalate ไป Custodian

---

## 4) Integrity Pipeline (ขั้นตอนยืนยันความถูกต้องของระบบ)

### **STEP 1 — Verify Identity**
ตรวจ token → ตรวจ role → ตรวจที่อยู่ request

### **STEP 2 — Verify Data Integrity**
ตรวจ hash → ตร

เหตุการณ์สำคัญ:
- `INTEGRITY_CHECK_START`  
- `HASH_MISMATCH`  
- `ORIGIN_MISSING`  
- `QUARANTINE_ENTER`  
- `QUARANTINE_EXIT`  
- `INTEGRITY_CONFIRMED`

---

## 7) รายงานสำหรับ Custodian (Custodian Review Pack)
ทุกสัปดาห์ต้องสร้างรายงาน:
- log สรุปเหตุการณ์ผิดปกติ  
- รายการ quarantine  
- hash mismatch  
- จำนวนครั้งที่ระบบต้อง reject  
- พฤติกรรมเสี่ยง  
- แนวโน้ม (trend)

---

## 8) สรุป (Final Notes)
Zero-Trust + Integrity Layer คือเกราะเหล็กของ WM-Core/801  
ช่วยให้:
- ระบบปลอดภัย  
- ข้อมูลไม่ถูกแก้ไข  
- pipeline นิ่ง  
- audit ได้ทุกขั้นตอน  
- นำไปใช้งานระดับองค์กรหรือรัฐได้จริง  

---
