# System Platform Administration - Ubuntu Server Guide

## 1. ติดตั้งระบบปฏิบัติการตระกูลยูนิกซ์ระดับเซิร์ฟเวอร์ Ubuntu Server หรือ FreeBSD/OpenBSD หรือเทียบเท่า

ไปที่ https://ubuntu.com/download/server เพื่อทำการดาวโหลด Ubuntu Server 20.04 LTS เมื่อทำการดาวโหลดเสร็จเเล้วทำการเปิด Virtual Box ไปที่เเถบ new เเล้วตั้งชื่อ ไฟล์เป็น Ubuntu Server 20.04 LTS เลือก Type เป็น Linux เเละ Version เป็น Ubantu(64bit) ทำการกดเเท็บต่อไปเรื่อยๆ จนสร้างเสร็จ เมื่อทำการสร้างเสร็จเเล้วทำการตั้งค่า โดยไปที่เเท็บ Network -> Attached to -> Bridged Adapter -> ok ทำการกดเเท็บ Start เพื่อเริ่มการใช้งาน รอระบบเริ่มจนเห็นหน้าต่างที่จะให้ทำการเลือกภาษาที่ต้องการ เลือก Layout หลักจากนั้นเลือก Network connection (เเนะนำให้เลือกเป็นค่าเริ่มต้นที่ระบบให้มา) หลังจากนั้นระบบจะให้ทำการตั้งค่า Proxy address ในตอนนี้เราไม่ต้องกำหนดค่าใดๆ ให้ทำการข้ามไปเลย หลังจากนั้นระบบจะให้ทำการตั้งค่า archive mirror เราก็ทำการใช้ค่าที่ระบบให้มาเลย ต่อไปเป็นการตั้งค่า disk storage ให้เพิ่ม Set up this disk as an LVM group ไปด้วยต่อไปเป็นการตั้งค่า Storage ให้ใช้ค่าที่ระบบให้มาได้เลย  ต่อไปเป้นการตั้งค่า profile สามารถตั้งค่าเป็นอะไรก็ได้ เเต่เราขอเเนะนำให้ตั้ง password ให้มีความซับซ้อนเเละยากต่อการคาดเดาเมื่อกดเเท็บ done ไปเรื่อยๆ จนถึงหน้า SSH setup ให้ทำการเลือก install SSH server ทำการกด done เเละรอจนระบบทำการดาวน์โหลดจนเสร็จเเล้วกดไปที่เเท็บ reboot now ได้เลยเเละทำการรอการrebootจยเสร็จทำการlogin ระบบด้วย username เเละ password ที่ตั้งไว้ 
  
## 2. ตั้งค่าให้เวลาเดินตรงกับมาตรฐานโลก

ทำการเข้าสู่ระบบ server เมื่อเข้าสู่ระบบเเล้ว ให้ทำการเช็คเวลาของระบบ โดย ใช้คำสั่ง `sudo timedatectl` เมื่อทำเเล้วจะเห็น time zone ขึ้นมา ในที่นี้เราต้องการเปลี่ยน time zone ให้เป็นประเทศไทยเราจึงทำการเปลี่ยนได้โดยใช้คำสั่ง

```bash
sudo timedatectl set-timezone Asia/Bangkok
```

เมื่อใช้คำสั่งนี้ระบบจะให้ทำการใส่รหัสผ่านของผู้ใช้ เมื่อถูกต้องระบบจะบอกว่า  AUTHENTICATION COMPLETE หลังจากนั้นให้เราทำการเช็ค time zone อีกที่ว่าระบบได้ใช้ time zone ที่เราต้องการหรือไม่

ต่อไปเป้นการตั้งค่า NTP server โดยในตอนเเรกเราต้องทำการเช็ค NTP server ในระบบของเราก่อนโดยใช้คำสั่ง `sudo systemctl status systemd-timesyncd.service` หลังจากนั้นทำการใส่รหัสของผู้ใช้ จะเห็นหน้า NTP server ออกมา จะเห็น ntp.ubantu.com ซึ่งเราจะทำการเปลี่ยนให้เป็น NTP server ของไทยโดยเราจะทำการเปลี่ยนโดยไปที่หน้า config โดยใช้คำสั่ง `sudo nano /etc/systemd/timesyncd.conf` เมื่อเข้าไปที่หน้า config เเล้วให้ทำการเเก้ไขโดยไปที่ NTP เเล้วทำการใส่ NTP server ที่เปิดในประเทศไทยลงไปเช่น time.navy.mi.th, clock.nectec.or.th, ntp.ku.ac.th หรือ time.uni.net.th ซึ่งสามารถหาได้จากใน google (ถ้า NTP server เหล่านี้ใช้ไม่ได้ระบบ จะทำการใช้ NTP server สำรองของระบบเเทน) หลังจากนั้นทำการเริ่มต้นระบบใหม่โดยใช้คำสั่ง `sudo systemctl restart systemd-timesyncd.service` เเละเมื่อต้องการเช็คว่า NTP server ที่เราใช้สามารถใช้ได้หรือไม่ ให้ทำการใช้คำสั่ง `sudo systemctl status systemd-timesyncd.service`

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
sudo systemctl enable unattended-upgrades
sudo systemctl status unattended-upgrades
```

## 4. เพิ่มยูสเซอร์ webadmin และ dbadmin เข้าสู่ระบบ พร้อมการกำกับดูแลที่ดีให้ปลอดภัย

การเพิ่ม user นั่นสามารถทำได้โดยการใช้คำสั่ง `sudo adduser (name)`

```bash
sudo adduser webadmin  
sudo adduser dbadmin

