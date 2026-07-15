# Hermes SnD XAUUSD — Embedded PWA

<p align="center"><img src="assets/hermes-snd-icon-512.png" width="180" alt="Hermes SnD Premium App Icon"></p>

GitHub Pages แสดง Google Apps Script Web App อยู่ภายในหน้า Application โดยตรงผ่าน iframe ที่ได้รับอนุญาตด้วย `HtmlService.XFrameOptionsMode.ALLOWALL`

## เปิดใช้งาน

- GitHub Pages / PWA: https://basssg.github.io/AI_PROJECT_118/
- Google Apps Script deployment: https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec

รุ่นที่ใช้งานปัจจุบัน: **Hermes SnD v1.9.8 Evidence Mode + Outcome Monitor / Apps Script deployment version 35**

- LINE OA ส่ง Flex Message ดีไซน์ Luxury Trading Terminal พร้อมโลโก้ Hermes, Entry Hero, การ์ด TP/SL, RR, QC%, SCORE และ Market Structure H4/H1/M15/M5 ในรูปแบบกะทัดรัดสำหรับมือถือ
- Channel Access Token และ Recipient ID เก็บใน Google Apps Script Properties เท่านั้น ไม่อยู่ในหน้าเว็บหรือ GitHub
- หาก LINE ส่งไม่สำเร็จ ระบบจะบันทึก Error แยกต่างหากและไม่เปลี่ยนผล Order ที่ Endpoint รับไปแล้ว

### Dual Automation Control

- `Web Auto` วิเคราะห์ทุก 15 นาทีในนาม `EBassWave` เฉพาะขณะที่หน้าเว็บเปิดอยู่ และใช้ M15 Candle Guard ป้องกันการเรียก AI ซ้ำบนแท่งเดิม
- `GAS Auto 24/7` วิเคราะห์ทุก 15 นาทีในนาม `1-more` แม้ปิดหน้าเว็บ โดยเปิดหรือปิด Trigger ได้จาก Dashboard
- ระบบไม่อนุญาตให้ Web Auto และ GAS Auto ทำงานพร้อมกัน เพื่อลดรอบวิเคราะห์ซ้ำและควบคุมค่า AI
- การควบคุม GAS Auto ใช้รหัส 6 หลักเพื่อจับคู่อุปกรณ์และ Session ที่ลงลายเซ็นอายุ 30 วัน โดยไม่ต้องยืนยันบัญชี Google
- การเปิด Automation ไม่เปลี่ยน Execution Gate: Webhook ยังคงส่งเฉพาะ `AUTO_PLACE_PENDING` ที่ผ่านทุกกฎ

### Outcome Monitor และสถิติออเดอร์

- GAS ตรวจออเดอร์ที่ยังทำงานทุก 5 นาที โดยตรวจสถานะจาก MT5 Endpoint ก่อน แล้วใช้แท่ง M5 จาก FMP เป็นชั้น Audit สำหรับ Entry, TP, SL และ Expired
- รอบตรวจออเดอร์ไม่เรียก AI (`aiCalls=0`) และเมื่อไม่มีออเดอร์ที่ต้องติดตามจะไม่เรียกทั้ง FMP และ Endpoint
- สถานะจาก MT5 เป็นข้อมูลหลัก ส่วนผลจาก FMP ติดป้าย `FMP_AUDIT`; ถ้า Entry/TP/SL ชนกันภายในแท่งเดียว ระบบบันทึก `AMBIGUOUS` และรอ MT5 ยืนยันโดยไม่เดาลำดับราคา
- `OrderChecks` เก็บผลตรวจราคาแต่ละรอบ, `Outcome` เก็บเหตุการณ์วงจรออเดอร์, `Orders` เก็บสถานะล่าสุด และ `Logs`/`PipelineTrace`/`RunReport` เก็บการทำงานและการวิเคราะห์ย้อนหลัง
- Dashboard แสดง Entry Filled, TP, SL, Win Rate, Realized RR, Ambiguous, สุขภาพ Trigger และเหตุการณ์ Outcome ล่าสุด

- ใช้ `openai/gpt-5.6-luna-pro` เป็นค่าเริ่มต้น และเลือกต่อรอบได้ 3 โมเดล: Luna Pro, Terra Pro และ Sol Pro
- Server ตรวจ Allowlist ทุกครั้ง จึงไม่รับชื่อโมเดลอื่นจากการแก้ค่าบน Browser
- Evidence Mode ใช้เกณฑ์เริ่มต้น Zone ≥68, Structural RR ≥2.00, AI confidence ≥68% และ Retest ≤1
- การทดสอบโซนครั้งแรก (`TESTED`, touches=1) และ Momentum M15/M5 ที่เป็น Pullback จะแสดงเป็นคำเตือน ไม่ถูกใช้เป็นเหตุปฏิเสธเพียงอย่างเดียว
- H4 เป็นกรอบทิศทางหลัก; เมื่อ H1 อยู่ช่วง `SIDEWAYS` จะผ่านได้เฉพาะเมื่อ M15 ยืนยันไปทางเดียวกับ H4 ส่วน H4 สวนทางหรือ Sideway ยังถูกบล็อก
- TP1 สมดุลไว้ที่ 3R เมื่อมีพื้นที่ก่อนโซนตรงข้าม และเก็บขอบโซนตรงข้ามไว้เป็น TP2 เพื่อไม่ให้ RR สูงเกินจริง
- เพดานคำสั่งที่ผ่านคือ 5 แผนต่อวัน ไม่ใช่โควตาบังคับ ระบบจะไม่สร้างออเดอร์ปลอมเมื่อหลักฐานไม่พอ
- Dashboard แสดง Analysis Funnel 24 ชั่วโมง: จำนวนรอบ, แผนที่สร้างได้, AI Review, แผนที่อนุมัติ, Webhook ที่ส่ง และ Gate ที่ตัดบ่อยที่สุด
- กฎความปลอดภัยหลักยังอยู่ครบ: TP ต้องมีโครงสร้าง, ราคา/SL/TP ต้องถูกด้าน, โซน `USED_UP`, Retest เกิน 1 ครั้ง, ความเสี่ยง 0.5%, Pending/Position อย่างละไม่เกิน 1 และ Daily Loss Guard 2%

