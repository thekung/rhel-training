# RHEL File & Directory Guide

## 1) โครงสร้าง File System ของ RHEL

RHEL ใช้โครงสร้างแบบ Hierarchical (ต้นไม้) โดยมี root คือ `/`

```
/ (Root Directory)
├── bin -> usr/bin          (คำสั่งพื้นฐานที่จำเป็นสำหรับ User ทุกคน)
├── boot                    (ไฟล์ที่ใช้ในการ Boot ระบบ เช่น Kernel, GRUB)
├── dev                     (ไฟล์อุปกรณ์ต่างๆ เช่น Disk, Keyboard, USB)
├── etc                     (ที่เก็บไฟล์ Configuration ของระบบและ Software)
├── home                    (Directory ส่วนตัวของ User ปกติ)
├── lib -> usr/lib          (Shared Libraries ที่จำเป็นสำหรับระบบ)
├── media                   (จุด Mount อุปกรณ์ถอดเสียบได้ เช่น USB, CD-ROM)
├── mnt                     (จุด Mount ชั่วคราวสำหรับ System Admin)
├── opt                     (ที่ติดตั้งซอฟต์แวร์เสริมจาก Third-party)
├── proc                    (Virtual Filesystem เก็บข้อมูลสถานะของ Process และ Kernel)
├── root                    (Home directory ของ User 'root' เท่านั้น)
├── run                     (ข้อมูลที่เกิดขึ้นระหว่างการรันระบบในปัจจุบัน)
├── sbin -> usr/sbin        (คำสั่งพื้นฐานสำหรับ System Admin เท่านั้น)
├── srv                     (ข้อมูลสำหรับ Service ที่ระบบให้บริการ เช่น เว็บไซต์)
├── sys                     (Virtual Filesystem เก็บข้อมูลเกี่ยวกับ Hardware)
├── tmp                     (ไฟล์ชั่วคราวที่จะถูกลบเมื่อ Restart หรือตามระยะเวลา)
├── usr                     (แหล่งรวมโปรแกรม, Libraries และเอกสารหลักของ User)
│   ├── bin                 (คำสั่งโปรแกรมทั่วไป)
│   ├── sbin                (คำสั่งโปรแกรมระดับระบบ)
│   └── local               (โปรแกรมที่ Admin ติดตั้งเองภายในเครื่อง)
└── var                     (ไฟล์ที่มีการเปลี่ยนแปลงบ่อย เช่น Log, Database, Mail)
    ├── log                 (ไฟล์บันทึกเหตุการณ์ต่างๆ ของระบบ)
    └── spool               (คิวงานต่างๆ เช่น งานพิมพ์ หรือ Email)
```

---

## 2) Directory สำคัญ

### `/`
- root ของทุกอย่าง

### `/etc`
- config ระบบทั้งหมด
- เช่น ssh, network

### `/var`
- ข้อมูลที่เปลี่ยนตลอด
- เช่น log, database

### `/home`
- user ปกติ

### `/root`
- home ของ root

### `/usr`
- program และ library

### `/tmp`
- temp file

### `/dev`
- device

### `/proc`
- kernel และ process info

---

## 3) File Types
สัญลักษณ์ต่างๆ ด้านหน้าของไฟล์/directory ที่ใช้แสดงและบอกถึงคุณสมบัติของไฟล์นั้นๆ แสดงดังนี้

| Symbol | Meaning |
|------|--------|
| - | file |
| d | directory |
| l | symlink |
| c | character device |
| b | block device |

---

## 4) คำสั่งพื้นฐาน

### ดูไฟล์
คำสั่งที่ใช้สำหรับแสดงรายการของไฟล์และ Directory ต่างๆ ในระบบของ Linux นั้นจะใช้คำสั่ง `ls` ตัวอย่างเช่น
```bash
ls
ls -l
ls -a
ls -lah
```

---

### เปลี่ยน directory
```bash
cd /etc
cd ~
cd ..
cd -
```

---

### ดู path ปัจจุบัน
```bash
pwd
```

---

### สร้าง directory
```bash
mkdir test
mkdir -p /tmp/a/b/c
```

---

### ลบ directory
```bash
rmdir test
rm -r test
rm -rf test
```

⚠️ ห้ามทำเด็ดขาด:
```bash
rm -rf /
```
เพราะคำสั่งนี้เป็นการลบทั้งระบบ

---

### สร้างไฟล์
```bash
touch file.txt
```

---

### ลบไฟล์
```bash
rm file.txt
```

---

### copy ไฟล์
```bash
cp file1 file2
cp -r dir1 dir2
```

---

### move / rename
```bash
mv file1 file2
mv file /tmp/
```

---

### ดูเนื้อหาไฟล์
การดูเนื้อหาของไฟล์สามารถใช้ได้หลายคำสั่ง ยกตัวอย่างเช่น
```bash
cat file
less file
more file
```

ตัวอย่างการดู log:
```bash
less /var/log/messages
```

---

### แก้ไขไฟล์
การแก้ไขไฟล์โดยปกติแล้วจะใช้ vi editor เป็นหลัก ซึ่งเป็นเครื่องมือที่มีอยู่ใน Linux ทุกตัวอยู่แล้ว แต่ยังมีอีกเครื่องมือเช่น nano ก็สามารถใช้งานสำหรับแก้ไขไฟล์ได้เช่นกัน
```bash
vi file
nano file
```

---

### ค้นหาไฟล์
โดยทั่วไปแล้วใน Linux เราสามารถค้นหาไฟล์ได้ด้วยคำสั่ง `find` ซึ่งสามารถใช้งานได้โดยไม่ต้องติดตั้ง package ใดๆ เพิ่ม

อีกคำสั่งคือคำสั่ง `locate` ซึ่งในบาง Linux อาจจะต้องติดตั้งเพิ่ม เนื่องจากไม่ได้ถูกติดตั้งมาโดย Default

ตัวอย่างการใช้คำสั่ง `find`
```bash
find / -name file.txt
find /var -type f
```
ตัวอย่างการใช้คำสั่ง `locate`
```bash
locate file.txt
```

---

### ดูขนาดไฟล์
```bash
du -sh *
df -h
```

---

## 5) Permission (สำคัญมาก)

```bash
ls -l
```

ตัวอย่าง:
```
-rw-r--r--
```

---

### เปลี่ยน permission
```bash
chmod 755 file
chmod +x script.sh
```

---

### เปลี่ยน owner
```bash
chown user:user file
chown root:root file
```

---

## 6) Symbolic Link

```bash
ln -s /etc/nginx/nginx.conf nginx.conf
```

---

## 7) ใช้งานจริง (Admin / Network)

### ดู log
```bash
cd /var/log/
less messages
```

---

### เช็ค disk
```bash
df -h
```

---

### หา config
```bash
find /etc -name "*network*"
```

---
