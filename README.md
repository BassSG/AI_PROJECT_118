# Hermes SnD XAUUSD — PWA Launcher

<p align="center">
  <img src="assets/hermes-snd-icon-512.png" width="180" alt="Hermes SnD Premium App Icon">
</p>

GitHub Pages ทำหน้าที่เป็น **PWA launcher / wrapper** สำหรับ Google Apps Script Web App เดิมโดยตรง ไม่ได้คัดลอกหรือสร้าง Dashboard ขึ้นใหม่

## เปิดใช้งาน

- App / GitHub Pages: https://basssg.github.io/AI_PROJECT_117/
- Google Apps Script ต้นฉบับ: https://script.google.com/macros/s/AKfycbweCV370sfJFtoCOR6g19j3cizvUzKZt9JMAbMqVtk5qF0jeG68VDypo6N0FcBNRRi9Iw/exec

`index.html` โหลด URL ของ Google Apps Script เข้าใน application frame โดยตรง ดังนั้นเมื่อ Deploy Apps Script รุ่นใหม่โดยใช้ deployment URL เดิม หน้า GitHub และแอปที่ติดตั้งไว้จะเปิดรุ่นใหม่โดยไม่ต้องคัดลอก source มายัง repository

## ติดตั้งเป็น Application

### Android / Chrome / Edge Desktop

1. เปิด https://basssg.github.io/AI_PROJECT_117/
2. กด **ติดตั้ง App** หรือเมนู **Install app / Add to Home screen**
3. แอปจะใช้ไอคอน Hermes SnD Premium และเปิดแบบ standalone

### iPhone / iPad

1. เปิดลิงก์ด้วย Safari
2. กด Share
3. เลือก **Add to Home Screen**

## โครงสร้าง

- `index.html` — premium PWA launcher และ iframe ไป Apps Script
- `manifest.webmanifest` — ข้อมูลติดตั้งแอป
- `sw.js` — cache เฉพาะ launcher และ assets; ไม่ cache/คัดลอก Apps Script
- `assets/` — SVG และ PNG app icons
- `.github/workflows/pages.yml` — GitHub Pages deployment

## เปลี่ยน Apps Script URL

หากสร้าง deployment ใหม่จน URL เปลี่ยน ให้แก้เพียงค่าต่อไปนี้ใน `index.html`:

```js
const APP_URL='https://script.google.com/macros/s/.../exec';
```

## หมายเหตุ

บาง browser หรือ policy อาจไม่อนุญาตให้บริการภายนอกแสดงใน iframe หน้า launcher จึงมีปุ่ม **เปิดต้นฉบับ** เป็น fallback เพื่อเปิด Google Apps Script โดยตรงเสมอ
