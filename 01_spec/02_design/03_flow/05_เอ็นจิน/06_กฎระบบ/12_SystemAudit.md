# 12_SystemAudit — WM-Core_801
System Audit Layer  
โปรโตคอลตรวจสอบคุณภาพ ความปลอดภัย และความถูกต้องทั้งหมดของระบบ 801

---

## 1) วัตถุประสงค์ของ System Audit
ระบบ Audit ใน 801 ถูกสร้างเพื่อ:

- ตรวจสอบจุดผิดปกติที่มนุษย์มองไม่เห็น  
- ย้อนรอยการทำงานของ AI และ Operator  
- ตรวจสอบว่าสเปกแต่ละหน้า (01–11) ถูกปฏิบัติจริงไหม  
- คัดแยกข้อมูลที่เสี่ยง  
- สร้างพื้นฐานสำหรับการตรวจสอบภายนอก (third-party audit)  
- ทำให้ 801 ผ่านมาตรฐานระดับองค์กร / หน่วยงานรัฐ

---

## 2) ขอบเขตการตรวจสอบ (Audit Scope)
Audit ครอบคลุม 6 ส่วนหลัก:

1) **Data Integrity** — hash, lineage, timestamp  
2) **System Behavior** — AI ตอบปกติไหม? มี drift ไหม?  
3) **User Activity** — ใครทำอะไร? เกินสิทธิ์หรือเปล่า?  
4) **Pipeline Flow** — flow เดินตามสเปกไหม? มี bypass ไหม?  
5) **Security** — token, PDPA, access right  
6) **CustodianOps** — คำสั่งลุงถูกบันทึกครบไหม?

---

## 3) Audit Engine (กลไกตรวจสอบอัตโนมัติ)

Audit Engine ทำงานทุก 24 ชม.  
และทุกครั้งที่เกิด Error ระดับ 3

### **Audit Engine ทำ 7 อย่างนี้เสมอ:**

1) ตรวจ hash ทุกไฟล์  
2) ตรวจ trace_id ทุกข้อมูล  
3) ตรวจ lineage map ว่าครบไหม  
4) ตรวจ consent flag (PDPA)  
5) ตรวจสิทธิ์การเข้าถึงย้อนหลัง  
6) ตรวจว่ามี pipeline ไหนโดนข้ามหรือไม่  
7) สร้าง audit report และส่งให้ Custodian

---

## 4) Audit Checklist (รายการตรวจ)

### **A) Data Integrity**
- hash ตรง?  
- timestamp เร็วผิดปกติไหม?  
- มีไฟล์ไหน lineage ขาด?  
- มี data stray (ข้อมูลหลงทาง) ไหม?  

### **B) System Behavior**
- AI ตอบผิดลูปไหม?  
- AI ตอบช้า/ตอบเร็วผิดปกติไหม?  
- intent misclassification เกิดบ่อยไหม?  

### **C) Security**
- token หมดอายุหรือไม่?  
- มีเหตุการณ์ access ผิดสิทธิ์หรือไม่?  
- มีการอัปโหลดไฟล์แปลกปลอมไหม?  

### **D) Custodian**
- มีคำสั่ง override ที่ไม่ได้ลง log ไหม?  
- มีคำสั่ง freeze / quarantine ผิดเวลาไหม?  

### **E) Pipeline Structure**
- ทุก task ผ่านตาม flow page 03 ไหม?  
- มี jump ข้าม Interpretation Layer ไหม?  

### **F) IO Channel**
- มี input จากช่องทางต้องห้ามหรือไม่?  
- มี output leak หรือไม่?  

---

## 5) Audit Log Format (รูปแบบบันทึก)

---

## 6) Audit Report (สรุปรายงานประจำวัน/ประจำสัปดาห์)
Audit Engine ต้องออก 2 แบบ:

### **Daily Report**  
- อัปเดต hash  
- ความผิดปกติเล็ก  
- pipeline hint  

### **Weekly Report**  
- พฤติกรรม AI  
- ข้อมูลเสี่ยง  
- ความผิดปกติสะสม  
- ข้อเสนอแนะระบบระบบ  
- รายการที่ต้องให้ Custodian ตรวจเอง

---

## 7) การแจ้งความผิดปกติ (Audit Alerts)
ระดับของ Alert:

- **Level 1**: แจ้งเตือนธรรมดา  
- **Level 2**: พบ pattern ผิดปกติ  
- **Level 3**: พบบางส่วนของระบบกำลังถูกโจมตี / มีข้อมูลเสี่ยง  
- **Level 4**:  
  หยุดระบบ + แจ้ง Custodian ทันที + เปิดโหมด War-Room  

---

## 8) สิ่งที่ Audit ห้ามละเว้น (Non-Negligible Items)
- PDPA / Consent  
- การเปลี่ยนแปลงสเปก  
- การเข้าไปแก้หลังบ้าน  
- การลบ log  
- การแก้ lineage  
- การเปลี่ยน trace_id  
- การ bypass CustodianOps  

สิ่งเหล่านี้ถือเป็น “critical breach”

---

## 9) แนวทางการต่อยอด (Upgrade Path)
- รองรับ external auditor (บริษัทภายนอก)  
- สร้าง real-time dashboard audit  
- ใช้ anomaly detection จาก ML  
- สร้าง Audit Score ให้ 801  
- ทำ archive รายปีสำหรับ VC / หน่วยงานรัฐ  

---

## 10) สรุป (Final Notes)
System Audit คือเกราะชั้นสุดท้ายของ WM-Core/801  
ช่วยให้ระบบ:
- นิ่ง  
- โปร่งใส  
- audit ได้จริง  
- ขยายได้ระดับองค์กร  
- ปลอดภัยทั้ง field แ
