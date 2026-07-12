# Hermes SnD XAUUSD — Embedded PWA

<p align="center"><img src="assets/hermes-snd-icon-512.png" width="180" alt="Hermes SnD Premium App Icon"></p>

GitHub Pages แสดง Google Apps Script Web App อยู่ภายในหน้า Application โดยตรงผ่าน iframe ที่ได้รับอนุญาตด้วย `HtmlService.XFrameOptionsMode.ALLOWALL`

## เปิดใช้งาน

- GitHub Pages / PWA: https://basssg.github.io/AI_PROJECT_117/
- Google Apps Script deployment: https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec

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
