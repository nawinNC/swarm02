# Ref
- [https://github.com/docker/awesome-compose/tree/master/gitea-postgres]

### wakatime swarm02 link
- [https://wakatime.com/projects/swarm02?start=2023-02-28&end=2023-03-06] 


## สร้าง VM template / Create VM template
  * Os Ubuntu 22.04
  * Cpu 2 core
  * Memory 2024 MB
  * Storege 32 GB
#### หลังจากสร้าง VM / After create VM
 * ตั้งเวลาไทม์โซน / Set timezone Bangkok Thailand
``` 
 timedatectl set-timezone Asia/Bangkok
``` 
 * ติดตั้ง Docker / Install docker engine for Ubuntu
 ``` 
 apt update ; apt upgrade -y
apt-get install \
    ca-certificates \
    curl wget \
    gnupg \
    lsb-release -y

mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null

apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
``` 
  * อัปเดต sevices หลังติดตั้ง Docker / Update services after install Docker 
``` 
apt update
apt upgrade -y
``` 
  * reboot node
``` 
reboot
``` 
### ตั้งค่า config / Run this code for set VM config to clone
```
cp /dev/null /etc/machine-id
rm /var/lib/dbus/machine-id
ln -s /etc/machine-id /var/lib/dbus/machine-id
init 0
```
### Run this code for docker authorited
```
sudo usermod -aG docker $USER
docker ps
```
 
## เตรียม node Ubuntu 22.04 ที่โคลนจาก template / Prepare node Ubuntu 22.04 clon from template
  * Node1 manager node 
  * Node2 work1
  * Node3 work2
 
### หลังจากโคลน / After clone node
 * Use sudo mode for set time and hostname (all three node) / ใช้ sudo mode เพื่อตั้งเวลา และ hostname
``` 
 sudo -i 
``` 
 * ตั้ง hostname สำหรับในแต่ละโหนด / Set hostname for in each node
   * Node 1 set hostname to "manager"
   * Node 2 set hostname to "work01"
   * Node 3 set hostname to "work2"
``` 
 hostnamectl set-hostname “Change-me”
``` 
## ใน manager node 
#### สามารถเรียกใช้คำสั่งนี้เพื่อดูโหนด ip สำหรับ remote ssh จาก VS code (manager) และ VS code ต้องใช้ส่วนขยาย ssh remote / Can run this command to see node ip for remote ssh with VS code (manager node) and Vs code requires extension ssh remote 
``` 
 ip a
```
#### หลังจาก เชื่อม ssh remote กับ vs code ให้ติดตั้งส่วนขยาย ดังนี้ / After ssh remote with vs code. Install this extensions
  * Docker
  * Git hub
  * Wakatime and enter api key
#### ใช้คำสั่งนี้เพื่อสร้าง token / Run this command to gennarate token for run on work node
``` 
 docker swarm init
```
  * And copy token for run on work node
#### หลังจากใช้ token บน work node แล้วใช้คำสั่งนี้เพื่อเช็คสมาชิก node / After enter token on work node can run this code for check member node
``` 
 docker node ls
```
#### ปรับใช้ portainer สำหรับ swarm / Deploy portainer for swarm 
``` 
 curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
docker stack deploy -c portainer-agent-stack.yml portainer
```
  * go to this port https://localhost:9443
## In work node ( work1,work2 )
  * Can switch manager node to work node by this code ( on Vs code )
``` 
 ssh target_ip_local
```
  * Paste token on work node (all two node)

## Switch to manager mode agian 
  * Create file git config
``` 
 git config --global user.name "github_username"
 git config --global user.email "github_useremail"
```
  * Create folder swarm01 and swarm02
``` 
mrkdir swarm01
mrkdir swarm02
```
  * Git init in swarm01 and swarm02
``` 
Git init
```
  * ไปที่ swarm02 เพื่อเลือก application potainer / Go to swarm02 for choose application potainer frome (https://github.com/docker/awesome-compose/tree/master/portainer)
  * สร้าง README.md และ compose.yaml / Create README.md and compose.yaml file
  * เรียกใช้คำสั่งนี้เพื่อส่งไฟล์ / Run this command to commit file
``` 
git commit -m "openProject"
```
  * Run this command for push to github
``` 
git push origin master
```

## ยังไม่เสร็จสมบูรณ์นะครับ
