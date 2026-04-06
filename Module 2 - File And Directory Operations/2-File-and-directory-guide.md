# การจัดการ Filesystem และคำสั่งพื้นฐานใน RHEL

ระบบไฟล์ของ RHEL ปฏิบัติตามมาตรฐาน **Filesystem Hierarchy Standard (FHS)** ซึ่งจัดระเบียบข้อมูลเป็นโครงสร้างต้นไม้ โดยเริ่มจาก Root directory (`/`)

---

## 1. โครงสร้าง Directory ที่สำคัญใน RHEL

| Directory | คำอธิบาย |
| :--- | :--- |
| `/bin` & `/usr/bin` | เก็บคำสั่งมาตรฐานที่ผู้ใช้ทุกคนใช้งาน (User Executable Commands) |
| `/sbin` & `/usr/sbin` | เก็บคำสั่งสำหรับการบริหารจัดการระบบ (System Administration Binaries) เช่น `fdisk`, `reboot` |
| `/etc` | **แหล่งเก็บไฟล์ Configuration** ของระบบและโปรแกรมต่างๆ |
| `/dev` | เก็บไฟล์ที่เชื่อมต่อไปยัง Hardware (Device Files) เช่น `/dev/sda` |
| `/var` | เก็บข้อมูลที่มีการเปลี่ยนแปลงบ่อย เช่น Log files (`/var/log`), ฐานข้อมูล, Mail spool |
| `/home` | Directory ส่วนตัวของผู้ใช้งานทั่วไป |
| `/root` | Home directory สำหรับ User root (Superuser) |
| `/tmp` | เก็บไฟล์ชั่วคราว (จะถูกลบเมื่อ Restart หรือตามรอบการทำความสะอาด) |
| `/boot` | เก็บไฟล์ที่ใช้ในการ Boot ระบบ เช่น Kernel และ GRUB bootloader |
| `/run` | เก็บข้อมูล Runtime ของ Process ที่กำลังทำงานอยู่ (เช่น PID files) |

---

## 2. คำสั่งจัดการ Directory (Navigation & Management)

### การเคลื่อนที่และการตรวจสอบ
* `pwd`: (**P**rint **W**orking **D**irectory) แสดงตำแหน่งปัจจุบันว่าอยู่ที่ไหน
* `ls`: รายชื่อไฟล์และ directory
    * `ls -l`: แสดงรายละเอียด (Permission, Owner, Size, Date)
    * `ls -a`: แสดงไฟล์ที่ถูกซ่อน (ไฟล์ที่ขึ้นต้นด้วยจุด `.`)
* `cd [path]`: เปลี่ยนไปยัง directory ที่ระบุ
    * `cd ~`: กลับไปยัง Home directory ของ user ปัจจุบัน
    * `cd ..`: ย้อนกลับไป directory ชั้นบน (Parent Directory)

### การสร้างและลบ
* `mkdir [name]`: สร้าง directory ใหม่
    * `mkdir -p path/to/dir`: สร้าง directory ซ้อนกันหลายชั้น (Parent directories)
* `rmdir [name]`: ลบ directory เปล่า

---

## 3. คำสั่งจัดการไฟล์ (File Manipulation)

### การสร้างและแก้ไข
* `touch [filename]`: สร้างไฟล์เปล่า หรืออัปเดต timestamp
* `cp [source] [dest]`: คัดลอกไฟล์
    * `cp -r`: คัดลอกทั้ง directory (Recursive)
* `mv [source] [dest]`: ย้ายไฟล์ หรือ ใช้ในการเปลี่ยนชื่อไฟล์
* `rm [filename]`: ลบไฟล์
    * `rm -f`: ลบโดยไม่ต้องยืนยัน (Force)
    * `rm -rf`: ลบ directory และไฟล์ข้างในทั้งหมด (ใช้ด้วยความระมัดระวังสูงสุด)

### การดูเนื้อหาไฟล์
* `cat [file]`: แสดงเนื้อหาทั้งหมดในครั้งเดียว
* `less [file]`: เปิดดูเนื้อหาแบบเลื่อนขึ้น-ลงได้ (เหมาะกับไฟล์ที่มีความยาวมาก)
* `head -n [number] [file]`: ดูส่วนหัวของไฟล์ตามจำนวนบรรทัดที่ระบุ
* `tail -n [number] [file]`: ดูส่วนท้ายของไฟล์
    * `tail -f [file]`: ติดตามการเปลี่ยนแปลงของไฟล์แบบ Real-time (นิยมใช้ Monitor Log)

---

## 4. การจัดการสิทธิ์และเจ้าของไฟล์ (Permissions & Ownership)

ใน RHEL ความปลอดภัยเป็นเรื่องสำคัญ ทุกไฟล์จะมี Permission กำหนดไว้ (Read, Write, Execute)

* `chmod [mode] [file]`: เปลี่ยนสิทธิ์การเข้าถึงไฟล์ (เช่น `chmod 755 script.sh`)
* `chown [user]:[group] [file]`: เปลี่ยนเจ้าของไฟล์และกลุ่ม (Ownership)
* `ls -l`: ใช้ตรวจสอบ Permission
    * ตัวอย่าง: `-rwxr-xr--`
    * `r` = read, `w` = write, `x` = execute

---

## 5. การค้นหาข้อมูล (Searching)

* `find [path] -name [filename]`: ค้นหาไฟล์ตามชื่อจากตำแหน่งที่ระบุ
* `grep "[pattern]" [file]`: ค้นหาข้อความหรือคำที่ต้องการภายในไฟล์
* `locate [filename]`: ค้นหาไฟล์จาก Database ของระบบ (ทำงานเร็วกว่า find แต่อาจต้องอัปเดต DB ด้วยคำสั่ง `updatedb`)

---

> **ข้อควรระวัง (Best Practice):** การใช้งานคำสั่งกลุ่ม `rm`, `chown`, และ `chmod` กับไฟล์ระบบสำคัญ (`/etc`, `/usr`, `/var`) ควรใช้งานผ่าน `sudo` และตรวจสอบ Path ให้ถูกต้องก่อนรันคำสั่งเสมอ