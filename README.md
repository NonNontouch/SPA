1. ติดตั้งระบบปฏิบัติการตระกูลยูนิกซ์ระดับเซิร์ฟเวอร์ Ubuntu Server หรือ FreeBSD/OpenBSD หรือเทียบเท่า

  

2. ตั้งค่าให้เวลาเดินตรงกับมาตรฐานโลก

```bash
sudo timedatectl set-timezone Asia/Bangkok
```

3. อัปเดต/แพตช์ระบบปฏิบัติการ

```bash
sudo apt update  
sudo apt upgrade -y
```

4. เพิ่มยูสเซอร์ webadmin และ dbadmin เข้าสู่ระบบ พร้อมการกำกับดูแลที่ดีให้ปลอดภัย

```bash
sudo adduser webadmin  
sudo adduser dbadmin

chmod -R 700 sysadmin
chmod -R 700 dbadmin
chmod -R 700 webadmin
```

5. เปิดให้เข้าถึงได้จากระยะไกลด้วย ssh หรือ remote desktop หรือเทียบเท่า

```bash
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb && 

sudo dpkg -i cloudflared.deb && 

sudo cloudflared service install eyJhIjoiNGRhN2JkNzY4ZGQ3NGJkYWM3YjUyMjQ5OGI3MDZhZjUiLCJ0IjoiZWVhZGQ2Y2UtNzQwNC00OWU3LTg4NWMtMTJjYjRiOGNlYTNlIiwicyI6Ik9EVXlOR1l3WTJFdE9UUXpZeTAwWW1VMUxXRTFOV1l0WWpZMFpHUTBZVEpqWW1SayJ9

ssh-keygen -t rsa -f ~/.ssh/ida_spa

vim ~/.ssh/config
```

```
Host ssh.example.com
  ProxyCommand /usr/local/bin/cloudflared access ssh --hostname %h
```

```bash
ssh <username>@ssh.example.com
```

6. เปิดบริการเป็นเว็บแอพพลิเคชันได้ (Web+DB+Server-Side Script) แบบปลอดภัย

```bash
sudo docker compose up -d
```

7. กำกับดูแลด้วยยูสเซอร์ webadmin สำหรับงานเว็บ และ dbadmin สำหรับงานฐานข้อมูล


8. คำสั่งที่จำเป็นต่อผู้ดูแลระบบไม่ต่ำกว่า 20 คำสั่ง
