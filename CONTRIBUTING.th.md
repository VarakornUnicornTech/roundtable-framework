# การมีส่วนร่วมกับ RoundTable Framework

ขอบคุณที่สนใจร่วมพัฒนา RoundTable Framework! โปรเจกต์นี้แปลง AI assistant ตัวเดียวให้กลายเป็นองค์กรวิศวกรรมแบบหลายทีมที่ทำงานร่วมกันบน Claude Code

## สร้างและใช้งานจริงในระบบ Production

> **นี่ไม่ใช่ framework ทฤษฎี** RoundTable Framework ถูกใช้งานจริงในระบบ production โดย [Unicorn Tech Integration Co., Ltd.](https://github.com/VarakornUnicornTech) เพื่อสร้างและส่งมอบซอฟต์แวร์จริง — ขับเคลื่อนด้วย Claude Code เต็มรูปแบบ ควบคุมโดย framework นี้ และสั่งการโดยมนุษย์ (Commander) ทุก policy, ทุก workflow, ทุก quality gate ที่คุณเห็นที่นี่ผ่านการทดสอบจริงในการพัฒนาจริง หากคุณกำลังมองหาเครื่องมือ AI governance ที่สร้างโดยผู้ใช้จริงที่ใช้ทุกวัน — คุณพบมันแล้ว

## จรรยาบรรณ

การเข้าร่วมโปรเจกต์นี้ หมายความว่าคุณยอมรับ [จรรยาบรรณ](CODE_OF_CONDUCT.md) ของเรา เรามุ่งมั่นที่จะสร้างประสบการณ์ที่เป็นมิตรและครอบคลุมสำหรับทุกคน

## ฉันจะมีส่วนร่วมได้อย่างไร?

### รายงานบัก

- ใช้ [แบบฟอร์มรายงานบัก](https://github.com/VarakornUnicornTech/UniOpsQC/issues/new?template=bug_report.md)
- ระบุเวอร์ชัน Claude Code และระบบปฏิบัติการของคุณ
- อธิบายขั้นตอนในการทำซ้ำปัญหา
- แนบ log output ที่เกี่ยวข้อง (ถ้ามี)

### เสนอฟีเจอร์ใหม่

- ใช้ [แบบฟอร์มเสนอฟีเจอร์](https://github.com/VarakornUnicornTech/UniOpsQC/issues/new?template=feature_request.md)
- อธิบายปัญหาที่ฟีเจอร์ของคุณจะแก้ไข
- อธิบายวิธีแก้ปัญหาที่คุณเสนอ
- พิจารณาแนวทางทางเลือก

### ส่ง Pull Request

ยินดีต้อนรับทุกการมีส่วนร่วม ตั้งแต่แก้พิมพ์ผิดจนถึงฟีเจอร์ใหม่ ปฏิบัติตามขั้นตอนด้านล่าง

### ปรับปรุงเอกสาร

การปรับปรุงเอกสารมีคุณค่าอย่างยิ่ง เรารักษาทั้งภาษาอังกฤษและภาษาไทย หากคุณช่วยแปลได้ จะเป็นประโยชน์อย่างมาก

## การตั้งค่าสำหรับนักพัฒนา

### สิ่งที่ต้องมี

- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code) ติดตั้งและกำหนดค่าแล้ว
- Git 2.30+
- บัญชี GitHub

### Fork & Clone

```bash
# Fork repository บน GitHub แล้ว:
git clone https://github.com/YOUR_USERNAME/UniOpsQC.git
cd UniOpsQC
git remote add upstream https://github.com/VarakornUnicornTech/UniOpsQC.git
git fetch upstream
```

### รูปแบบการตั้งชื่อ Branch

สร้าง branch จาก `dev` โดยใช้ prefix ดังนี้:

| Prefix | วัตถุประสงค์ | ตัวอย่าง |
|--------|------------|---------|
| `feature/` | ฟีเจอร์ใหม่ | `feature/add-review-skill` |
| `fix/` | แก้บัก | `fix/hook-path-resolution` |
| `docs/` | เอกสาร | `docs/update-getting-started` |
| `security/` | ความปลอดภัย | `security/input-validation` |

## ขั้นตอนการส่ง Pull Request

### 1. Fork & Branch จาก `dev`

```bash
git checkout dev
git pull upstream dev
git checkout -b feature/your-feature-name
```

### 2. ทำการเปลี่ยนแปลง

- ปฏิบัติตาม Style Guide
- ทดสอบการเปลี่ยนแปลงกับ Claude Code
- อัพเดตเอกสารหากจำเป็น

### 3. ทดสอบในเครื่อง

```bash
# ตรวจสอบว่า framework ทำงานได้กับ Claude Code
claude
```

### 4. Commit

```bash
git commit -m "feat(skills): เพิ่ม skill สำหรับ review workflow ใหม่"
```

### 5. ส่ง PR ไปยัง `dev`

```bash
git push origin feature/your-feature-name
```

จากนั้นสร้าง Pull Request ไปยัง branch `dev` กรอกแบบฟอร์ม PR ให้ครบถ้วน

### 6. กระบวนการ Review

- Maintainer จะ review PR ของคุณตาม SLA ที่กำหนด
- แก้ไขตามที่ร้องขอ
- เมื่อ approve แล้ว maintainer จะ merge ให้

**สำคัญ:** เฉพาะ maintainer เท่านั้นที่สามารถ merge PR ได้

## กลยุทธ์ Branch

```
feature/* ──> dev ──> staging ──> main
fix/*     ──> dev ──> staging ──> main
hotfix/*  ──> staging ──> main (+ cherry-pick ไปยัง dev)
```

| Branch | วัตถุประสงค์ | ใครเป็นคน Merge |
|--------|------------|----------------|
| `main` | Production / เวอร์ชันที่เผยแพร่ | Owner เท่านั้น |
| `staging` | ตรวจสอบก่อนเผยแพร่ | Maintainer (พร้อม review) |
| `dev` | รวมฟีเจอร์และการแก้ไข | Maintainer |
| `feature/*` | Branch สำหรับทำงาน | คุณ (ผ่าน PR ไปยัง dev) |

ดูรายละเอียดเพิ่มเติมที่ [docs/BRANCH_STRATEGY.md](docs/BRANCH_STRATEGY.md)

## โซนที่ได้รับการคุ้มครอง

Path ดังต่อไปนี้ต้องได้รับ **review จาก Maintainer หรือ Owner** ผ่าน CODEOWNERS:

| Path | คืออะไร | ต้องได้รับ Review จาก |
|------|---------|---------------------|
| `.claude/CLAUDE.md` | สมองหลักของ framework | Owner |
| `.claude/agents/` | นิยามตัวตนของทีม | Owner |
| `.claude/settings.json` | การตั้งค่า hook | Owner |
| `.claude/policies/` | กฎการปกครอง | Maintainer |
| `.claude/skills/` | นิยาม skill | Maintainer |
| `template-version.json` | ความสมบูรณ์ของเวอร์ชัน | Owner |

คุณยังคงส่ง PR ที่แก้ไขไฟล์เหล่านี้ได้ — แค่ต้องได้รับ approval จากผู้ที่กำหนดเท่านั้น

## รูปแบบ Commit Message

เราใช้ [Conventional Commits](https://www.conventionalcommits.org/):

```
type(scope): คำอธิบายสั้นๆ

[รายละเอียดเพิ่มเติม — ไม่บังคับ]
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `security`

**Scopes:** `core`, `policies`, `skills`, `agents`, `docs`, `ci`, `hooks`

## ผู้มีส่วนร่วมที่เป็น AI Agent

RoundTable Framework ยินดีต้อนรับการมีส่วนร่วมจาก AI agent ดูรายละเอียดที่ [docs/AI_AGENTS.md](docs/AI_AGENTS.md)

**กฎสำคัญ:**
1. ต้องระบุว่าใช้ AI tool อะไรในแบบฟอร์ม PR
2. ต้องมีมนุษย์เป็นผู้สนับสนุนและรับผิดชอบทุก PR ที่สร้างโดย AI
3. การมีส่วนร่วมจาก AI ผ่านกระบวนการ review เดียวกับมนุษย์
4. ห้ามส่ง PR จำนวนมากแบบอัตโนมัติโดยไม่ได้รับอนุมัติล่วงหน้า

## ชุมชน

- **Discord:** [เข้าร่วม community](https://discord.gg/SwYzZjru) — พูดคุย ถามคำถาม แชร์การตั้งค่าของคุณ
- **คำถาม:** ใช้ [GitHub Discussions](https://github.com/VarakornUnicornTech/UniOpsQC/discussions)
- **บัก:** ใช้ [Issues](https://github.com/VarakornUnicornTech/UniOpsQC/issues)
- **ฟีเจอร์:** ใช้ [Issues](https://github.com/VarakornUnicornTech/UniOpsQC/issues)
- **ความปลอดภัย:** ดู [SECURITY.md](SECURITY.md) — **ห้าม** เปิด issue สาธารณะสำหรับช่องโหว่

## สิทธิ์การใช้งาน

การมีส่วนร่วมกับ RoundTable Framework หมายความว่าคุณยอมรับให้ผลงานของคุณอยู่ภายใต้ [MIT License](LICENSE)

## การให้เกียรติ

ผู้มีส่วนร่วมทุกคนจะได้รับเครดิตใน [CHANGELOG.md](CHANGELOG.md) ผลงานที่สำคัญจะถูกเน้นในบันทึกการเผยแพร่

---

*ขอบคุณที่ช่วยทำให้ RoundTable Framework ดียิ่งขึ้นสำหรับทุกคน*
