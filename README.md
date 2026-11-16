# 📦 Google Sheets & LINE Stock Alert System

ระบบจัดการสต็อกสินค้าอัตโนมัติเชื่อมต่อระหว่าง **Google Sheets (Google Forms)** และ **LINE Official Account / Messaging API** พัฒนาด้วย Google Apps Script ช่วยแจ้งเตือนเมื่อสินค้าใกล้หมดอายุ, สรุปสต็อกคงเหลือประจำวัน และบันทึกประวัติสต็อกอัตโนมัติ

---

## 🚀 ฟังก์ชันหลักของระบบ (Features)

1. **🔔 แจ้งเตือนเมื่อฟอร์มถูกส่ง (onFormSubmit):** อัปเดตข้อมูลลงชีต `MasterStock` ทันที และส่งข้อความแจ้งเตือนสถานะสินค้าพร้อมวันหมดอายุเข้ากลุ่ม LINE
2. **🗓️ แจ้งเตือนของใกล้หมดอายุ / หมดอายุรายวัน (checkStockAndAlertDaily):** ตรวจสอบสต็อกล่วงหน้า (ค่าเริ่มต้น 7 วัน) และสรุปยอดคงเหลือประจำวันส่งเข้า LINE
3. **📊 บันทึกประวัติสต็อกอัตโนมัติ (logDailyStockLevels):** สร้างชีตประวัติรายเดือน (เช่น `ประวัติ_11-2025`) และบันทึกจำนวนสต็อกคงเหลือของแต่ละวันแยกตามคอลัมน์ให้อัตโนมัติ
4. **🌐 รองรับ Web App (doGet):** สำหรับทำหน้า Dashboard แสดงผลสต็อก

---

## 🛠️ วิธีการติดตั้งและการตั้งค่า (Installation & Setup)

### 1. เตรียม Google Sheets
* สร้าง Google Sheets ที่มีชีตชื่อ **`MasterStock`** (คอลัมน์ A: ชื่อสินค้า, คอลัมน์ B: จำนวน, คอลัมน์ C: วันหมดอายุ (ISO), ฯลฯ)
* เชื่อมต่อ Google Forms เข้ากับ Google Sheets นี้ โดยตั้งค่าลำดับคำตอบในฟอร์มให้ตรงกับฟังก์ชัน `onFormSubmit` (Timestamp, ItemName, Qty, Expiry, Reporter)

### 2. ติดตั้ง Google Apps Script
1. เปิด Google Sheets -> ไปที่เมนู **ส่วนขยาย (Extensions)** -> **Apps Script**
2. คัดลอกโค้ดจากไฟล์ `Code.gs` ในโปรเจกต์นี้ไปวางแทนที่
3. บันทึกโปรเจกต์ (Ctrl + S)

### 3. ตั้งค่าความปลอดภัย (Script Properties) **สำคัญมาก!**
เพื่อป้องกันไม่ให้ Token และ Group ID รั่วไหลบน GitHub อย่าใส่ค่าจริงลงไปในโค้ดตรงๆ แต่ให้ตั้งค่าผ่าน **Script Properties** ดังนี้:
1. ไปที่หน้าโปรเจกต์ Apps Script -> คลิกไอคอน **เฟือง (Project Settings)** ทางซ้าย
2. เลื่อนลงมาที่หัวข้อ **Script Properties** -> คลิก **Add script property**
3. เพิ่มตัวแปรดังนี้:
   * **Property:** `LINE_CHANNEL_ACCESS_TOKEN` | **Value:** `ใส่ LINE Channel Access Token ของคุณ`
   * **Property:** `LINE_TARGET_GROUP_ID` | **Value:** `ใส่ LINE Group ID ของคุณ`

### 4. การตั้งเวลาทำงานอัตโนมัติ (Triggers)
1. ไปที่เมนู **นาฬิกาปลุก (Triggers)** ทางซ้ายมือของ Apps Script
2. ตั้งค่า Trigger สำหรับฟังก์ชัน:
   * `onFormSubmit`: เลือกอีเวนต์เป็น **From spreadsheet** -> **On form submit**
   * `checkStockAndAlertDaily` และ `logDailyStockLevels`: เลือกอีเวนต์เป็น **Time-driven** (เช่น ตั้งเวลาให้รันทุกวัน เวลา 08:00 - 09:00 น.)

---

## 📝 โครงสร้างโค้ด (Code Structure)

* `onFormSubmit(e)`: จัดการข้อมูลเมื่อมีการส่ง Google Form
* `checkStockAndAlertDaily()`: ตรวจสอบวันหมดอายุและส่งสรุปสต็อกประจำวันทาง LINE
* `logDailyStockLevels()`: บันทึกสต็อกรายวันลงชีตประวัติแยกตามเดือน
* `parseDateSafe(dateInput)`: ฟังก์ชันแปลงวันที่ปลอดภัยรองรับทั้งรูปแบบ `DD/MM/YYYY` และ ISO String
* `formatThaiDate(date)`: จัดรูปแบบวันที่เป็นภาษาไทย (เช่น 11 พ.ย. 2025)
* `sendLineMessage(message)`: ฟังก์ชันกลางสำหรับยิงข้อความผ่าน LINE Messaging API

---

## 🤝 การมีส่วนร่วม (Contributing)
หากต้องการเสนอแนะปรับปรุงโค้ดหรือแจ้งปัญหา สามารถเปิด **Pull Request** หรือ **Issue** ไว้ได้เลยครับ!
