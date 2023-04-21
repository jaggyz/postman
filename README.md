## ข้อตกลง Naming convertion ร่วมกัน

- Directory Name : ใช้ตัวอักษรพิมพ์เล็กทั้งหมด เช่น egat
- File Name : ใช้ Snake_Case เช่น create_success_project.robot
- Variable Name :
  - ชื่อตัวแปรเป็นคำเดียวให้ตั้งชื่อเป็นพิมพ์เล็กทั้งหมด เช่น ...
  - ชื่อตัวแปรมีความยาวตั้งแต่ 2 คำขึ้นไป ให้คำหลังขึ้นตันด้วยตัวอักษรตัวใหญ่เสมอ ในรูปแบบ camelCase เช่น ...
  - ชื่อตัวแปรเก็บค่าให้เติม "List" ต่อท้ายตัวแปรเสมอ เช่น ...
  - ชื่อตัวแปร Constant ให้ตังชื่อเป็นตัวอักษรพิมพ์ใหญ่ทั้งหมด เช่น URL

## ข้อตกลง Commit Message ร่วมกัน

- [Test] ใช้เพิ่ม/แก้ไข Test
- [Docs] การแก้ไขที่เกี่ยวกับเอกสาร เช่น แก้ README
- [Feat] Add new feature
- [Fix] Bug Fix
- [Refactor] ปรับโครงสร้าง
- [Build] แก้ไขกระบวนการสร้าง
- [Styles] เปลี่ยนคำใหม่แต่ความหมายยังคงเดิม
- [Perf] เพิ่มประสิทธิภาพในการทำงาน
- [Ci] การเปลี่ยนแปลงไฟล์การกำหนดค่า CI และ สคริปต์ เช่น ปรับขั้นตอนการทำงานของ GitHub
- [Chore] การเปลี่ยนแปลงอื่นๆที่ไม่เกี่ยวกับตัวไฟล์
- [Revert] คืนค่าคอมมิชชันก่อนหน้า

## ลิ้งก์ที่ใช้ในการทำงาน

- miroboard https://miro.com/app/board/uXjVOo6_NbY=/
- excel https://docs.google.com/spreadsheets/d/1PjcPTZkEIbDMXremaoapUdVm4pF18E-O6i2sU0sVL8Y/edit?usp=sharing
- seluenuim library https://robotframework.org/SeleniumLibrary/SeleniumLibrary.html

## Start Server by Docker

1. เปิด terminal ที่ `egat project`
2. พิมพ์

   ```sh
   cd ..
   ```

3. พิมพ์

   ```sh
   git clone git@bitbucket.org:opendream/landaims_api.git
   ```

   ```sh
   cd landaims_api
   git switch develop
   cd ..
   ```

4. พิมพ์

   ```sh
   git clone git@bitbucket.org:opendream/landaims_app.git
   ```

   ```sh
   cd landaims_app
   git switch develop
   cd ..
   ```

5. พิมพ์

   ```sh
   cd egat
   ```

6. landaims_api แก้ไฟล์ settings.py

   - add (at the be)

     ```python
     import os
     from pathlib import Path
     ...
     ```

   - replace

     ```python
     ALLOWED_HOSTS = []
     ```

   - with

     ```python
     # ALLOWED_HOSTS = []
     ALLOWED_HOSTS = os.environ.get("DJANGO_ALLOWED_HOSTS", "localhost").split(" ")
     ```

7. พิมพ์

   ```sh
   docker image build -f landaims_api/Dockerfile -t landaims_api ../landaims_api
   ```

8. landaims_app แก้ไฟล์ next.config.js

   - replace

     ```js
     async rewrites() {
      return [
         {
         source: '/landaims/api/:path*',
         destination: `http://localhost:10000/api/:path*/`, // Proxy to Backend
         },
      ]
     },
     ```

   - with

     ```js
     async rewrites() {
      const BACKEND_URL = process.env.BACKEND_URL || 'localhost:10000';
      return [
         {
         source: '/landaims/api/:path*',
         destination: `${BACKEND_URL}/api/:path*/`, // Proxy to Backend
         },
      ]
     },
     ```

9. landaims_app แก้ไฟล์ .env

   - add

     ```.env
     BACKEND_URL=http://localhost:10000
     ```

10. พิมพ์

   ```sh
   docker image build -f landaims_app/Dockerfile -t landaims_app ../landaims_app
   ```

11. พิมพ์

   ```sh
   cd landaims_app
   ```

12. start server โดยพิมพ์

   ```sh
   docker compose up -d
   ```

13. ทดสอบโดยเปิด `web browser` แล้วพิมพ์

    ```sh
    http://localhost:3000
    ```

14. stop server โดยพิมพ์

    ```sh
    docker compose down
    ```
