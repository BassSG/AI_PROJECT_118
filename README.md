# Hermes SnD XAUUSD — Embedded PWA

<p align="center"><img src="assets/hermes-snd-icon-512.png" width="180" alt="Hermes SnD Premium App Icon"></p>

GitHub Pages แสดง Google Apps Script Web App อยู่ภายในหน้า Application โดยตรงผ่าน iframe ที่ได้รับอนุญาตด้วย `HtmlService.XFrameOptionsMode.ALLOWALL`

## เปิดใช้งาน

- GitHub Pages / PWA: https://basssg.github.io/AI_PROJECT_118/
- Google Apps Script deployment: https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec

รุ่นที่ใช้งานปัจจุบัน: **Hermes SnD v1.9.0 Balanced Auto / Apps Script deployment version 21**

- ใช้ `openai/gpt-5.6-luna-pro` เป็นค่าเริ่มต้น และเลือกต่อรอบได้ 3 โมเดล: Luna Pro, Terra Pro และ Sol Pro
- Server ตรวจ Allowlist ทุกครั้ง จึงไม่รับชื่อโมเดลอื่นจากการแก้ค่าบน Browser
- Balanced Auto ใช้เกณฑ์เริ่มต้น Zone ≥68, Structural RR ≥2.00, AI confidence ≥68% และ Retest ≤1
- การทดสอบโซนครั้งแรก (`TESTED`, touches=1) และ Momentum M15/M5 ที่เป็น Pullback จะแสดงเป็นคำเตือน ไม่ถูกใช้เป็นเหตุปฏิเสธเพียงอย่างเดียว
- กฎความปลอดภัยหลักยังอยู่ครบ: H4/H1 ต้องตรงทิศ, TP ต้องมีโครงสร้าง, ราคา/SL/TP ต้องถูกด้าน, โซน `USED_UP` และ Retest เกิน 1 ครั้งยังถูกบล็อก

- หน้าเว็บแสดงทุกเหตุการณ์ของ Full Pipeline ทั้ง `PASS`, `FAIL`, `STOP`, `BYPASS` และเหตุผล
- ผล `NO_TRADE` แสดงรายละเอียดครบ แต่ไม่ส่ง `PLACE_PENDING`
- ส่ง Order Webhook เฉพาะแผนที่ผ่านทุก Gate และแสดงสถานะ `SENT_ACCEPTED`, `SENT_REJECTED` หรือ `NOT_SENT`
- การวิเคราะห์จากหน้าเว็บใช้ผู้ส่ง `EBassWave`; การรันจาก GAS ใช้ผู้ส่ง `1-more`
- Endpoint URL และ Query Token ถูกจัดการใน `Code.gs` โดยตรง ไม่อ่าน `ENDPOINT_URL` หรือ `ENDPOINT_SECRET` จาก Script Properties
- Endpoint Queue ตอบกลับด้วย `stored_event_id` และระบบบันทึกสถานะเป็น `QUEUED`; หาก Endpoint/Auth ไม่พร้อมจะถอยเป็น Analysis-only อย่างปลอดภัย
- หน้า Dashboard แสดง Endpoint URL, Query-token Auth, Emergency Stop และ Weekend policy แยกกันชัดเจน
- ป๊อปอัปผลลัพธ์สรุปเหตุผล แผนที่ตรวจ และสถานะ Webhook เป็นภาษาอ่านง่าย โดยพับ Pipeline ทุกขั้นและข้อมูลอ้างอิงไว้ให้เปิดดูเมื่อจำเป็น
- ป๊อปอัปบนมือถือเลื่อนเนื้อหาได้ พร้อมปุ่มปิดด้านบน, “รับทราบ” และ “ดูผลล่าสุด” ที่กดได้เสมอ
- AI Diagnostic จะไม่ถูกเรียกซ้ำเพื่อคิดค่าใช้จ่ายเมื่อรอบหยุดด้วย `CONFIG_INVALID`
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
