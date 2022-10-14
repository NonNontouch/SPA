# System Platform Administration - Ubuntu Server Guide

## 1. ติดตั้งระบบปฏิบัติการตระกูลยูนิกซ์ระดับเซิร์ฟเวอร์ Ubuntu Server หรือ FreeBSD/OpenBSD หรือเทียบเท่า

  

## 2. ตั้งค่าให้เวลาเดินตรงกับมาตรฐานโลก

```bash
sudo timedatectl set-timezone Asia/Bangkok
```

## 3. อัปเดต/แพตช์ระบบปฏิบัติการ

อัปเดตข้อมูลเวอร์ชันของแพคเกจและดาวน์โหลดแพคเกจเวอร์ชันใหม่มาติดตั้ง
คำสั่งนี้จะแสดง update list ของ package โดยจะตรวจสอบการอัปเดตผ่านทาง internet โดย list จะถูกนำมาจาก /etc/apt/source.list
hit คือเป็นversionล่าสุดแล้ว 
get คือสามารถอัปเดตได้
หรือสามารถใช้คำสั่ง `apt list --upgradable` เพื่อดูว่าpackageไหนupdateได้  

```bash
sudo apt-get update
```

คำสั่งนี้จะ install package ทั้งหมดที่สามารถ update ได้
หากต้องการ install package ที่ต้องการเท่านั้นสามารถใช้คำสั่ง `sudo apt --only-upgrade install (package name)`

```bash
sudo apt-get upgrade
```

เปิดใช้งานการอัปเดตอัตโนมัติด้านความปลอดภัย

```bash
sudo systemctl status unattended-upgrades
sudo systemctl enable unattended-upgrades
sudo systemctl status unattended-upgrades
```

## 4. เพิ่มยูสเซอร์ webadmin และ dbadmin เข้าสู่ระบบ พร้อมการกำกับดูแลที่ดีให้ปลอดภัย

การเพิ่ม user นั่นสามารถทำได้โดยการใช้คำสั่ง `sudo adduser (name)`

```bash
sudo adduser webadmin  
sudo adduser dbadmin

chmod -R 700 sysadmin
chmod -R 700 dbadmin
chmod -R 700 webadmin
```

## 5. เปิดให้เข้าถึงได้จากระยะไกลด้วย ssh หรือ remote desktop หรือเทียบเท่า

```bash
sudo ufw allow 22
sudo ufw limit 22
sudo ufw enable
```

```bash
sudo ufw show added
sudo ufw app list
sudo ufw status numbered
```

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_sysadmin
ssh-copy-id -i ~/.ssh/id_sysadmin sysadmin@<server-ip>
```

```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && 

sudo dpkg -i cloudflared.deb && 

sudo cloudflared service install <token>
```

```bash
sudo nano /etc/ssh/sshd_config
```

```
PermitRootLogin no
PasswordAuthentication no
```

```bash
sudo nano ~/.ssh/config
```

```
Host ssh.example.com
  ProxyCommand /usr/local/bin/cloudflared access ssh --hostname %h
```

```bash
ssh <username>@<server-ip>
```

## 6. เปิดบริการเป็นเว็บแอพพลิเคชันได้ (Web+DB+Server-Side Script) แบบปลอดภัย

```bash
sudo apt install docker.io -y
sudo apt install docker-compose -y

sudo docker-compose up -d
```

## 7. กำกับดูแลด้วยยูสเซอร์ webadmin สำหรับงานเว็บ และ dbadmin สำหรับงานฐานข้อมูล

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_webadmin
ssh-copy-id -i ~/.ssh/id_webadmin webadmin@<server-ip>

ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_dbadmin
ssh-copy-id -i ~/.ssh/id_dbadmin dbadmin@<server-ip>
```

```
Host spa-web
    Hostname <hostname>
    User webadmin
    IdentityFile ~/.ssh/id_webadmin
```

## 8. คำสั่งที่จำเป็นต่อผู้ดูแลระบบไม่ต่ำกว่า 20 คำสั่ง
