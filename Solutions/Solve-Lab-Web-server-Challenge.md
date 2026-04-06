# เฉลย LAB: Web server challenge
## ขั้นตอนการทำ
Task 1: สร้าง User ใหม่ชื่อ `webadmin` และให้สิทธิ์ `sudo`
```bash
sudo useradd -r -s /sbin/nologin webadmin
```

Task 2: ติดตั้งแพ็กเกจ `httpd (Apache)` และสั่งให้ service เริ่มทำงาน
```bash
sudo dnf install -y httpd
sudo systemctl enable --now httpd
```

Task 3: สร้างไฟล์ `index.html` ในโฟลเดอร์ `/var/www/html/` โดยใช้สิทธิ์ของ `webadmin`
```bash
echo "Welcome to RedHat Lab" | sudo tee /var/www/html/index.html
```

Task 4: ปรับ Permission ของไฟล์ให้เหมาะสม (644)
```bash
sudo chmod 644 /var/www/html/index.html
```

Task 5: จัดการ Firewall เพื่ออนุญาตให้ Traffic ของพอร์ต 80 ผ่านได้
```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
```

Task 6: ทดสอบเข้าถึงหน้าเว็บผ่าน IP ของเครื่อง Lab
```bash
curl http://<webserver-ip>
```