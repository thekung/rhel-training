# เฉลยแบบฝึกหัด: การติดตั้งและกำหนดค่า LAMP Stack บน RHEL

เอกสารฉบับนี้คือสรุปคำสั่งที่ถูกต้องสำหรับการทำ Lab

## 1. เฉลยขั้นตอนการปฏิบัติ (Lab Solutions)

1: ค้นหา Package
คำสั่ง:
```
dnf search mariadb-server
```
2: ติดตั้ง Package ทั้งหมด
คำสั่ง:
```
sudo dnf install httpd mariadb-server php php-mysqlnd -y
```

3 & 4: เริ่มทำงานและตั้งให้รันตอน Boot
คำสั่ง Apache:
```
sudo systemctl enable --now httpd
```
คำสั่ง MariaDB:
```
sudo systemctl enable --now mariadb
```
(**หมายเหตุ:** `--now` คือการสั่ง `start` และ `enable` พร้อมกัน)

5 ตรวจสอบสถานะ
คำสั่ง:
```
systemctl status httpd
```
คำสั่ง:
```
systemctl status mariadb
```

6 ตั้งค่า Firewall
คำสั่ง:
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

7 สร้างไฟล์ทดสอบ PHP
คำสั่ง:
```
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

8 การเพิ่มความปลอดภัยฐานข้อมูล
คำสั่ง:
```
sudo mysql_secure_installation
```

## เฉลยคำถามท้ายบท
1. หากต้องการลบ Package `httpd` ออกจากเครื่อง ต้องใช้คำสั่งใด?
```
sudo dnf remove httpd
```
2. ไฟล์ที่เก็บการตั้งค่า Repository ของ DNF อยู่ที่โฟลเดอร์ใด?
```
/etc/yum.repos.d/
```
3. ความแตกต่างระหว่าง `systemctl stop` และ `systemctl disable` คืออะไร?
```
Ans: ไม่มีผลต่อ Service ที่รันอยู่ปัจจุบัน (Service จะยังรันต่อจนกว่าจะสั่ง stop)
    แต่จะมีผลคือเมื่อมีการ Restart เครื่องใหม่ Service นั้นจะไม่รันขึ้นมาเอง
```

## ข้อแนะนำเพิ่มเติม (Tips)
- ตรวจสอบ SELinux ด้วยคำสั่ง 'sestatus' หากเข้าหน้าเว็บไม่ได้
- ตรวจสอบ Log การติดตั้งได้ที่ /var/log/dnf.log
- สามารถใช้คำสั่ง 'dnf history' เพื่อดูประวัติการทำรายการย้อนหลังได้
