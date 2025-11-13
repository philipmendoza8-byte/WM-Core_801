# 14_InterfaceSpec — WM-Core_801
Interface Specification  
สเปกการเชื่อมต่อข้อมูลทั้งหมดของระบบ WM-Core/801

---

## 1) ภาพรวม (Overview)
Interface Specification นี้ใช้กำหนด:
- รูปแบบข้อมูลเข้าออก  
- ช่องทางรับ–ส่ง  
- โครงสร้าง payload  
- กฎการตรวจสอบ (validation rules)  
- มาตรฐาน I/O ที่ใช้ในระบบ WM-Core/801

เชื่อมตรงกับหน้า 07, 10, 12, 13

---

## 2) ประเภทอินพุต (Input Types)

### **A. Text Input**
- ชนิดข้อมูล: `string`  
- การเข้ารหัส: UTF-8  
- ขนาดสูงสุด: 10,000 ตัวอักษร  
- Metadata บังคับ:
  - `source`
  - `owner`
  - `intent?`
  - `timestamp`
  - `trace_id`

---

### **B. Voice Input**
- ชนิดข้อมูล: audio stream (.wav, .m4a)  
- Sample rate: 16kHz / 44.1kHz  
- จำเป็นต้องผ่าน:
  - Speech-to-Text  
  - Emotion Analyzer  
  - Noise Filter  
- Metadata บังคับ:
  - `duration`
  - `audio_quality`
  - `speaker?`
  - `timestamp`

---

### **C. Image Input**
- ชนิดข้อมูล: JPEG / PNG / Base64  
- ขนาดสูงสุด: 5–10MB  
- ขั้นตอน:
  - Metadata extraction  
  - Vision Analysis  
  - Hash check  
  - PDPA mask (ถ้ามี human face)  

---

## 3) Output Types

### **A. JSON Output**
สำหรับ developer / CLI / API:

---

### **B. Markdown Output**
สำหรับรายงาน / summary / deck:
- สรุป  
- แผนภาพ  
- bullet list  
- table  
- ข้อเสนอแนะ  

---

### **C. Image Output**
- ใช้สำหรับ diagrams / analysis  
- อาจถูก encode Base64  
- มี hash กำกับเสมอ

---

## 4) I/O Channels (ช่องทางรับ-ส่ง)

| Channel | รับเข้า | ส่งออก | หมายเหตุ |
|--------|---------|--------|----------|
| Web | text, image | json, md, image | UI ทั่วไป |
| Mobile | text, voice, image | json, md, image | รองรับกล้อง/ไมค์ |
| CLI | text, json | json | สำหรับ dev |
| Voice | voice stream | text/json | มี emotion analysis |

---

## 5) โครงสร้าง Payload มาตรฐาน (Standard Payload Format)

### **Input Payload**

### **Output Payload**

---

## 6) Validation Rules (กฎตรวจสอบข้อมูล)

### **Level 1 — Format Check**
- ชนิดข้อมูลตรงไหม?  
- Payload ว่างไหม?  
- Encoding ถูกต้องไหม?

### **Level 2 — Metadata Check**
ต้องมี:
- `trace_id`  
- `timestamp`  
- `owner`  
- `source`

### **Level 3 — Security Check**
- PDPA  
- hash  
- token  
- permission  

### **Level 4 — Behavioral Check**
- ขนาดข้อมูลผิดปกติไหม?  
- เวลา request แปลกไหม?  
- ส่งซ้ำถี่เกินไป?

---

## 7) I/O Error Codes

---

## 8) Interface Policy (ข้อกำหนดสำคัญ)
- I/O ทุกชนิดต้องผ่าน Zero-Trust Layer  
- ห้าม bypass Interpretation Layer  
- ต้องมี hash ทุกครั้งที่ข้อมูลออก  
- ต้องบันทึกเข้า Log ทุกครั้ง  
- ต้องตรวจ permission ทุกครั้งก่อนตอบกลับ  
- Custodian override ได้เสมอ

---

## 9) Use Cases ของ Interface 801

### **Use Case 1 — รับเสียง SME**
voice input → transcript + emotion → intent → summary markdown

### **Use Case 2 — รับภาพสินค้า**
image input → detect → metadata → suggestion → image output

### **Use Case 3 — คำสั่ง CLI จากลุง**
cli → override command → system engine → json output

### **Use Case 4 — summary สั้นเพื่อ VC**
text input → interpretation → markdown summary

---

## 10) สรุป (Final Notes)
InterfaceSpec หน้า 14 คือสเปกการเชื่อมต่อแบบเต็มของ 801  
ใช้เพื่อ:
- ออกแบบ API  
- สร้าง UI  
- ทำระบบเชื่อมต่อ  
- อธิบายงาน dev และ partner  
- ปูพื้นระบบ field ไป lab  

---
