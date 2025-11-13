# 04_สคีมา — WM-Core_801

## User
- **user_id:** string  
- **role:** ["custodian", "operator", "ai"]  
- **fullname:** string  
- **phone:** string  
- **permissions:** list  
- **created_at:** datetime  

---

## Task
- **task_id:** string  
- **user_id:** string  
- **task_type:** ["voice", "text", "image", "system"]  
- **payload:** object  
- **status:** ["pending", "processing", "done", "error"]  
- **timestamp:** datetime  

---

## SystemLog
- **log_id:** string  
- **event_type:** string  
- **details:** object  
- **source:** ["AI", "USER", "SYSTEM"]  
- **created_at:** datetime  

---

## VoiceMapping
- **voice_id:** string  
- **user_id:** string  
- **transcript:** text  
- **emotion:** string  
- **ai_tag:** list  
- **confidence:** float  
- **created_at:** datetime  

---

## IO_Channel
- **channel_id:** string  
- **source:** ["web", "mobile", "voice", "cli"]  
- **input_format:** ["text", "audio", "json"]  
- **output_format:** ["json", "markdown", "image"]  
- **endpoint:** string  
- **last_used:** datetime  
