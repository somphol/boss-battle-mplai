# Boss Battle: ม.ปลาย (M.4–M.6)

An arcade quiz game for Thai high-school core subjects, built as one offline HTML file.
Answer correctly to damage the boss, chain combos, spend three tactical skills, and survive the rage phase.

**Play:** https://somphol.github.io/boss-battle-mplai/

## Subjects (7)

| Subject | Questions | Based on |
|---|---|---|
| ฟิสิกส์ | 101 | หลักสูตรแกนกลาง 2551 (ปรับปรุง 2560) · สสวท |
| เคมี | 100 | สาระที่ 2 วิทยาศาสตร์กายภาพ · สสวท |
| ชีววิทยา | 100 | สาระที่ 1 วิทยาศาสตร์ชีวภาพ · สสวท |
| คณิตศาสตร์ | 99 | กลุ่มสาระคณิตศาสตร์ · สสวท |
| ภาษาไทย | 100 | กลุ่มสาระภาษาไทย |
| สังคมศึกษา | 100 | หน้าที่พลเมือง เศรษฐศาสตร์ ประวัติศาสตร์ ภูมิศาสตร์ |
| ภาษาอังกฤษ | 100 | กลุ่มสาระภาษาต่างประเทศ |

**Total: 700 questions**, every item with an explanation.

Every question also shows **what to review next**: the topic, the textbook chapter (สสวท strand and
grade), and a link to an online lesson portal (IPST Project 14, OBEC, or Khan Academy).

## Features

- **5 bosses + Boss Rush** — each subject has its own boss; Rush mixes 20 questions from all seven.
- **Three difficulty tiers** in the boss dossier: ผสม / พื้นฐาน (ง่าย–กลาง) / เข้มข้น (กลาง–ยาก).
- **Three skills** with charges + cooldowns: 🛡️ Shield, ⏱️ Time Freeze, 💡 50/50.
- **Rage phase** below 50% boss HP: red aura and a shorter clock.
- Combo/crit damage, screen shake, canvas particles, synthesised Web Audio (no audio files).
- **Bilingual UI** (Thai default, English toggle). Question content stays Thai, except the English set.
- **Bottom tabs** (เล่น / สถิติ / ตั้งค่า), subject carousel, one-tap 🏠 in battle.
- Local stats, per-subject mastery, best scores, day streak.

## Controls

| Action | Key |
|---|---|
| Answer | `1` `2` `3` `4` |
| Shield / Freeze / 50-50 | `Q` `W` `E` |
| Next question | `Enter` or `Space` |
| Quit battle | `Esc` or 🏠 |

## About the questions

All items are **original practice questions written for this project** against the topic map of the
national core curriculum (2551, revised 2560) and สสวท textbook strands. They are not copied from
textbooks, exams, or any copyrighted question bank. They are a study aid, not an official assessment.

## Run locally

```bash
open index.html
# or
python3 -m http.server 8080
```

See [AGENTS.md](AGENTS.md) for the engineering handoff.
