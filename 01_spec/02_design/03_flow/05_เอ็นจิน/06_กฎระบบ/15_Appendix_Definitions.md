# 15_Appendix & Definitions — WM-Core/801
เอกสารอ้างอิงศัพท์, คำจำกัดความ, และ Appendix สำหรับระบบ WM-Core/801

---

# 1) Glossary — พจนานุกรมระบบ

## **801 / WM-Core**
ชุดแกนกลางของระบบ World Model ที่ออกแบบให้รองรับ:
- Multi-Input (Text, Voice, Image)
- Multi-Agent
- Field-to-Lab Pipeline
- Interpretation Layer
- Zero-Trust Data Flow

---

## **Custodian**
บทบาทผู้ถือสิทธิ์ข้อมูลสูงสุดในระบบ  
มีอำนาจ override, ปรับทิศ, ตัดสิน “ความหมายสุดท้าย” ของข้อมูล

---

## **Interpretation Layer**
ชั้นตีความ “เสียงมนุษย์ → ความหมาย”  
แยกเป็น 3 ส่วน:
1. Intent  
2. Emotion  
3. Pattern  

---

## **Zero-Trust Layer**
ระบบตรวจสอบความปลอดภัย  
ทุก request ต้องได้รับการยืนยัน:
- hash
- metadata
- permission
- PDPA masking

---

## **Field Node**
จุดรับข้อมูลจริงจากพื้นที่ เช่น:
- ร้านค้า
- ตลาด
- เสียง SME
- สถานที่ภาคสนาม

---

## **Lab Node**
จุดประมวลผล/วิเคราะห์ เช่น:
- GPT / Claude Ops  
- DeepSeek Logic  
- Gemini Drive Ops  

เป็นที่ที่ข้อมูล “ถูกเปลี่ยนเป็น Insight”

---

## **Voice Pipeline**
เส้นทางข้อมูลเสียง:
voice → emotion → transcript → intent → summary → output hash

---

## **Image Pipeline**
image → metadata → tagging → suggestion → base64 output

---

## **JSON Output v1.3**
รูปแบบมาตรฐานสำหรับ dev:

---

## **Trace ID**
หมายเลขติดตามทุก action เช่น:
- คำสั่ง  
- เสียง  
- ไฟล์  
- Knowledge update  

ใช้ผูก log และ ledger

---

## **LedgerLog / Auto-Sign**
ระบบบันทึก hash อัตโนมัติ  
“ลายเซ็นของข้อมูล”  
บังคับใช้กับทุก output ที่ออกจาก 801

---

## **PDPA Shield**
เลเยอร์กลั่นกรองข้อมูลที่มีมนุษย์:
- ชื่อ  
- เบอร์  
- หน้าคน  
- เสียง  
- ตำแหน่งที่อยู่  

ต้องถูก mask ก่อนออกจากระบบ

---

## **Routing Engine**
ตัวเลือกเส้นทาง:
- Text → Text Engine  
- Voice → Interpretation Engine  
- Image → Vision Engine  
- CLI → System Engine  

---

## **Multi-Agent Sync**
กลไกประสานงาน:
- GPT = Final Synthesis  
- Claude = PDPA/ Ethics  
- DeepSeek = Logic/Tagging  
- Gemini = Sheets/Drive Ops  

---

# 2) Appendix A — Metadata Standards

| Field | Type | Example | Requirement |
|------|------|---------|-------------|
| `trace_id` | string | "T801-2025-001" | required |
| `timestamp` | int | 1731500000 | required |
| `owner` | string | "custodian" | required |
| `source` | string | "mobile/voice/cli/web" | required |
| `intent` | string? | "report" | optional |
| `location` | geohash? | "w21x3" | optional |
| `role` | string | "custodian/system" | optional |

---

# 3) Appendix B — Intent Types (v1.0)

### **A. Informative**
- ask  
- summary  
- checklist  
- explain  
- compare  

### **B. Operational**
- override  
- system  
- routing  
- generate  
- classify  

### **C. Field**
- capture  
- diary  
- transcript  
- observation  

### **D. Creative**
- design  
- narrative  
- brief  
- visual  

---

# 4) Appendix C — Error Codes


---

# 5) Appendix D — Pipeline Diagram (Mermaid)

```mermaid
flowchart TD
    A[Input] --> B{Type?}
    B -->|Text| C[Text Engine]
    B -->|Voice| D[Voice Engine]
    B -->|Image| E[Vision Engine]
    C --> F[Interpretation Layer]
    D --> F
    E --> F
    F --> G[Zero-Trust Layer]
    G --> H[Output Engine]
    H --> I[JSON/Markdown/Image]
[YYYYMMDD]_[ProjectTag]_[FileCode]_[vX].md

ตัวอย่าง:
20251113_WM_InterpretationLayer_v1.md
20251113_801_InterfaceSpec_v1.md

---

ลุง… พร้อมไปหน้า 16 ไหม  
หน้า 16 = **WM-Core/801 System Contract (สัญญาการทำงานของระบบ)**  
อันนี้เป็นหน้าโคตรสำคัญ เวลาเอาไปคุยกับ VC หรือ dev จะดูโปรมาก 💥
