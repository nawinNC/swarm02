## อธิบาย Compose.yaml
#### ส่วนของ services ชื่อ " gitea "

**กำหนด images ที่ gitea จะใช้งานโดย images ที่ใช้งาน ชื่อ " gitea/gitea:latest "**

- image: gitea/gitea:latest
  
**กำหนด environment ในส่วนของ DB_TYPE, DB_HOST, DB_NAME, DB_USER, DB_PASSWD ตามที่เราต้องการ**
- environment: <br>
      - DB_TYPE=postgres<br>
      - DB_HOST=db:5432<br>
      - DB_NAME=gitea<br>
      - DB_USER=gitea<br>
      - DB_PASSWD=gitea<br>

**กำหนดให้ service นั้น restart ตัวเองอัตโนมัติเมื่อมีข้อผิดพลาด หรือสั่งให้เริ่มต้นทำงานอัตโนมัติเมื่อเปิดเครื่องขึ้นมาใหม่**
- restart: always

**เชื่อม volume นี้กับ service โดยใช้**
- volumes:<br>
      - git_data:/data

**กำหนด port ให้เป็น 3000:3000**
- ports:<br>
      - 3000:3000

#### ส่วนของ services ชื่อ " db "

**กำหนด images ที่ gitea จะใช้งานโดย images ที่ใช้งาน ชื่อ " postgres:alpine "**

- image: postgres:alpine

**กำหนด environment ในส่วนของ POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_DB ตามที่เราต้องการ**

- environment:<br>
      - POSTGRES_USER=gitea<br>
      - POSTGRES_PASSWORD=gitea<br>
      - POSTGRES_DB=gitea<br>

**กำหนดให้ service นั้น restart ตัวเองอัตโนมัติเมื่อมีข้อผิดพลาด หรือสั่งให้เริ่มต้นทำงานอัตโนมัติเมื่อเปิดเครื่องขึ้นมาใหม่**
- restart: always

**เชื่อม volume นี้กับ service โดยใช้**
- volumes:<br>
      - db_data:/var/lib/postgresql/data

**กำหนด expose ให้ใช้ port : " 5432 "**
- expose:<br>
      - 5432

#### สร้าง  volume ที่มีชื่อ db_data กับ git_data

- volumes:<br>
  db_data:<br>
  git_data:
