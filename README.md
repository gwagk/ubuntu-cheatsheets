# Ubuntu Cheatsheets & Setup Guides

คลังรวมโพยคำสั่ง Ubuntu ตั้งแต่ตั้งค่า SSH/Git ยันคำสั่ง Terminal ประจำวัน สำหรับเซ็ตอัปเครื่องใหม่ในพริบตา

---

## 1. ติดตั้ง Git บนเครื่องใหม่
```bash
git --version
sudo apt update && sudo apt install -y git

2. ตั้งค่าชื่อและอีเมล (ทำครั้งเดียวต่อเครื่อง)

(เปลี่ยน YOUR_NAME และ your_email@example.com เป็นข้อมูลของท่าน)
Bash

git config --global user.name "YOUR_NAME"
git config --global user.email "your_email@example.com"

3. การสร้าง SSH Key และนำไปผูก GitHub (สำหรับเครื่องใหม่)
Bash

ssh-keygen -t ed25519 -C "your_email@example.com"

(กด Enter ผ่านไปเรื่อย ๆ จนเสร็จ)

เปิดดู Public Key เพื่อนำไปวางบน GitHub (Settings -> SSH and GPG keys):
Bash

cat ~/.ssh/id_ed25519.pub

4. ทดสอบการเชื่อมต่อกับ GitHub ผ่าน SSH
Bash

ssh -T git@github.com

5. แผนผังคำสั่ง Git ประจำวัน (Daily Workflow)
Bash

git add .
git commit -m "คำอธิบายสิ่งที่แก้ไขหรือเพิ่มเข้ามา"
git push

6. การ Clone Repository นี้ไปใช้งานบนเครื่องใหม่
Bash

git clone git@github.com:<YOUR_GITHUB_USERNAME>/ubuntu-cheatsheets.git
cd ubuntu-cheatsheets

EOF


---

#### 2. แก้ไข commit เดิม และ Push ทับประวัติบน GitHub ทันที

รัน 3 บรรทัดนี้ครับ:

```bash
git add README.md
git commit --amend -m "Initial commit: Ubuntu cheatsheets repository"
git push --force origin main
