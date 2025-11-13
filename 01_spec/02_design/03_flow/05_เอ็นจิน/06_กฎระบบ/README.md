# 06_กฎระบบ — WM-Core_801
System Rules & Operational Constraints

---

## 1) กฎการไหลของข้อมูล (Data Flow Rules)
- ข้อมูลทุกชนิดต้องผ่าน **Input Layer → Interpretation Layer → Core Processor → IO Engine** เท่านั้น  
- ห้ามข้ามชั้น (No Bypass Rule)  
- ข้อมูลที่เข้าระบบจะถูก **แฮช + ตรึงเวลา (timestamp)** อัตโนมัติ  
- ทุกแพ็กเกจข้อมูลต้องมี:
  - `source`
  - `type` (voice / text / image / system)
  - `intent`
  - `owner`

---

## 2) กฎสำหรับผู้ใช้ (User Rules)
- บทบาทผู้ใช้:
  - `custodian` – ดูแลระบบ / อนุญาตข้อมูล
  - `operator` – ใช้งานระบบ
  - `ai` – ระบบภายใน
- ทุก Session ต้องตรวจสอบตัวตนก่อน  
- ทุกการกระทำของผู้ใช้จะถูกบันทึกลง Log  
- ผู้ใช้ไม่สามารถแก้ไขข้อมูลย้อนหลังได้ (immutable rule)

---

## 3) กฎของ Core Processor (AI Logic Rules)
- AI เริ่มงานได้เมื่อมี `task_id` + `payload` ชัดเจน  
- Task Dispatcher เรียงคิวงาน:
  1. งานด่วน  
  2. งานวิเคราะห์  
  3. งานสร้างคอนเทนต์  
  4. งานระบบ  
- ทุกผลลัพธ์ต้องมี `confidence_score`  
- หาก `confidence < 0.45` → ระบบต้องถามยืนยันจากผู้ใช้

---

## 4) กฎของ Logging Engine (SystemLog Rules)
- รูปแบบ Log:
- ประเภทเหตุการณ์:
- `TASK_START`
- `TASK_END`
- `ERROR`
- `SYSTEM_CHECKPOINT`
- `IO_ACCESS`
- Log ห้ามลบ และต้อง audit ย้อนหลังได้ ≥ 90 วัน

---

## 5) กฎของ IO Channel (Input/Output Rules)
- ช่อง IO ที่อนุญาต:
- `web`
- `mobile`
- `voice`
- `cli`
- Input Format: `text`, `audio`, `json`  
- Output Format: `json`, `markdown`, `image`  
- ข้อมูลที่เป็นภาพ/เสียงต้องผ่าน PDPA Filter

---

## 6) กฎความปลอดภัย (Security Rules)
- ทุกการเขียนข้อมูลต้องแฮช SHA-256  
- ข้อมูลต้องเข้ารหัสระหว่างส่ง  
- ห้ามดึงข้อมูลจากภายนอกโดยไม่ผ่าน Custodian  
- ระบบต้องทำ self-diagnosis ทุก 24 ชม.  
- หากพบ error level ≥ 3 → หยุด queue และแจ้ง Custodian

---

## 7) กฎการเปลี่ยนแปลง (Change Control Rules)
- ทุกแก้ไขต้อง commit พร้อม message ชัดเจน  
- ห้ามสร้างไฟล์ซ้ำใน spec  
- ฟีเจอร์ใหม่ต้องมีคู่ไฟล์:  
- `_design.md`  
- `_flow.md`
- ก่อน merge ต้องผ่าน checklist:
- สอดคล้องกับกฎระบบ  
- ไม่กระทบ core 801  
- ผ่านรีวิว Custodian

---

## 8) สรุป (Final Notes)
กฎเหล่านี้คือโครงสร้างหลักของ WM-Core_801 เพื่อความเสถียร ความปลอดภัย และการเติบโตของระบบในระยะยาว

---
