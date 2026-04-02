# Must have configuration after installed

หลังจากติดตั้ง Linux มีสิ่งที่ควรตั้งค่าเบื้องต้นดังนี้

Task 1: ตรวจสอบการทำงานของ SElinux ด้วยคำสั่ง `getenforce`

Task 2: หากมี RedHat Subscription ให้ทำการ register subscription เพื่อให้ RHEL สามารถใช้งาน Repository ในการติดตั้ง Software ต่างๆ เพิ่มเติมได้

Task 3: หากไม่มี RedHat Subscription ให้ทำการสร้าง Local Repository เพื่อใช้ติดตั้ง Software

Task 4: ติดตั้ง Software พื้นฐานคือ `curl`, `wget`, `rsync`, `net-tools`

Task 5: ติดตั้ง Software เพิ่มเติมตามที่ต้องการ หรือตาม Requirement

Task 6: กำหนด boot target mode ให้เป็น multi-target