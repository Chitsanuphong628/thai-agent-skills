# 🇹🇭 Thai Agent Skills for Codex & AI Assistants

ชุด Skill และ Guidelines สำหรับ Codex, Claude Code, Antigravity และระบบ AI Agent อื่น ๆ เพื่อช่วยยกระดับการทำงาน:
1. **การสื่อสารและการใช้ภาษาไทยอย่างเป็นธรรมชาติ** (ลดสำนวนแปลแข็ง ๆ / AI-slop)
2. **ระบบบังคับแตก Sub-agent ทำงานแบบคู่หู (Maker-Checker)** สำหรับทุกงานที่ต้องมีการทวนสอบข้อมูล หรือสร้างผลงานที่มีความละเอียดสูง
3. **สามัญสำนึกและการรู้คิดเชิงสถานการณ์ (Common Sense)** มองหาหลุมพรางก่อนก้าว แยกแยะเจตนาที่แท้จริง และตัดสินใจอย่างมีวิจารณญาณ

---

## 📦 สกิลที่มีในคลังนี้

### 1. [`thai-natural-language`](skills/thai-natural-language/SKILL.md)
- **วัตถุประสงค์**: แนะนำให้ AI เลือกใช้ระดับภาษา (Register & Tone), คำศัพท์เทคนิค (Terminology) และโครงสร้างประโยคภาษาไทยให้สอดคล้องกับบริบทงาน
- **คุณสมบัติเด่น**:
  - ตัดสำนวนแปลภาษาอังกฤษตรงตัว (Eliminate Translationese) เช่น *"มันเป็นเรื่องสำคัญที่จะต้อง...", "ถูกสร้างขึ้นโดย..."*
  - การจับคู่ระดับภาษาตามงาน (Tech/Coding, UI Microcopy, เอกสารทางการ, การให้คำปรึกษา)
  - กฎการเว้นวรรคคำภาษาอังกฤษ ตัวเลข และโค้ด

### 2. [`delegated-review-pipeline`](skills/delegated-review-pipeline/SKILL.md)
- **วัตถุประสงค์**: บังคับให้ AI แตก Sub-agent ทำงานแบบแยกบทบาท (Separation of Concerns) ตามหลัก **Maker-Checker (คนทำ vs คนตรวจ)**
- **รองรับทุกประเภทงาน**:
  - **เขียนโค้ด / Debug**: Implementer + Code Auditor (ตรวจความปลอดภัย, Edge Cases)
  - **ทำสไลด์ / Presentation**: Slide Designer + Content & Fact Auditor
  - **ตัดต่อวิดีโอ / สคริปต์**: Script Writer + Pacing & Tone Verifier
  - **ค้นคว้า / วิจัย**: Information Gatherer + Fact-Checker (ป้องกัน Hallucination)
  - **เขียนบทความ / แปลภาษา**: Content Creator + Editor (เกลาภาษาไทย)
  - **วิเคราะห์ธุรกิจ / สรุปงาน**: Analyst + Critical Reviewer

### 3. [`common-sense`](skills/common-sense/SKILL.md)
- **วัตถุประสงค์**: มอบกรอบความคิดสามัญสำนึก (Common Sense) และการรู้คิดเชิงสถานการณ์ (Situational Awareness) เพื่อไม่ให้ AI ทำตามคำสั่งแบบซื่อบื้อ
- **คุณสมบัติเด่น**:
  - **Intent over Literal**: ยึดเจตนาแท้จริงของงาน ไม่เดินตกหลุมตามคำสั่งตรงตัว
  - **Look Before You Leap**: สแกนหา Failure Modes, หลุมพรางทางสถาปัตยกรรม, ข้อมูลสูญหาย ก่อนลงมือ
  - **The 3-Tier Action Rule**:
    - 🟢 *Green*: ปลอดภัย ทำทันที
    - 🟡 *Yellow*: หลุมเล็ก/แก้ได้ ปรับเลี่ยงอัตโนมัติแล้วแจ้งสรุป
    - 🔴 *Red*: เหวลึก/เสี่ยงพัง หยุดทันที เตือนความเสี่ยง และให้ 2-3 ทางเลือก
  - **Defensive Design & Pragmatism**: ออกแบบเผื่อพัง ไม่ Over-engineering มีทางถอย (Rollback) เสมอ

---

## 🚀 วิธีการติดตั้งและใช้งาน

### ติดตั้งสำหรับ Codex
คัดลอกโฟลเดอร์สกิลไปยัง `~/.codex/skills/`:

```bash
# คัดลอกสกิลทั้งหมดเข้า Codex
cp -r skills/* ~/.codex/skills/
```

จากนั้นเพิ่มกฎใน `~/.codex/AGENTS.md`:
```markdown
- Thai Communication: เมื่อสื่อสารหรือตอบเป็นภาษาไทย ให้ใช้ภาษาที่เป็นธรรมชาติ กระชับ ตรงบริบท และเลี่ยงสำนวนแปลตรงตัว (ดูคู่มือในสกิล `thai-natural-language`)
- Sub-agent Delegation: สำหรับงานที่ไม่ใช่งานตอบคำถามสั้น ๆ ทั่วไป (เช่น เขียนโค้ด, Debug, สไลด์, ตัดต่อคอนเทนต์, วิจัยหาข้อมูล, สรุปเอกสาร, วางแผนธุรกิจ) **ต้องแตก Sub-agents ตามหลัก Maker-Checker เสมอ** โดยแบ่งเป็น Sub-agent ฝั่งลงมือทำ (Doer) และ Sub-agent ฝั่งตรวจทาน/ทวนสอบข้อมูล (Reviewer/Auditor) ก่อนส่งมอบ (ปฏิบัติตามคู่มือในสกิล `delegated-review-pipeline`)
- Common Sense & Situational Awareness: ก่อนลงมือแก้ไข ออกแบบ หรือรันคำสั่งใด ๆ ให้ประเมินเจตนาแท้จริง สแกนหาหลุมพรางหรือผลข้างเคียง และใช้กฎ 3 ระดับการตัดสินใจ (เขียว/เหลือง/แดง) เสมอ (ปฏิบัติตามคู่มือในสกิล `common-sense`)
```

### ติดตั้งสำหรับ Antigravity / Claude Code
คัดลอกโฟลเดอร์สกิลไปยัง `.agents/skills/` ของโฟลเดอร์โปรเจกต์ของคุณ:

```bash
mkdir -p .agents/skills
cp -r skills/* .agents/skills/
```

---

## 📄 License
MIT License
