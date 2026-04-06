# RHEL Administration: Package Management and Services

เอกสารฉบับนี้สรุปการบริหารจัดการซอฟต์แวร์ (Packages) และการควบคุมการทำงานของระบบ (Services) บน Red Hat Enterprise Linux (RHEL)

---

## 1. การบริหารจัดการ Package (DNF / YUM)

ใน RHEL เวอร์ชันปัจจุบัน เครื่องมือหลักคือ **DNF (Dandified YUM)** ซึ่งทำงานบนพื้นฐานของไฟล์นามสกุล `.rpm` โดยทำหน้าที่จัดการการติดตั้ง ลบ และแก้ไขปัญหา Dependencies ของซอฟต์แวร์โดยอัตโนมัติ



### คำสั่ง DNF ที่จำเป็นต้องรู้

| การทำงาน | คำสั่ง |
| :--- | :--- |
| **ติดตั้ง Package** | `sudo dnf install <package_name>` |
| **ลบ Package** | `sudo dnf remove <package_name>` |
| **อัปเดต Package ทั้งหมด** | `sudo dnf update` |
| **ค้นหา Package ในคลัง** | `dnf search <keyword>` |
| **ดูรายละเอียดของ Package** | `dnf info <package_name>` |
| **ตรวจสอบรายการ Repository** | `dnf repolist` |
| **ล้างไฟล์ Cache ในเครื่อง** | `dnf clean all` |
| **ตรวจสอบประวัติการติดตั้ง** | `dnf history` |

> **หมายเหตุ:** เรายังสามารถใช้คำสั่ง `yum` แทน `dnf` ได้ (เป็น Alias ที่ระบบสร้าง symbolic link เอาไว้ให้) เพื่อความสะดวกสำหรับผู้ที่คุ้นเคยกับเวอร์ชันเก่า

---

## 2. การบริหารจัดการ Services (systemd)

RHEL ใช้ **systemd** เป็นตัวจัดการระบบและเซอร์วิส (Service Manager) โดยมีหน่วยพื้นฐานที่เรียกว่า "Unit" และใช้คำสั่ง `systemctl` ในการควบคุมกระบวนการต่างๆ



### คำสั่ง systemctl ที่จำเป็นต้องรู้

| การทำงาน | คำสั่ง |
| :--- | :--- |
| **เริ่มทำงาน Service** | `sudo systemctl start <service_name>` |
| **หยุดทำงาน Service** | `sudo systemctl stop <service_name>` |
| **เริ่มทำงานใหม่ (Restart)** | `sudo systemctl restart <service_name>` |
| **โหลด Configuration ใหม่** | `sudo systemctl reload <service_name>` |
| **ตรวจสอบสถานะปัจจุบัน** | `systemctl status <service_name>` |
| **ตั้งให้เริ่มทำงานตอน Boot** | `sudo systemctl enable <service_name>` |
| **ยกเลิกการเริ่มตอน Boot** | `sudo systemctl disable <service_name>` |
| **ตรวจสอบว่าเปิดใช้งานตอน Boot หรือไม่** | `systemctl is-enabled <service_name>` |

---

## 3. ขั้นตอนการทำงานจริง (Practical Example)

เมื่อต้องการติดตั้งและเปิดใช้งานบริการใหม่ (ตัวอย่างเช่น Apache Web Server) ให้ทำตามลำดับดังนี้:

1. **ติดตั้งโปรแกรม:**
```bash
sudo dnf install httpd
```
2. เปิดการทำงานและตั้งให้รันตอน Boot (ใช้ --now เพื่อรันทันที):

```bash
sudo systemctl enable --now httpd
```
3. ตรวจสอบสถานะ:
```bash
systemctl status httpd
```
4. ตำแหน่งไฟล์ที่สำคัญ
-  `/etc/yum.repos.d/`: เก็บไฟล์การตั้งค่า Repository (.repo)
-  `/var/log/dnf.log`: ล็อกไฟล์สำหรับการติดตั้ง/อัปเดต Package
-  `/usr/lib/systemd/system/`: เก็บไฟล์ Unit ของ Service ที่มากับระบบ
-  `/etc/systemd/system/`: เก็บไฟล์ Unit ที่ User สร้างขึ้นหรือแก้ไขเอง