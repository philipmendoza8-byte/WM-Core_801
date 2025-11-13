# 05_เอ็นจิน — WM-Core_801

## 1) User → Input Layer → Interpretation
- ผู้ใช้ส่งข้อมูลเข้ามา เช่น ข้อความ, เสียง, ภาพ
- Input Layer ตรวจรูปแบบ
- Interpretation แปลงเป็นโครงสร้างที่ระบบเข้าใจ

---

## 2) Core Processor → Task Dispatcher
- Core Processor วิเคราะห์ว่าควรประมวลผลแบบไหน
- Task Dispatcher กระจายงานไปยังโมดูล:
  - Voice Engine
  - Text Engine
  - Image Engine
  - System Engine

---

## 3) Logging Engine → IO Engine → Output
- Logging Engine บันทึกทุก event
- IO Engine จัดรูปแบบผลลัพธ์ตามช่องทาง (web, mobile, voice)
- Output ส่งผลกลับสู่ผู้ใช้

