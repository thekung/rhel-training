## 💡 เฉลยคำสั่ง (Cheat Sheet สำหรับตรวจสอบ)

| ข้อที่ | ชุดคำสั่งที่ใช้ |
| :--- | :--- |
| **1** | `sudo groupadd devops` <br> `sudo useradd -g devops -c "DevOps Engineer" alice` <br> `echo "P@ssw0rd123" \| sudo passwd --stdin alice` |
| **2** | `sudo mkdir /opt/devops_data` <br> `sudo chgrp devops /opt/devops_data` <br> `sudo chmod 770 /opt/devops_data` <br> `sudo useradd -G devops bob` |
| **3** | `sudo useradd -M -s /sbin/nologin app_service` <br> `sudo passwd -e alice` <br> `sudo chage -M 30 alice` <br> `sudo usermod -L bob` |
| **4** | `sudo userdel -r alice` <br> `sudo groupdel devops` |