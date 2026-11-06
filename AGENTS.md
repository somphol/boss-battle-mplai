# AGENTS.md — Boss Battle: ม.ปลาย

## 1. What it is

A static, single-file arcade quiz for Thai high-school core subjects (M.4–M.6): physics, chemistry,
biology, math, Thai, social studies and English. Correct answers damage a boss; wrong answers cost a
heart and heal the boss. 420 original questions, each with an explanation.

Content is aligned to the topic map of the national core curriculum (2551, revised 2560) and the
สสวท textbook strands, but every question is written for this project — nothing is copied from
textbooks or exams.

This repo is a **sibling** of `boss-battle-tpat3`, which keeps the TPAT3/engineering/logic set.
Physics, math and English questions were carried over from there; the four school-only subjects
(chemistry, biology, Thai, social) are new.

## 2. Commands

```bash
# run
open index.html
python3 -m http.server 8080

# syntax check (the only "build")
node -e 'const fs=require("fs");const h=fs.readFileSync("index.html","utf8");fs.writeFileSync("/tmp/bbm.js",h.match(/<script>([\s\S]*?)<\/script>/)[1])'
node --check /tmp/bbm.js

# bank validation (schema, duplicate ids/stems, answer index, CJK leaks)
node -e 'const fs=require("fs");const h=fs.readFileSync("index.html","utf8");const b=new Function("return ["+h.match(/const BANK = \[([\s\S]*?)\n\];/)[1]+"]")();/* see AGENTS §3 */'

# deploy: push to main; Pages serves the repo root
git push origin main
```

## 3. Architecture

Same engine as `boss-battle-tpat3` (navigation v2). One `index.html` containing:

1. `<style>` — tokens, views, carousel, boss ring, question card, skills, sheets, overlays.
2. Markup — `#vMenu`, `#vBattle`, `#vEnd`, `#vStats`, `#tabbar`, `#setSheet`, `#ov`, FX layers.
3. `<script>` IIFE — helpers → audio → storage → i18n → subjects/bank → DOM refs → particles →
   state → menu → dossier → battle → timer → skills → HUD → finish → stats → events → boot.

Control points:

| Location | Controls |
|---|---|
| `SUB` | 7 subjects: Thai/English name, emoji, accent colour, boss name, description |
| `BANK` | 420 questions, one per line, keys `id,s,d,q,o,a,w` |
| `TIER` | `mix` \\| `core` (d1–2) \\| `hard` (d2–3), applied in `buildSet()` |
| `S` | Battle state; `startBattle()` resets it |
| `M.th` / `M.en` + `t()` | UI copy; static nodes use `data-i18n` / `data-i18n-aria` |
| `renderMenu` / `paintDossier` / `renderStats` / `paintEnd` | Re-rendered on language change |
| `setView` / `goHome` / `goStats` / `openSheet` | Navigation |

Question schema:

```js
{ "id":"ch1", "s":"chemistry", "d":1, "q":"…", "o":["…","…","…","…"], "a":1, "w":"…" }
```

`id` unique · `s` ∈ physics | chemistry | biology | math | thai | social | english · `d` 1–3 ·
`o` exactly 4 · `a` correct index · `w` Thai explanation.

The bank in this file is serialized one-object-per-line (quoted keys). It is valid JS and easy to
diff. Adding questions: append a line before `];` and run the validator.

## 4. Decisions and constraints

- **Zero external requests** — no fonts, CDN, images or audio files. Must run from `file://`.
- **Thai is the default UI language**, persisted at `bbTPAT3_v1.lang` (same key as the sibling repo).
- **Question content is Thai** except the English subject. The English UI does not translate content.
- **Subjects are school subjects, not exam papers.** Engineering/logic from the TPAT3 repo are
  deliberately excluded here.
- **The new subjects are seed banks (30 each).** This is intentional for the first release; expand
  them in batches. Keep 4 options, one correct, a Thai explanation, and no duplicate stems.
- **Balance:** boss 1300 HP (Rush 1600), damage `70 + 12×combo` capped at combo 6, crit ×1.5 within
  8 s, wrong answer −1 heart and boss +30 HP, 32 s clock → 26 s in rage. Retune together.
- **Navigation:** bottom tabs are the home affordance; do not reintroduce the old chip row.
- **No grade tags yet.** Adding `g: 4|5|6` to the schema and a grade filter is the next planned
  change; do it for all subjects at once, not only the new ones.

## 5. Known issues

- Chemistry, biology, Thai and social each have only 30 questions. Heavy users will see repeats in
  a few runs. Expansion is the main maintenance task.
- Grade level (ม.4/5/6) is not tagged, so the difficulty tiers are the only filter.
- Content is written by the assistant against the curriculum map; it has **not been reviewed by a
  teacher**. Treat it as practice material and spot-check before classroom use.
- Emoji rendering depends on the OS font.
- Audio is synthesised; stats are per-device.
- No committed test suite; verification is manual + headless Chrome.

## 6. Continuity

- **Deployed:** https://somphol.github.io/boss-battle-mplai/ (Pages, `main`, root)
- **Repo:** https://github.com/somphol/boss-battle-mplai
- **Sibling repo:** https://github.com/somphol/boss-battle-tpat3
- **Local:** `/Users/somphol/Projects/boss-battle-mplai/index.html`
- **Sources used for the topic map:** หลักสูตรแกนกลางการศึกษาขั้นพื้นฐาน 2551 (ปรับปรุง 2560),
  สสวท (ipst.ac.th) subject strands, and the standard M.4–M.6 topic sequence for each subject.
