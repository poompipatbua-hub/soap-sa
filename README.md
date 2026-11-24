# BAAN SOAP — โปรเจกต์ร้านขายสบู่

## ไฟล์ในโปรเจกต์
- `index.html` หน้าร้านค้า (สินค้า 2 SKU, ตะกร้า, checkout)
- `admin.html` หน้าจัดการคำสั่งซื้อ โหมดสาธิต: หยิบสินค้า, เปลี่ยนสถานะ, พิมพ์ใบจ่าหน้า
- `app.js` ข้อมูลตัวอย่างและ logic หน้าร้าน
- `products.csv` ข้อมูลเริ่มต้นสำหรับนำเข้า Google Sheets
- `gas-backend/Code.gs` backend สำหรับเชื่อม Google Sheets
- `assets/` โฟลเดอร์เก็บภาพสินค้า

## 1) ตั้งค่า Google Sheets
1. สร้าง Google Sheet ใหม่
2. เลือก Extensions > Apps Script แล้ววางโค้ดจาก `gas-backend/Code.gs`
3. กด Run ฟังก์ชัน `setup` และอนุญาตสิทธิ์
4. ในชีต Products นำเข้าข้อมูลจาก `products.csv` หรือกรอก SKU, Product, Description, Price, Stock
5. เปลี่ยน `ADMIN_PIN` ใน Code.gs
6. Deploy > New deployment > Web app; Execute as: Me; Who has access: Anyone
7. คัดลอก Web app URL

## 2) เชื่อมหน้าเว็บกับ Google Sheets
เปิด `app.js` แล้วใส่ URL ใน `const API_URL = "";` จากนั้น commit ขึ้น GitHub ใหม่
- เมื่อ API_URL ว่าง ระบบ checkout จะเก็บข้อมูลใน localStorage ของเบราว์เซอร์เท่านั้น (โหมดสาธิต)
- เมื่อใส่ URL แล้ว คำสั่งซื้อใหม่จะถูกบันทึกในแท็บ Orders และ OrderItems
- ตัวอย่าง endpoint สำหรับดึงสินค้า: `?action=products`
- ตัวอย่าง endpoint สำหรับดูออเดอร์หลังเพิ่มระบบหลังบ้าน: `?action=orders&pin=YOUR_PIN`

**ข้อควรทราบ:** หน้า admin ในชุดเริ่มต้นเป็นโหมดสาธิตที่อ่าน localStorage เพื่อให้ทดลอง flow ได้ทันที การใช้งานจริงควรเชื่อมหน้า admin เข้ากับ endpoint ที่ป้องกันด้วย PIN และอ่าน/อัปเดตข้อมูลจาก Google Sheets โดยตรง

## 3) Deploy GitHub + Vercel
1. สร้าง GitHub repository ใหม่ แล้วอัปโหลดไฟล์และโฟลเดอร์ทั้งหมด
2. เข้า Vercel > Add New > Project > Import Git Repository
3. เลือก repository นี้; Framework Preset: Other; Build Command เว้นว่าง; Output Directory `.` 
4. กด Deploy แล้วใช้โดเมน `ชื่อโปรเจกต์.vercel.app`
5. ตรวจสอบ `https://โดเมนของคุณ/` และ `https://โดเมนของคุณ/admin.html`

## 4) ปรับแต่งก่อนส่ง
- เปลี่ยนชื่อร้าน รายละเอียดสินค้า ราคา และรูปภาพตัวอย่างให้เป็นของกลุ่ม
- วางรูปสินค้าใน `assets/` และแก้ส่วน product-art ใน `index.html` หรือข้อมูลใน `app.js`
- ทดสอบสั่งซื้อ 1 รายการ และตรวจสอบข้อมูลใน Google Sheets
- ทดสอบหน้า `admin.html` และพิมพ์ใบจ่าหน้าพัสดุ

## ข้อจำกัดของชุดเริ่มต้น
ไฟล์นี้เป็นเว็บพร้อมใช้งานในโหมดสาธิตและเตรียมจุดเชื่อม Google Sheets แล้ว แต่ยังไม่ได้สร้าง Google Sheet, GitHub repository หรือ deploy Vercel ให้ เพราะต้องดำเนินการผ่านบัญชีของเจ้าของงานและให้สิทธิ์เข้าถึงก่อน ห้ามใช้ข้อมูลลูกค้าจริงในโหมดสาธิต
