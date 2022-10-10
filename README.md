## ติดตั้งระบบปฏิบัติการตระกูลยูนิกซ์ระดับเซิร์ฟเวอร์ Ubuntu Server หรือ FreeBSD/OpenBSD หรือเทียบเท่า

  

## ตั้งค่าให้เวลาเดินตรงกับมาตรฐานโลก

```bash
sudo timedatectl set-timezone Asia/Bangkok
```

## อัปเดต/แพตช์ระบบปฏิบัติการ

```bash
sudo apt update  
sudo apt upgrade -y
```

## เพิ่มยูสเซอร์ webadmin และ dbadmin เข้าสู่ระบบ พร้อมการกำกับดูแลที่ดีให้ปลอดภัย

```bash
sudo adduser webadmin  
sudo adduser dbadmin
```

## เปิดให้เข้าถึงได้จากระยะไกลด้วย ssh หรือ remote desktop หรือเทียบเท่า

## เปิดบริการเป็นเว็บแอพพลิเคชันได้ (Web+DB+Server-Side Script) แบบปลอดภัย

```bash
docker volume create portainer_data  
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \  
    --restart=always \  
    -v /var/run/docker.sock:/var/run/docker.sock \  
    -v portainer_data:/data \  
    portainer/portainer-ce:2.9.3
```

## กำกับดูแลด้วยยูสเซอร์ webadmin สำหรับงานเว็บ และ dbadmin สำหรับงานฐานข้อมูล

## คำสั่งที่จำเป็นต่อผู้ดูแลระบบไม่ต่ำกว่า 20 คำสั่ง
