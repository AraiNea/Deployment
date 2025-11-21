คู่มือการ Deploy โปรเจ็กต์ด้วย GitHub Actions Runner (Self-hosted)

คู่มือนี้จะช่วยให้คุณตั้งค่า GitHub Actions Runner บนเครื่องของตัวเองเพื่อใช้ในการ deploy โปรเจ็กต์โดยใช้ self-hosted runner

ขั้นตอนที่ 1: ดาวน์โหลดและติดตั้ง GitHub Actions Runner

สร้างโฟลเดอร์สำหรับ GitHub Actions Runner:
เปิดเทอร์มินัลและรันคำสั่งนี้เพื่อสร้างโฟลเดอร์ใหม่สำหรับเก็บ GitHub Actions Runner:

mkdir actions-runner && cd actions-runner


ดาวน์โหลดไฟล์ติดตั้ง GitHub Actions Runner:
ดาวน์โหลดไฟล์จาก GitHub โดยใช้คำสั่งนี้:

curl -o actions-runner-linux-x64-2.329.0.tar.gz -L https://github.com/actions/runner/releases/download/v2.329.0/actions-runner-linux-x64-2.329.0.tar.gz


(Optional) ตรวจสอบ Hash ของไฟล์ที่ดาวน์โหลด:
คำสั่งนี้ช่วยให้คุณตรวจสอบว่าไฟล์ที่ดาวน์โหลดมาถูกต้องหรือไม่:

echo "194f1e4bd028f80b7e963cf5406848d84e19f3928a324d512ea53430102e1d actions-runner-linux-x64-2.329.0.tar.gz" | shasum -a 256 -c


แตกไฟล์ที่ดาวน์โหลด:
รันคำสั่งนี้เพื่อแตกไฟล์ติดตั้ง:

tar xzf ./actions-runner-linux-x64-2.329.0.tar.gz

ขั้นตอนที่ 2: ตั้งค่า GitHub Actions Runner

ใช้คำสั่งในการตั้งค่า GitHub Actions Runner:
คุณต้องไปที่หน้า Settings ของ repository บน GitHub และสร้าง GitHub Runner Token จากหน้า Actions > Runners สำหรับการตั้งค่า
รันคำสั่งนี้ในเทอร์มินัลของคุณ:

./config.sh --url https://github.com/YOUR_USERNAME/YOUR_REPO --token YOUR_TOKEN


YOUR_USERNAME/YOUR_REPO คือชื่อผู้ใช้และชื่อ repository ของคุณ

YOUR_TOKEN คือ GitHub runner token ที่คุณได้จากการสร้างในหน้า Settings ของ repository

เริ่มต้นการทำงานของ Runner:
รันคำสั่งนี้เพื่อเริ่มต้น GitHub Actions Runner:

./run.sh

ขั้นตอนที่ 3: ใช้ Self-Hosted Runner ใน GitHub Actions Workflow

เมื่อติดตั้งและตั้งค่า GitHub Actions Runner เสร็จเรียบร้อยแล้ว ให้เปิดไฟล์ GitHub Actions workflow ที่คุณใช้ในการ deploy โปรเจ็กต์ (ไฟล์ .yml)

เพิ่มบรรทัดนี้ในส่วนของ jobs เพื่อใช้ self-hosted runner ในการ deploy:

runs-on: self-hosted

ขั้นตอนที่ 4: การเชื่อมต่อกับ Kubernetes (สำหรับ Backend)

หากคุณต้องการเชื่อมต่อกับ backend ใน Kubernetes และทำ port-forwarding ให้รันคำสั่งนี้:

kubectl port-forward service/arainea-backend 8080:8080 -n arainea


คำสั่งนี้จะทำให้คุณสามารถเข้าถึง backend ที่ http://localhost:8080

ขั้นตอนที่ 5: การ Deploy โปรเจ็กต์

เมื่อคุณตั้งค่าทุกอย่างเสร็จแล้ว ระบบจะรันคำสั่ง deploy อัตโนมัติจาก GitHub Actions และแสดงผลการ deploy:

2025-11-21 12:16:21Z: Running job: deploy
2025-11-21 12:17:41Z: Job deploy completed with result: Succeeded

สรุป

ทำตามขั้นตอนด้านบนเพื่อดาวน์โหลดและตั้งค่า GitHub Actions Runner บนเครื่องของคุณ

ใช้ self-hosted ใน GitHub Actions Workflow เพื่อทำการ deploy โปรเจ็กต์จากเครื่องของคุณ

หากใช้ Kubernetes อย่าลืมทำการ port-forward เพื่อเข้าถึง backend
