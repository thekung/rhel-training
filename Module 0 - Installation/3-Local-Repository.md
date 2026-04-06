# Create Local Repository
ในระบบของ RHEL หากไม่ Register Subscription จะไม่สามารถติดตั้ง Software ใดๆ เพิ่มเติมจาก Repository ของ RedHat ได้

แต่เรายังสามารถสร้าง Repository ของตัวเองขึ้นมาเพื่อใช้งานได้

## Repository Type
- Local files
- Ftp protocol
- http / https protocol

## How to create Local Repository
Concept ของการสร้าง Local Repository คือการนำข้อมูลในแผ่นติดตั้งของ RHEL (Full DVD) มาใช้งานเป็นคลัง Software แทนการเรียกใช้จากภายนอก

ใน LAB นี้จะเป็นการสร้าง Repository ด้วยการใช้งานแบบ Local Files นึกถึงความเป็นจริง เราติดตั้ง RHEL เสร็จ และไม่ได้ subscription เราจะไม่สามารถติดตั้ง ftp หรือ http เพื่อสร้างเป็น Repository ได้อยู่แล้ว ดังนั้นจึงเหลือทางเลือดเดียวคือใช้งานแบบ Local Files

### Step 1
Mount แผ่น DVD เพื่อให้ใช้งานได้
```bash
mount /dev/sr0	/mnt
```
หลังจาก mount เรียบร้อยแล้วจะสามารถเข้าถึงไฟล์ต่างๆ ใน DVD นั้นได้จาก `/mnt`
```bash
root@rhel01:~# ls -l /mnt
total 69
drwxr-xr-x 1 root root  2048 Oct 21 13:26 AppStream
drwxr-xr-x 1 root root  2048 Oct 21 13:26 BaseOS
drwxr-xr-x 1 root root  2048 Oct 21 13:14 boot
drwxr-xr-x 1 root root  2048 Oct 21 13:14 EFI
-r--r--r-- 1 root root  8154 Oct 21 13:08 EULA
-r--r--r-- 1 root root  7861 Oct 21 13:26 extra_files.json
drwxr-xr-x 1 root root  2048 Oct 21 13:26 Flatpaks
-r--r--r-- 1 root root 18092 Oct 21 13:08 GPL
drwxr-xr-x 1 root root  2048 Oct 21 13:14 images
-r--r--r-- 1 root root   102 Oct 21 13:26 media.repo
-r--r--r-- 1 root root  1670 Oct 21 13:08 RPM-GPG-KEY-redhat-beta
-r--r--r-- 1 root root 20700 Oct 21 13:08 RPM-GPG-KEY-redhat-release
root@rhel01:~#
```

### Step 2
ต่อไปคือสร้างไฟล์ `.repo` เพื่ออ้างอิงให้ระบบมองเห็นไฟล์และเรียกใช้จาก DVD เมื่อมีการสั่งติดตั้ง Software เกิดขึ้น

สร้างไฟล์ `/etc/yum.repos.d/local.repo`

```bash
vi /etc/yum.repos.d/local.repo
```
เพิ่มข้อมูลดังนี้
```bash
[Local_Base]
name=Local Base Local Repository
baseurl=file:///mnt/BaseOS
gpgcheck=0
enabled=1

[Local_AppStream]
name=Loal AppStream Local Repository
baseurl=file:///mnt/BaseOS
gpgcheck=0
enabled=1
```
### Step 3
สั่งโหลดข้อมูลใหม่ด้วยคำสั่ง
```bash
dnf repolist all
```
จะเห็นว่ามี Repository ที่สามารถใช้งานได้เกิดขึ้นแล้ว
```bash
root@rhel01:~# dnf repolist all
repo id                   repo name                               status
Local_AppStream           Loal AppStream Local Repository         enabled
Local_Base                Local Base Local Repository             enabled
root@rhel01:~#
```
### Step 4
ทดลองค้นหา httpd software จากคำสั่ง
```bash
dnf search httpd
```
จะได้ลักษณะนี้ แสดงว่าค้นหาเจอ httpd ดังนั้นเราจะสามารถติดตั้ง httpd เพื่อนำมาใช้งานได้
```bash
root@rhel01:~# dnf search httpd
Last metadata expiration check: 0:03:52 ago on Fri 03 Apr 2026 03:27:09 PM +07.
====================== Name Exactly Matched: httpd =======================
httpd.x86_64 : Apache HTTP Server
===================== Name & Summary Matched: httpd ======================
httpd-core.x86_64 : httpd minimal core
redhat-logos-httpd.noarch : Red Hat-related icons and pictures used by
                          : httpd
========================== Name Matched: httpd ===========================
httpd-filesystem.noarch : The basic directory layout for the Apache HTTP
                        : Server
httpd-tools.x86_64 : Tools for use with the Apache HTTP Server
root@rhel01:~#
```