chmod -R 750 sysadmin
chmod -R 750 dbadmin
chmod -R 750 webadmin
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
git clone https://github.com/nkd3v/chat-app.git
```

```bash
sudo apt install docker.io -y
```

```bash
docker network create snappy-network
```

```bash
docker run -d \
  -e MONGO_INITDB_ROOT_USERNAME=dbadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=cq7p8N9qeMgKq3B2WuUp \
  -v /home/dbadmin/db:/data/db \
  --name mongo \
  --network snappy-network \
  mongo

docker ps

docker exec -it c74 bash

mongosh --authenticationDatabase admin -u dbadmin -p
```

```mysql
use chat
db.createUser(
  {
    user: "webadmin",
    pwd:  passwordPrompt(),
    roles: [ { role: "readWrite", db: "chat" } ]
  }
)

p5jy8gyXUQq3ap7jXKWP
```

```dockerfile
FROM node:16

WORKDIR /app

COPY package*.json ./

RUN npm install

EXPOSE 5000

CMD [ "npm", "start" ]
```

```bash
docker build -t snappy-app .

docker run -dp 3000:3000 --name snappy-app --network snappy-network -v $PWD:/app -v app-deps:/app/node_modules snappy-app

docker build -t snappy-api .

docker run -dp 5000:5000 --name snappy-api --network snappy-network  -v $PWD:/app -v api-deps:/app/node_modules snappy-api
```

```bash
mongodb://webadmin:p5jy8gyXUQq3ap7jXKWP@mongo:27017/chat?authSource=chat
```

```bash
curl localhost:3000
```

## 7. กำกับดูแลด้วยยูสเซอร์ webadmin สำหรับงานเว็บ และ dbadmin สำหรับงานฐานข้อมูล

```bash
# danger (rootful container)
sudo usermod -aG docker sysadmin

# safe (rootless container)
sudo docker rm -f `docker ps -aq`
sudo systemctl disable --now docker.service docker.socket

curl -fsSL https://get.docker.com/rootless | sh

ip link add snappy-network type dummy
ip addr add 172.20.0.2/16 dev snappy-network
ip addr show

su dbadmin

sudo chown -R dbadmin:dbadmin /home/dbadmin/db

docker run -dp 172.20.0.2:27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=dbadmin \
  -e MONGO_INITDB_ROOT_PASSWORD=cq7p8N9qeMgKq3B2WuUp \
  -v /home/dbadmin/db:/data/db \
  --name mongo \
  mongo

vi /etc/hosts
mongo 172.20.0.2

su webadmin

docker build -t snappy-app .
docker run -dp 3000:3000 --name snappy-app -v $PWD:/app -v app-deps:/app/node_modules snappy-app
docker build -t snappy-api .
docker run -dp 5000:5000 --name snappy-api -v $PWD:/app -v api-deps:/app/node_modules snappy-api
```

## 8. คำสั่งที่จำเป็นต่อผู้ดูแลระบบไม่ต่ำกว่า 20 คำสั่ง

Networking
```bash
ip a
sudo ss -ntulp
whois snappy.pp.ua
dig app.snappy.pp.ua
curl -Iv https://app.snappy.pp.ua
curl localhost:3000
curl ipinfo.io
```

Maintenance
```bash
df -ah
du -sh *
sudo shutdown now
sudo shutdown -r now
vi backup-db.sh
chmod +x backup-db.sh
mkdir /home/sysadmin/backup
```

Backup
```bash
#!/bin/bash

backup_files="/home/dbadmin/db"

dest="/home/sysadmin/backup"

date=$(date +%d-%m-%Y)
archive_file="$date.tgz"

tar czf $dest/$archive_file $backup_files

# sudo crontab -e
# 0 0 * * * /home/sysadmin/backup-db.sh
```

Essential
```bash
ps aux
htop
history
kill
w
# make file then
rm
shred -vfuz -n 10 file
man man
alias dr='docker run'
```
