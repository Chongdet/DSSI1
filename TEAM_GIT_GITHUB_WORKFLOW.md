# คู่มือการทำงานร่วมกันเป็นทีมด้วย Git, GitHub และ GitHub Copilot Agent

> **หัวข้อ:** AI-Driven Git Collaboration Workflow
> **เครื่องมือ:** Visual Studio Code, Git, GitHub, GitHub CLI และ GitHub Copilot Agent
> **วัตถุประสงค์:** เรียนรู้กระบวนการทำงานร่วมกันบนโปรเจกต์ที่มีอยู่แล้ว ตั้งแต่ตรวจสอบ Repository และเชื่อมต่อ GitHub ไปจนถึงพัฒนา ตรวจสอบ และรวมโค้ด โดยใช้ภาษาธรรมชาติเพื่อสั่งงาน Copilot Agent

## สารบัญ

1. [บทนำ](#1-บทนำ)
2. [ภาพรวมกระบวนการทำงาน](#2-ภาพรวมกระบวนการทำงาน)
3. [ขั้นตอนการทำงานโดยละเอียด](#3-ขั้นตอนการทำงานโดยละเอียด)
   1. [ตรวจสอบโปรเจกต์และเชื่อมต่อ GitHub](#ขั้นตอนที่-1-ตรวจสอบโปรเจกต์และเชื่อมต่อ-github)
   2. [ดึงโค้ดล่าสุดจาก GitHub](#ขั้นตอนที่-2-ดึงโค้ดล่าสุดจาก-github-pull)
   3. [สร้าง Branch สำหรับงานของตนเอง](#ขั้นตอนที่-3-สร้าง-branch-สำหรับงานของตนเอง)
   4. [พัฒนาและแก้ไขโค้ด](#ขั้นตอนที่-4-พัฒนาและแก้ไขโค้ด)
   5. [ตรวจสอบและบันทึกงาน](#ขั้นตอนที่-5-ตรวจสอบและบันทึกงาน-commit)
   6. [ส่งงานขึ้น GitHub](#ขั้นตอนที่-6-ส่งงานขึ้น-github-push)
   7. [สร้าง Pull Request](#ขั้นตอนที่-7-สร้าง-pull-request-pr)
   8. [ตรวจสอบโค้ดของสมาชิก](#ขั้นตอนที่-8-ตรวจสอบโค้ดของสมาชิก-code-review)
   9. [รวมโค้ดเข้ากับ Main Branch](#ขั้นตอนที่-9-รวมโค้ดเข้ากับ-main-branch-merge)
   10. [อัปเดตโค้ดและเริ่มงานรอบใหม่](#ขั้นตอนที่-10-อัปเดตโค้ดและเริ่มงานรอบใหม่)
4. [กฎการทำงานร่วมกันของทีม](#4-กฎการทำงานร่วมกันของทีม)
5. [ข้อควรระวังในการใช้ GitHub Copilot Agent](#5-ข้อควรระวังในการใช้-github-copilot-agent)
6. [Checklist สำหรับการทำงานจริง](#6-checklist-สำหรับการทำงานจริง)
7. [สรุป](#7-สรุป)
8. [เอกสารอ้างอิง](#8-เอกสารอ้างอิง)

## 1. บทนำ

การพัฒนาโปรเจกต์ซอฟต์แวร์ร่วมกันเป็นทีมจำเป็นต้องมีระบบจัดการ Source Code เพื่อให้สมาชิกทำงานพร้อมกันได้ โดยไม่ทำให้โค้ดของผู้อื่นเสียหาย

Git เป็นระบบควบคุมเวอร์ชัน (Version Control System) ที่ช่วยติดตามการเปลี่ยนแปลงของโค้ด ส่วน GitHub เป็นแพลตฟอร์มสำหรับจัดเก็บ Repository และสนับสนุนการทำงานร่วมกันผ่าน Branch, Pull Request และ Code Review [1][4]

ในการทำงานนี้จะใช้ **GitHub Copilot Agent** เป็นผู้ช่วยดำเนินการ เช่น ตรวจสอบสถานะ Git, เริ่มต้น Repository, เชื่อมต่อ GitHub, สร้าง Branch, Pull, Commit, Push และเตรียม Pull Request สมาชิกสามารถสั่งงานด้วยภาษาธรรมชาติ และยังคงตรวจสอบการเปลี่ยนแปลงก่อนอนุมัติการดำเนินการสำคัญได้ [9]

## 2. ภาพรวมกระบวนการทำงาน

กระบวนการทำงานร่วมกันของทีมแบ่งเป็น 10 ขั้นตอน:

1. ตรวจสอบโปรเจกต์และเชื่อมต่อ GitHub Repository
2. ดึงโค้ดล่าสุดจาก GitHub (Pull)
3. สร้าง Branch สำหรับงานของตนเอง
4. พัฒนาและแก้ไขโค้ดด้วย Copilot Agent
5. ตรวจสอบและบันทึกงาน (Commit)
6. ส่ง Branch ขึ้น GitHub (Push)
7. สร้าง Pull Request
8. ตรวจสอบโค้ดของสมาชิก (Code Review)
9. รวมโค้ดเข้ากับ Main Branch (Merge)
10. อัปเดตโค้ดล่าสุดเพื่อเริ่มงานรอบใหม่

## 3. ขั้นตอนการทำงานโดยละเอียด

### ขั้นตอนที่ 1: ตรวจสอบโปรเจกต์และเชื่อมต่อ GitHub

**วัตถุประสงค์:** ตรวจสอบโปรเจกต์ปัจจุบัน เตรียม Git Repository และเชื่อมต่อกับ GitHub Repository ก่อนเริ่มทำงานร่วมกัน

**สิ่งที่ต้องตรวจสอบ**

- โครงสร้างโปรเจกต์และตำแหน่งที่กำลังใช้งาน
- การติดตั้ง Git และ GitHub CLI
- โปรเจกต์มี Git Repository อยู่แล้วหรือไม่
- Branch ปัจจุบัน
- Git Remote และ GitHub Repository
- ไฟล์ที่มีการแก้ไขและยังไม่ได้ Commit
- การเชื่อมต่อและสิทธิ์การเข้าถึง GitHub
- การตั้งค่า `.gitignore` เพื่อป้องกันไฟล์ที่ไม่ควรเผยแพร่

**Git Commands**

```bash
git --version
gh --version
gh auth status
git status
git branch
git remote -v
git init
git branch -M main
git remote add origin <REPOSITORY_URL>
git remote -v
```

หากยังไม่ได้เชื่อมต่อ GitHub CLI ให้ล็อกอินก่อน และตรวจสอบสถานะหลังล็อกอิน [5]

```bash
gh auth login
gh auth setup-git
gh auth status
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบโปรเจกต์ปัจจุบันและเตรียมเชื่อมต่อ GitHub โดยตรวจสอบ Git, Repository, Branch และ Remote ก่อน หากยังไม่มี Git ให้เริ่มต้นด้วย `git init` และตรวจสอบหรือสร้าง `.gitignore` ให้เหมาะสม หากยังไม่มี GitHub Repository ให้เตรียมสร้างและเชื่อมต่อ `origin` หากมีอยู่แล้วห้ามสร้างซ้ำ ตรวจสอบสิทธิ์และสถานะไฟล์ทั้งหมดก่อนดำเนินการ ห้ามเขียนทับหรือลบข้อมูลเดิม และรายงานผลก่อนดำเนินการที่มีความเสี่ยง

### ขั้นตอนที่ 2: ดึงโค้ดล่าสุดจาก GitHub (Pull)

**วัตถุประสงค์:** ทำให้โค้ดในเครื่องเป็นเวอร์ชันล่าสุดก่อนเริ่มพัฒนา เพื่อลดปัญหาการทำงานบนโค้ดที่ล้าสมัย

**สิ่งที่ต้องดำเนินการ**

- ตรวจสอบการเปลี่ยนแปลงในเครื่อง
- ตรวจสอบงานที่ยังไม่ได้ Commit
- ตรวจสอบ Remote Repository
- ดึงโค้ดล่าสุดจาก Repository
- ตรวจสอบผลลัพธ์หลัง Pull

`git pull --ff-only` จะหยุดแทนการสร้าง Merge Commit เมื่อประวัติในเครื่องและ Remote แยกทางกัน จึงควรตรวจสอบสาเหตุก่อนแก้ความขัดแย้ง [3]

**Git Commands**

```bash
git status
git fetch origin
git pull --ff-only origin main
git status
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบสถานะ Git และงานที่ยังไม่ได้ Commit ก่อนดึงโค้ดล่าสุดจาก `origin/main` หากมีความเสี่ยงต่อข้อมูลสูญหายหรือเกิด Conflict ให้หยุดและแจ้งฉันก่อน เมื่อปลอดภัยแล้วจึง Pull และตรวจสอบผลลัพธ์

### ขั้นตอนที่ 3: สร้าง Branch สำหรับงานของตนเอง

**วัตถุประสงค์:** แยกพื้นที่การทำงานของสมาชิกแต่ละคนออกจาก Main Branch เพื่อป้องกันไม่ให้การแก้ไขของสมาชิกคนหนึ่งกระทบต่อโค้ดหลักของทีม

**ตัวอย่างการแบ่งงาน**

| สมาชิก | งานที่รับผิดชอบ | Branch |
|---|---|---|
| Member A | ระบบ Login | `feature/login` |
| Member B | Dashboard | `feature/dashboard` |
| Member C | ระบบ Event | `feature/event` |

คำสั่ง `git switch -c` ใช้สร้าง Branch ใหม่และสลับไปยัง Branch นั้น [2]

**Git Commands**

```bash
git branch -a
git switch main
git pull --ff-only origin main
git switch -c feature/login
git branch
```

**Prompt สำหรับ GitHub Copilot Agent**

> สร้าง Branch ใหม่ชื่อ `feature/login` โดยตรวจสอบชื่อซ้ำก่อนสร้าง และยืนยันว่าอยู่บน Branch ที่ถูกต้อง ห้ามแก้ไข `main` โดยตรง

### ขั้นตอนที่ 4: พัฒนาและแก้ไขโค้ด

**วัตถุประสงค์:** พัฒนา Feature ตามหน้าที่ที่ได้รับมอบหมาย โดยใช้ Copilot Agent ช่วยวิเคราะห์ วางแผน และแก้ไขโค้ด

**สิ่งที่ต้องดำเนินการ**

- วิเคราะห์โครงสร้างโปรเจกต์
- ตรวจสอบไฟล์ที่เกี่ยวข้องกับ Feature
- วางแผนการแก้ไขก่อนเริ่มงาน
- พัฒนาเฉพาะส่วนที่ได้รับมอบหมาย
- ตรวจสอบผลกระทบต่อส่วนอื่นของระบบ

**Git Commands**

```bash
git status
git diff
git diff --stat
git diff --check
```

**Prompt สำหรับ GitHub Copilot Agent**

> วิเคราะห์โครงสร้างโปรเจกต์และไฟล์ที่เกี่ยวข้องกับ Feature Login วางแผนการแก้ไขและรออนุมัติก่อนพัฒนา จากนั้นแก้ไขเฉพาะส่วนที่เกี่ยวข้อง พร้อมตรวจสอบ Diff และผลกระทบต่อระบบเดิม

> **หมายเหตุ:** Prompt นี้ยกตัวอย่าง Feature Login ตามต้นฉบับ ให้เปลี่ยนชื่อ Feature ให้ตรงกับงานที่ได้รับมอบหมายจริง

### ขั้นตอนที่ 5: ตรวจสอบและบันทึกงาน (Commit)

**วัตถุประสงค์:** ตรวจสอบการเปลี่ยนแปลงและบันทึกประวัติการพัฒนา เพื่อให้ติดตามได้ว่าสมาชิกแก้ไขอะไรไปบ้าง

**สิ่งที่ต้องตรวจสอบ**

- รายการไฟล์ที่เพิ่ม แก้ไข หรือลบ
- ความถูกต้องของโค้ด
- ข้อผิดพลาดที่พบ
- ข้อมูลสำคัญ เช่น Password และ API Keys
- Commit Message ที่สื่อความหมาย

**Git Commands**

```bash
git status
git diff
git diff --check
git add <FILE>
git diff --cached
git commit -m "feat: add login feature"
git log -1 --oneline
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบไฟล์ที่เพิ่ม แก้ไข และลบ ตรวจสอบ Secrets และข้อผิดพลาด สรุปสิ่งที่พัฒนาและเสนอ Commit Message ตามรูปแบบ Conventional Commits แสดง Diff และรออนุมัติก่อน Commit

### ขั้นตอนที่ 6: ส่งงานขึ้น GitHub (Push)

**วัตถุประสงค์:** ส่ง Branch ที่พัฒนาเสร็จแล้วขึ้น GitHub เพื่อให้สมาชิกคนอื่นเข้าถึงและตรวจสอบงานได้

**สิ่งที่ต้องดำเนินการ**

- ตรวจสอบ Branch ปัจจุบัน
- ตรวจสอบ Commit ล่าสุด
- ตรวจสอบไฟล์ที่กำลังจะส่ง
- Push Branch ขึ้น GitHub
- ตรวจสอบผลลัพธ์หลังส่งงาน

**Git Commands**

```bash
git status
git branch --show-current
git log -1 --oneline
git push -u origin feature/login
git branch -vv
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบ Branch, Commit และไฟล์ก่อน Push หากไม่มีปัญหาให้แสดงรายละเอียดและรออนุมัติก่อนส่ง Branch ขึ้น GitHub จากนั้นตรวจสอบผลลัพธ์

### ขั้นตอนที่ 7: สร้าง Pull Request (PR)

**วัตถุประสงค์:** ส่งงานให้สมาชิกในทีมตรวจสอบก่อนรวมโค้ดเข้ากับ Main Branch

Pull Request เป็นกระบวนการเสนอการเปลี่ยนแปลงจาก Branch หนึ่งไปยังอีก Branch หนึ่ง เพื่อให้ทีมตรวจสอบและแสดงความคิดเห็นก่อน Merge [4]

**รายละเอียดที่ควรมีใน Pull Request**

- ชื่อ Feature ที่พัฒนา
- รายละเอียดการเปลี่ยนแปลง
- รายการไฟล์สำคัญที่แก้ไข
- วิธีทดสอบ Feature
- ข้อจำกัดหรือสิ่งที่ยังต้องแก้ไข

**Git Commands**

```bash
gh pr create
gh pr list
gh pr view
```

**Prompt สำหรับ GitHub Copilot Agent**

> สร้าง Pull Request จาก Branch ปัจจุบันไปยัง `main` โดยสรุป Feature รายละเอียดการเปลี่ยนแปลง ไฟล์ที่แก้ไข และวิธีทดสอบ ตรวจสอบรายละเอียดและรออนุมัติก่อนสร้าง PR

### ขั้นตอนที่ 8: ตรวจสอบโค้ดของสมาชิก (Code Review)

**วัตถุประสงค์:** ตรวจสอบคุณภาพและความถูกต้องของโค้ดจากสมาชิกคนอื่นก่อนอนุมัติให้รวมเข้ากับ Main Branch การ Review ช่วยให้ทีมแสดงความคิดเห็น อนุมัติ หรือขอให้แก้ไขก่อน Merge [7]

**สิ่งที่ต้องตรวจสอบ**

- ความถูกต้องของโค้ด
- ความเข้ากันได้กับโครงสร้างโปรเจกต์
- ผลกระทบต่อ Feature อื่น
- ข้อผิดพลาดที่อาจเกิดขึ้น
- ความปลอดภัยของโค้ด
- การทดสอบเพิ่มเติมที่จำเป็น

**Git Commands**

```bash
gh pr view
gh pr diff
gh pr checks
git fetch origin
git diff origin/main...origin/feature/login
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบ Pull Request ของสมาชิก โดยวิเคราะห์ Code Diff, Bug, Security, Tests และผลกระทบต่อระบบ สรุปปัญหาพร้อมระดับความรุนแรงและแนวทางแก้ไข ห้ามแก้ไขหรือ Merge อัตโนมัติ

### ขั้นตอนที่ 9: รวมโค้ดเข้ากับ Main Branch (Merge)

**วัตถุประสงค์:** รวม Feature ที่ผ่านการตรวจสอบแล้วเข้ากับ Main Branch เพื่อให้กลายเป็นส่วนหนึ่งของโปรเจกต์หลัก

**ก่อน Merge ต้องตรวจสอบ**

- Pull Request ผ่าน Code Review
- สถานะการทดสอบ
- ไม่มี Merge Conflict ที่ยังไม่ได้แก้ไข
- ไม่มีปัญหาที่ส่งผลกระทบต่อ Main Branch

**Git Commands**

```bash
gh pr view
gh pr checks
git fetch origin
git diff origin/main...origin/feature/login
gh pr merge
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบว่า Pull Request ผ่าน Code Review และ Tests แล้ว รวมถึงไม่มี Conflict หากพบปัญหาให้รายงานและเสนอวิธีแก้ไข หากพร้อมให้สรุปผลและรออนุมัติก่อน Merge

### ขั้นตอนที่ 10: อัปเดตโค้ดและเริ่มงานรอบใหม่

**วัตถุประสงค์:** ทำให้สมาชิกทุกคนมีโค้ดเวอร์ชันล่าสุดหลังจากมีการ Merge งานของสมาชิกคนอื่นเข้ากับ Main Branch

**สิ่งที่ต้องดำเนินการ**

- ตรวจสอบสถานะการทำงานปัจจุบัน
- ตรวจสอบงานที่ยังไม่ได้ Commit
- ดึงโค้ดล่าสุดจาก Main Branch
- ตรวจสอบความขัดแย้งของโค้ด
- ทดสอบการทำงานร่วมกันของ Feature ต่าง ๆ

**Git Commands**

```bash
git status
git fetch origin
git switch main
git pull --ff-only origin main
git log -5 --oneline
git status
```

**Prompt สำหรับ GitHub Copilot Agent**

> ตรวจสอบงานที่ยังไม่ได้ Commit ก่อนสลับไป `main` จากนั้นดึงโค้ดล่าสุดจาก GitHub ตรวจสอบความขัดแย้งและทดสอบ Feature หากพบความเสี่ยงหรือ Conflict ให้หยุดและแจ้งฉันก่อน

## 4. กฎการทำงานร่วมกันของทีม

เพื่อให้การทำงานเป็นไปอย่างมีระบบ สมาชิกทุกคนควรปฏิบัติตามกฎต่อไปนี้:

| หัวข้อ | กฎการทำงาน |
|---|---|
| Repository | สมาชิกทุกคนต้องใช้ GitHub Repository เดียวกัน |
| Main Branch | ไม่แก้ไขโค้ดโดยตรง |
| Branch | สมาชิกแยก Branch ตามงานที่รับผิดชอบ |
| Pull | ตรวจสอบและดึงโค้ดล่าสุดก่อนเริ่มงาน |
| Commit | บันทึกงานเป็นระยะและใช้ข้อความที่สื่อความหมาย |
| Push | ส่งงานขึ้น GitHub ผ่าน Branch ของตนเอง |
| Pull Request | ต้องส่ง PR ก่อนรวมโค้ด |
| Code Review | ต้องมีการตรวจสอบงานก่อน Merge |
| Merge | ต้องได้รับการอนุมัติก่อนดำเนินการ |
| Conflict | ให้ AI วิเคราะห์และเสนอแนวทางแก้ไขก่อน |
| Security | ห้ามเผยแพร่ Password, Token หรือ API Keys |

การกำหนดให้ต้องมี Review หรือ Status Checks ก่อน Merge สามารถตั้งผ่าน Branch Protection หรือ Rulesets ของ GitHub ได้ [8]

## 5. ข้อควรระวังในการใช้ GitHub Copilot Agent

1. สมาชิกแต่ละคนควร Clone Repository ลงเครื่องของตนเอง เพื่อให้มีพื้นที่ทำงานแยกจากกัน
2. ควรตรวจสอบสถานะ Git ก่อนดำเนินการทุกครั้ง
3. ห้ามให้ AI แก้ไขหรือเขียนทับโค้ดโดยไม่ตรวจสอบผลลัพธ์
4. ก่อน Commit ควรตรวจสอบรายการไฟล์ที่เปลี่ยนแปลง
5. ก่อน Push และ Merge ควรตรวจสอบและอนุมัติการดำเนินการ
6. หากเกิด Merge Conflict ควรให้ AI วิเคราะห์สาเหตุและเสนอแนวทางแก้ไขก่อนยืนยัน
7. ควรหลีกเลี่ยงการใช้ Force Push เพราะอาจทำให้ประวัติการทำงานของสมาชิกคนอื่นสูญหาย
8. หากโปรเจกต์ยังไม่มี Git ให้ตรวจสอบก่อนใช้ `git init`
9. หากมี GitHub Repository อยู่แล้ว ให้เชื่อมต่อ Remote เดิมแทนการสร้างซ้ำ
10. สมาชิกใหม่ควรใช้ `git clone <REPOSITORY_URL>` เพื่อดาวน์โหลดโปรเจกต์ก่อนเริ่มทำงาน

> **หมายเหตุ:** ความสามารถในการ Push, สร้าง Pull Request หรือ Merge ผ่าน Copilot Agent ขึ้นอยู่กับสิทธิ์และเครื่องมือที่เชื่อมต่ออยู่ เช่น GitHub Authentication, VS Code และ GitHub CLI ในบางกรณีอาจต้องให้ผู้ใช้ดำเนินการหรืออนุมัติผ่าน GitHub ด้วยตนเอง ใน VS Code, Agent mode สามารถวิเคราะห์ไฟล์ แก้ไขโค้ด และเสนอหรือรันคำสั่งตามสิทธิ์ที่ตั้งไว้ ผู้ใช้ยังควรตรวจสอบผลก่อนอนุมัติ [9]

## 6. Checklist สำหรับการทำงานจริง

- [ ] ตรวจสอบและเตรียมโปรเจกต์
- [ ] ติดตั้งและตรวจสอบ Git
- [ ] เริ่มต้น Git Repository หากยังไม่มี
- [ ] ตรวจสอบและสร้าง `.gitignore`
- [ ] เชื่อมต่อ GitHub Repository
- [ ] ตรวจสอบ Git Remote และ Branch
- [ ] Commit โปรเจกต์ครั้งแรก
- [ ] Push โปรเจกต์ขึ้น GitHub
- [ ] ตรวจสอบและ Pull โค้ดล่าสุด
- [ ] สร้าง Branch ของตนเอง
- [ ] พัฒนา Feature ที่ได้รับมอบหมาย
- [ ] ตรวจสอบการเปลี่ยนแปลง
- [ ] Commit งาน
- [ ] Push Branch ขึ้น GitHub
- [ ] สร้าง Pull Request
- [ ] ผ่าน Code Review
- [ ] ได้รับอนุมัติและ Merge
- [ ] Pull โค้ดล่าสุดหลัง Merge
- [ ] ทดสอบโปรเจกต์หลังรวมโค้ด

## 7. สรุป

การทำงานร่วมกันด้วย Git และ GitHub โดยใช้ GitHub Copilot Agent ช่วยลดความจำเป็นในการพิมพ์ Git Commands ด้วยตนเอง สมาชิกสามารถใช้ภาษาธรรมชาติสั่งให้ AI ตรวจสอบและจัดการ Repository, Branch, บันทึกการเปลี่ยนแปลง และเตรียมส่งงานผ่าน Pull Request ได้ [9]

อย่างไรก็ตาม AI เป็นเพียงผู้ช่วยในการดำเนินงาน สมาชิกทุกคนยังคงมีหน้าที่ตรวจสอบความถูกต้องของโค้ดและอนุมัติการดำเนินการที่สำคัญ โดยเฉพาะ Commit, Push และ Merge เพื่อให้การทำงานร่วมกันเป็นทีมมีความปลอดภัยและลดความผิดพลาด

> **เป้าหมายสุดท้าย:** สมาชิกทุกคนสามารถพัฒนาโปรเจกต์เดียวกัน แบ่งงานกันอย่างเป็นระบบ และทำงานร่วมกันผ่าน GitHub ได้ โดยใช้ GitHub Copilot Agent เป็นผู้ช่วยจัดการ Git Workflow ตั้งแต่การเริ่มต้น Repository จนถึงการทำงานร่วมกันผ่าน Pull Request

## 8. เอกสารอ้างอิง

1. [Git Documentation](https://git-scm.com/docs/) — เอกสารคำสั่งและแนวคิดของ Git
2. [git-switch](https://git-scm.com/docs/git-switch) — การสลับและสร้าง Branch
3. [git-pull](https://git-scm.com/docs/git-pull) — การดึงและรวมการเปลี่ยนแปลงจาก Remote
4. [GitHub: Pull Request reference](https://docs.github.com/en/pull-requests/reference) — ภาพรวมการทำงานกับ Pull Request
5. [GitHub CLI: `gh auth login`](https://cli.github.com/manual/gh_auth_login) — การยืนยันตัวตนผ่าน GitHub CLI
6. [GitHub CLI: Pull Request commands](https://cli.github.com/manual/gh_pr) — คำสั่งสร้าง ตรวจสอบ และ Merge Pull Request
7. [GitHub Docs: About pull request reviews](https://docs.github.com/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/about-pull-request-reviews) — การ Review, Approve และ Request changes
8. [GitHub Docs: Managing and standardizing pull requests](https://docs.github.com/en/pull-requests/reference/managing-and-standardizing-pull-requests) — Branch protection, Rulesets และ Status Checks
9. [GitHub Copilot Docs: Using agent mode in your IDE](https://docs.github.com/en/copilot/how-tos/copilot-in-your-ide/use-copilot-agents/use-agent-mode) — การใช้ Copilot Agent ใน IDE