- หน้าเว็บแสดงทุกเหตุการณ์ของ Full Pipeline ทั้ง `PASS`, `FAIL`, `STOP`, `BYPASS` และเหตุผล
- เมื่อสร้าง Candidate ได้ ระบบจะแสดง Best Analysis Plan พร้อม `BUY_LIMIT`/`SELL_LIMIT`, Entry, SL, TP และ RR แม้ผลสุดท้ายเป็น `NO_TRADE`
- แผนที่ไม่ผ่าน Execution Gate จะติดป้าย `REFERENCE_ONLY` และ `NOT_SENT` ชัดเจน โดยไม่ส่ง `PLACE_PENDING`
- OpenRouter ถูกเรียกเพื่อ Review เมื่อมี Candidate อย่างน้อยหนึ่งแผน แม้แผนนั้นยังไม่ผ่าน Auto Gate เพื่อให้เห็นคำอธิบายและ Token/Cost จริง
- ถ้าไม่มี Candidate ที่สร้าง Entry/SL/TP ได้ ระบบจะไม่เรียก AI, แสดง `NOT_CALLED_NO_CANDIDATE` และค่า AI เป็นศูนย์
- ส่ง Order Webhook เฉพาะแผนที่ผ่านทุก Gate และแสดงสถานะ `SENT_ACCEPTED`, `SENT_REJECTED` หรือ `NOT_SENT`
- การวิเคราะห์จากหน้าเว็บใช้ผู้ส่ง `EBassWave`; การรันจาก GAS ใช้ผู้ส่ง `1-more`
- Endpoint URL และ Query Token ถูกจัดการใน `Code.gs` โดยตรง ไม่อ่าน `ENDPOINT_URL` หรือ `ENDPOINT_SECRET` จาก Script Properties
- Endpoint Queue ตอบกลับด้วย `stored_event_id` และระบบบันทึกสถานะเป็น `QUEUED`; หาก Endpoint/Auth ไม่พร้อมจะถอยเป็น Analysis-only อย่างปลอดภัย
- หน้า Dashboard แสดง Endpoint URL, Query-token Auth, Emergency Stop และ Weekend policy แยกกันชัดเจน
- ป๊อปอัปผลลัพธ์แยก Best Analysis Plan, สถานะแผน, AI Review/Cost และ Webhook เป็นช่องอ่านง่าย โดยพับ Pipeline ทุกขั้นและข้อมูลอ้างอิงไว้ให้เปิดดูเมื่อจำเป็น
- ป๊อปอัปบนมือถือเลื่อนเนื้อหาได้ พร้อมปุ่มปิดด้านบน, “รับทราบ” และ “ดูผลล่าสุด” ที่กดได้เสมอ
- ระบบไม่เรียก AI Diagnostic แฝงเมื่อ Pipeline ไม่มี Candidate; การทดสอบ AI จะเกิดเฉพาะเมื่อผู้ใช้กดคำสั่ง “ทดสอบ AI” โดยตรง
- ค่าใช้จ่าย AI คำนวณตามโมเดลที่เลือกจริงในแต่ละรอบ และผลสรุปแสดงโมเดลที่ใช้เพื่อ Audit ย้อนหลัง

เมื่อ Deploy Google Apps Script รุ่นใหม่ด้วย deployment URL เดิม หน้า GitHub และ Application ที่ติดตั้งไว้จะโหลดรุ่นใหม่โดยอัตโนมัติ โดยไม่ต้องคัดลอก source หรือ Dashboard มายัง repository

## ติดตั้งเป็น Application

- Android/Chrome/Edge Desktop: เปิด GitHub Pages แล้วเลือก **ติดตั้ง App** หรือ **Install app / Add to Home screen**
- iPhone/iPad: เปิดด้วย Safari → Share → **Add to Home Screen**

## ไฟล์สำคัญ

- `index.html` — PWA shell และ iframe ไปยัง Apps Script deployment
- `manifest.webmanifest` — application metadata
- `sw.js` — cache เฉพาะ GitHub shell และ assets ไม่ cache Apps Script
- `assets/` — Premium SVG/PNG icons สำหรับมือถือและคอมพิวเตอร์

## เปลี่ยน Deployment URL

หากสร้าง Deployment ใหม่จน URL เปลี่ยน ให้แก้ค่าเดียวใน `index.html`:

```js
const APP_URL='https://script.google.com/macros/s/.../exec';
```

Apps Script `doGet()` ต้องมี:

```js
.setXFrameOptionsMode(HtmlService.XFrameOptionsMode.ALLOWALL)
```

สิทธิ์ผู้ใช้และคำสั่งสำคัญต้องตรวจฝั่ง Apps Script/server เสมอ เพราะ `ALLOWALL` อนุญาตให้เว็บไซต์ภายนอกฝัง Web App ได้
