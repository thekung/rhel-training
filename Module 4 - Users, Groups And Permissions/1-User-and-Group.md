# การบริหารจัดการ User และ Group ใน RHEL

ในระบบ **RHEL (Red Hat Enterprise Linux)** หรือแม้แต่ใน Linux Distro อื่นก็ตาม การจัดการสิทธิ์และความปลอดภัยเป็นเรื่องสำคัญพื้นฐาน โดยระบบจะเก็บข้อมูลผู้ใช้ไว้ในไฟล์หลักคือ
- `/etc/passwd` (ข้อมูลผู้ใช้)
- `/etc/shadow` (รหัสผ่านที่เข้ารหัส)
- `/etc/group` (ข้อมูลกลุ่ม)

## 1. การบริหารจัดการ User (User Management)

### ประเภทของ User ในระบบ
* **Root User (UID 0):** ผู้ดูแลระบบสูงสุด
* **System User (UID 1-999):** ใช้สำหรับรัน Service หรือแอปพลิเคชัน (เช่น Apache, Database)
* **Normal User (UID 1000+):** ผู้ใช้งานทั่วไปในระบบ

### การสร้าง User แบบต่างๆ
ใช้คำสั่ง `useradd` ในการสร้าง โดยมี Options ที่ต่างกันตามวัตถุประสงค์:

| ประเภทการสร้าง | คำสั่งที่ใช้ |
| :--- | :--- |
| **User ปกติ** | `sudo useradd username` |
| **System User** | `sudo useradd -r serviceuser` |
| **ไม่ให้ Login (No Shell)** | `sudo useradd -s /sbin/nologin username` |
| **กำหนด UID เฉพาะ** | `sudo useradd -u 1500 username` |
| **ใส่หมายเหตุ (Comment)** | `sudo useradd -c "John Doe" username` |

## 2. การจัดการรหัสผ่านและความปลอดภัย

### การตั้งค่ารหัสผ่าน
* **ตั้งรหัสผ่านใหม่:** `sudo passwd username`
* **บังคับเปลี่ยนรหัสผ่านเมื่อ Login ครั้งหน้า:** `sudo passwd -e username`

### การจัดการสถานะ Account (Block/Unlock)
* **Lock ผู้ใช้ (ระงับการใช้งาน):** `sudo usermod -L username`
* **Unlock ผู้ใช้:** `sudo usermod -U username`

### การกำหนดวันหมดอายุ (Password Aging)
ใช้คำสั่ง `chage` (Change Age) เพื่อความปลอดภัยตามนโยบายองค์กร:
* **ดูรายละเอียดการหมดอายุ:** `sudo chage -l username`
* **กำหนดรหัสผ่านหมดอายุทุก 90 วัน:** `sudo chage -M 90 username`
* **กำหนดวันที่ Account จะหมดอายุ (ใช้งานไม่ได้):** `sudo chage -E 2026-12-31 username`

## 3. การบริหารจัดการ Group (Group Management)

Group ช่วยให้การจัดการ Permission ของไฟล์และโฟลเดอร์ทำได้ง่ายขึ้นโดยจัดกลุ่มผู้ใช้ที่มีสิทธิ์เท่ากันเข้าด้วยกัน

* **สร้างกลุ่มใหม่:** `sudo groupadd groupname`
* **เพิ่ม User เข้ากลุ่มเสริม (Secondary Group):** `sudo usermod -aG groupname username` (ต้องใส่ `-a` เพื่อไม่ให้หลุดจากกลุ่มเดิม)
* **ลบ User ออกจากกลุ่ม:** `sudo gpasswd -d username groupname`
* **ลบกลุ่ม:** `sudo groupdel groupname`

## 4. การลบ User ออกจากระบบ

* **ลบเฉพาะ User (เก็บไฟล์ Home ไว้):**
    ```bash
    sudo userdel username
    ```
* **ลบ User พร้อมลบไฟล์ทั้งหมด (Home Directory & Mail Spool):**
    ```bash
    sudo userdel -r username
    ```

## 5. คำสั่งตรวจสอบข้อมูลที่จำเป็น

* `id username`: ดู UID, GID และกลุ่มที่สังกัด
* `whoami`: ตรวจสอบว่าปัจจุบันเราเป็นใคร
* `last`: ตรวจสอบประวัติการ Login ล่าสุด
* `cat /etc/passwd | grep username`: ค้นหาข้อมูล User ในไฟล์ระบบ

> **ข้อแนะนำ:** ใน RHEL แนะนำให้ใช้ `sudo` เสมอแทนการ Login เป็น `root` โดยตรงเพื่อความปลอดภัย และควรตรวจสอบสิทธิ์การเข้าถึงไฟล์หลังจากการเพิ่ม User เข้ากลุ่มด้วยคำสั่ง `ls -l`