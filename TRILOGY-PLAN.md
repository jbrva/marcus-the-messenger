# Marcus the Messenger — Trilogy Plan

Rev 2 · 2026-09-25 · absorbs Game 1 shipping reality, URL/hub decision, Passion Timeline reverent-mode design, and pointer to the Autumn Mystery prototype.

A design document for a three-game series teaching Roman numerals, the Roman calendar, and the ancient/biblical/liturgical hours of the day to elementary-age children. Each game is a self-contained set of HTML files in the same illuminated-history-book aesthetic.

---

## The trilogy at a glance

| # | Title | Subject | Status |
|---|-------|---------|--------|
| 1 | **Marcus and the Lost Scroll** | Roman numerals (I, V, X, L, C, D, M, subtractive rule, plus archaic IIII / VIIII / V̄) | ✅ Live at `marcus-the-messenger.ctso.workers.dev` |
| 2 | **Marcus and the Roman Calendar** | Months of the year + days of the week | Planned · Autumn Mystery prototype exists |
| 3 | **Marcus and the Hours of the Day** | Ancient / biblical / Orthodox liturgical hours | Planned |

**Throughline.** Marcus is a young Roman messenger boy. In Book I he learns to read numerals to deliver a scroll. In Book II he helps sort the Emperor's festival calendar. In Book III he learns how the day itself was measured — Roman sundials, biblical timekeeping, and the Church's daily prayer cycle. Numerals learned in Book I are reused throughout; the "*octo* means 8 but October is month 10" mystery in Book II directly cashes in on knowing X and VIII.

---

## URL and hub decision

The current URL stays as the hub:

```
https://marcus-the-messenger.ctso.workers.dev/
```

`index.html` is the shared landing page. Today it has three doorways (Play the Game, Parent Guide, Vade Mecum). When Books II and III ship, the landing page evolves into a **trilogy hub** with three big cards (one per book), each opening into that book's own set of pages. Books II and III live under subpaths of the same domain (e.g. `/calendar/`, `/hours/`) rather than separate subdomains — keeps DNS simple and keeps the "one URL to remember" property.

Deployment stays as-is: GitHub → Cloudflare Pages auto-deploy, `_headers` file for cache control.

---

## Game 1 as shipped

Supersedes the original plan's "recap" section. The plan understated what actually got built; Books II and III should inherit these decisions rather than treat them as retrofits.

**Content shipped:**
- Four main stages (Forum / Aqueduct / Colosseum / Emperor's Hall) plus **Stage V: The Wild Frontier** (archaic forms IIII, VIIII, V̄ bar notation)
- Final Challenge (5 questions across all stages)
- 26+ practice questions across 5 stages + final
- Vade Mecum companion (`cheat-sheet.html`) — printable two-page reference with symbols, rules, worked example, real-world places you'll see Roman numerals, plus practice + year decoder + arithmetic + answer key, with print / save-as-PDF button
- Password-gated parent/teacher guide (`for-parents-and-teachers.html`) — soft gate with a hint parents will recognize, opens onto a full pedagogical guide with per-stage conversation prompts

**Multi-player from day one:**
- Player picker screen with gendered profile cards (♀ rose/wine, ♂ imperial purple)
- Up to 6 named players per browser
- 16 Roman names built in (8 girls, 8 boys); custom names also allowed
- Each player has independent save state (`marcus-players-v2` localStorage key)
- Delete flow is captcha-protected with a Roman numeral arithmetic problem (dialed-up: e.g. `LX + XV`, `M − CD`) so kids can't accidentally erase each other

**Persistence:**
- Auto-save on every state change
- "Continue Adventure" vs "Start Over" flow on the welcome screen
- One-way migration from single-player v1 saves preserved

**File layout as shipped:**
```
Roman Numerals Game/
├── index.html                        (landing page — 3 doorways today, → hub tomorrow)
├── game.html                         (the game — multi-player, 5 stages + final)
├── cheat-sheet.html                  (Vade Mecum)
├── for-parents-and-teachers.html     (password-gated guide)
├── favicon.svg                       (wax-seal M)
├── _headers                          (Cloudflare cache config)
├── wrangler.jsonc                    (Cloudflare Workers config)
├── autumn-mystery-prototype.html     (Book II proof of concept)
├── README.md                         (handoff doc)
├── DEPLOY.md                         (deployment guide)
├── for-parents-and-teachers.md       (source for parent guide HTML)
└── TRILOGY-PLAN.md                   (this file)
```

Books II and III should follow the same pattern within their own subpath folders: `index.html` (book landing), `game.html`, `cheat-sheet.html`, `for-parents-and-teachers.html`, favicon, headers, docs.

---

## Design system (extracted, reused across all three books)

### Palette
```
--parchment       #F5E6C8   page background
--parchment-light #FAF1DC   card / modal background
--parchment-dark  #E8D4A8   subtle accents
--ink             #3A2814   body text
--ink-light       #6B4F35   secondary text
--terracotta      #C8553D   primary buttons
--terracotta-dark #9A3F2D   button hover
--wine            #7A2E1F   headings, wine-red accents
--gold            #D4A14A   badges, gold trim
--gold-dark       #A87F35   gold borders
--imperial        #5B3E7D   math boxes, imperial purple accents
--laurel          #5C7C4E   correct-answer / earned-badge green
--laurel-dark     #3F5635
--stone           #B8A984   subtle borders
--stone-dark      #8A7B5C
```

Rose bg (`#F4D5D8` / `#FBE9EB`) and imperial bg (`#E0DCEA` / `#EDEBF3`) are Book I additions used for the gendered player picker; Books II and III inherit them.

### Typography
- **Cinzel** (Google Fonts) — headings, buttons, numerals. Roman-inscription serif.
- **Lora** (Google Fonts) — body text.
- System `serif` fallback.

### Visual conventions
- Parchment card with faint texture noise (SVG filter overlay, 35% opacity)
- Scroll cards have rope-textured top/bottom edges via `::before` / `::after`
- SVG illustrations inline in JavaScript (no external image files)
- Wax-seal favicon (M in parchment on wine background with gold trim)
- Print CSS built into all pages that make sense to print (Vade Mecum especially)

### Tech pattern
- Single self-contained HTML file per screen
- Vanilla JavaScript, embedded CSS, inline SVG — no build step, no framework
- localStorage for save state (per-book key namespace, e.g. `marcus-calendar-players-v1`, `marcus-hours-players-v1`)
- Cache control via `_headers`: HTML no-cache, static assets 1-day
- Security headers baseline: `X-Content-Type-Options`, `Referrer-Policy`, `X-Frame-Options`, `Permissions-Policy`

### Shared player identity (open question, see below)
Currently each book will have its own localStorage namespace. A future v2 refactor could consolidate to a shared trilogy-wide player list so Julia's Book I profile follows her into Books II and III. Decision deferred until Book II ships.

---

## Book II — Marcus and the Roman Calendar

### Frame

Marcus, having proven himself with the Lost Scroll, is asked to help sort out the Emperor's calendar of festivals. He travels through the year, learning how each month got its name, and then through the week, learning what each day was dedicated to.

### Months — stage breakdown

**Stage I: The Temple of Janus (January, February)**
- Janus, two-faced god of doorways and beginnings, looking back at the old year and forward to the new
- February from *Februa*, a Roman purification festival held that month
- **Key setup:** these two months were added to the calendar *later* — the seed for Stage IV's mystery
- Bonus: the word **calendar** itself comes from *Kalendae*, the 1st day of each Roman month (when debts came due)

**Stage II: The Field of Mars (March, April, May, June)**
- March — Mars, god of war (campaigning season began; the old Roman year started here). Bonus: the **Ides of March** as a natural bridge from Book I's Emperor's Hall.
- April — probably from *aperire*, "to open" (buds opening). Origin slightly uncertain, which is itself worth mentioning honestly.
- May — Maia, goddess of growth and springtime
- June — Juno, queen of the gods, protector of marriage (June weddings!)

**Stage III: The Imperial Palace (July, August)**
- July — renamed from *Quintilis* (the 5th month) to honor Julius Caesar
- August — renamed from *Sextilis* (the 6th month) to honor Augustus
- The Augustus story: he wanted his month to have as many days as Caesar's, which is why July and August are back-to-back 31-day months (breaking the alternating pattern)

**Stage IV: The Autumn Mystery (September, October, November, December)**
The payoff stage. Directly reuses numerals from Book I.
- September = *septem* = 7 → but month **IX** (9)
- October = *octo* = 8 → but month **X** (10)
- November = *novem* = 9 → but month **XI** (11)
- December = *decem* = 10 → but month **XII** (12)

**The explanation:** the original Roman calendar started in March and had only 10 months. January and February were added later and eventually moved to the front of the year, pushing the numbered months two slots down. A real, memorable historical mystery hiding in plain sight on every calendar.

**Prototyped as `autumn-mystery-prototype.html`** — the 3-step guided reveal (Septem = 7 → September is 9th → why?) with a full timeline animation. Play it to confirm the mechanic lands before committing to the full Book II build.

**Final: The Calendar Scroll**
- Knuckle trick for 31-day months
- "Thirty days hath September" rhyme
- Mixed practice: month → numeral, numeral → month, origin → month
- **Bonus content (Vade Mecum sidebar):** the Julian → Gregorian reform. Why the calendar skipped 10 days in October 1582. Too much for the main flow; perfect for the printable companion.
- Victory badge

### Days of the week

**Stage V: The Seven Days**
Romans named the days after the seven "planets" (celestial bodies) visible to the naked eye, each associated with a god:

| English | Latin | Body / God | Norse replacement (in English) |
|---------|-------|------------|--------------------------------|
| Sunday | *dies Solis* | Sun | — (kept) |
| Monday | *dies Lunae* | Moon | — (kept) |
| Tuesday | *dies Martis* | Mars | Tiw's day (Norse god of war) |
| Wednesday | *dies Mercurii* | Mercury | Woden's day (Odin) |
| Thursday | *dies Iovis* | Jupiter | Thor's day |
| Friday | *dies Veneris* | Venus | Frigg's day |
| Saturday | *dies Saturni* | Saturn | — (kept) |

Two great teaching moments:
1. Romance languages (French *lundi, mardi, mercredi*; Spanish *lunes, martes, miércoles*; Italian *lunedì, martedì, mercoledì*) preserved the Roman names almost intact.
2. English is a mix: Sunday, Monday, and Saturday kept the Roman names; Tuesday through Friday got swapped for Norse equivalents when the Anglo-Saxons came to Britain. So Tuesday and *mardi* both mean "day of the war god" — just a different war god (Tiw vs. Mars).

**Question types:** match god to day, English → Latin, English → French/Spanish equivalent (recognition, not spelling), "why does English Wednesday mean the same thing as French *mercredi*?"

### Book II badges (proposed)
- Temple of Janus → **The Two-Faced Coin**
- Field of Mars → **The Laurel Wreath**
- Imperial Palace → **Caesar's Crown**
- Autumn Mystery → **The Solved Riddle**
- Seven Days → **The Seven-Star Amulet**
- Final → **The Calendar Scroll**

### Book II Vade Mecum (proposed)
- All 12 months with Latin origin
- Days of the week matrix (English / Latin / god / Norse)
- Kalends / Nones / Ides Roman-date reckoning (bonus)
- Julian → Gregorian sidebar
- Knuckle trick diagram for 31-day months

### Latin roots picked up in Book II
*Ianua* (door → January), *Februa* (purification → February), *aperire* (to open → April), *quintilis/sextilis* (5th/6th → July/August pre-rename), *septem/octo/novem/decem* (7/8/9/10 — Book I payoff), *dies* (day), *Kalendae*, *Idus*, *Nonae*. Kids passively acquire ~15 roots.

---

## Book III — Marcus and the Hours of the Day

The most spiritually substantive of the three. Preserve care in tone and accuracy — this game teaches something that matters for reading Scripture and understanding Orthodox worship.

### Background: what "the hours" meant

**The ancient system.** Daytime was divided into twelve hours, running from sunrise to sunset. Each "hour" was 1/12 of the daylight — so they stretched long in summer and shrank in winter. They were not clock hours. Night was divided into four watches. Sunrise was the reference point, roughly 6 a.m.

| Ancient hour | Approximate modern time |
|--------------|-------------------------|
| 1st hour | 6–7 a.m. (sunrise) |
| 3rd hour | ≈ 9 a.m. |
| 6th hour | ≈ noon |
| 9th hour | ≈ 3 p.m. |
| 11th hour | ≈ 5 p.m. (last hour of daylight) |
| 12th hour | sunset |

**Why "the eleventh hour" means "just barely in time":** from Christ's parable of the workers in the vineyard (Matthew 20), where laborers hired at the 11th hour still get paid — the last hour before the workday ends.

### Biblical references (with modern times)

Passages a child should be able to place on a timeline:
- **Mark 15:25** — Christ was crucified at the **third hour** (≈ 9 a.m.)
- **Matthew 27:45 / Mark 15:33 / Luke 23:44** — darkness covered the land from the **sixth to the ninth hour** (noon to 3 p.m.)
- **Matthew 27:46 / Mark 15:34** — Christ cried out and died around the **ninth hour** (≈ 3 p.m.)
- **Acts 2:15** — Peter on Pentecost: "these men are not drunk, as you suppose, since it is only the **third hour** of the day" (9 a.m. — too early to be drunk)
- **Acts 3:1** — Peter and John go up to the Temple "at the **ninth hour**, the hour of prayer" (3 p.m.)
- **Acts 10:9** — Peter goes up to pray "about the **sixth hour**" (noon)

The 3rd, 6th, and 9th hours recur as times of prayer. This is inherited Jewish practice (Daniel 6:10, "three times a day") that the early Christians continued. The Didache prescribes the Lord's Prayer three times daily.

### The Orthodox Little Hours

The Orthodox daily cycle preserves this ancient rhythm through short prayer services called the **Hours** (Ώραι / *Chasy*):

| Hour | Modern time | Commemorates |
|------|-------------|--------------|
| **First Hour** (Prime) | ≈ 6 a.m. | Dawn; Christ as the true light; asking God to direct the day |
| **Third Hour** (Terce) | ≈ 9 a.m. | The descent of the Holy Spirit at Pentecost (Acts 2:15); Christ's condemnation before Pilate |
| **Sixth Hour** (Sext) | ≈ noon | The Crucifixion (Christ nailed to the Cross); the beginning of the three-hour darkness |
| **Ninth Hour** (None) | ≈ 3 p.m. | The Death of Christ on the Cross |

**Denominational note worth including:** Prime is retained in the Orthodox rite but was suppressed in the Roman rite at Vatican II (1963). Kids in Roman Catholic families won't have First Hour in the modern Liturgy of the Hours; Orthodox kids will. Worth being explicit.

**In practice today** these are usually clustered rather than prayed at their literal hours: the 1st Hour follows Matins, the 3rd and 6th are read together before the Divine Liturgy, and the 9th Hour is read before Vespers. Monasteries keep them closer to their proper times.

**The full daily cycle** includes: Vespers (sunset), Compline (before sleep), Midnight Office, Matins, then the four Little Hours — echoing Psalm 119:164, "Seven times a day do I praise thee."

### Suggested stage structure

**Stage I: The Roman Sundial.** How the day was divided. Interactive sundial that shows how the "hour" lengths change with the seasons — a slider from June to December, watching the "hour" width breathe. Practice converting "the Nth hour" ↔ modern clock time using numerals from Book I. **Bonus content:** the gnomon (the bronze stylus that casts the shadow) as the origin of the word.

**Stage II: The Passion Timeline.** The child places the events of Good Friday on a clock face — the Crucifixion at the 3rd hour, the darkness from the 6th to the 9th, the Death at the 9th. This is the emotional heart of the game and needs its own UI convention (see "Reverent Mode" below).

**Stage III: The Hours of Prayer.** Introduce the ancient Jewish practice of praying at set hours (Daniel 6:10; Ps 55:17); then Peter and John at the 9th hour, Peter on the roof at the 6th, Peter on Pentecost at the 3rd. Establish the pattern the Church inherited.

**Stage IV: The Little Hours.** The four Orthodox Hours — what each commemorates and why. Match the hour to what it remembers.

**Stage V: The Full Daily Cycle.** All seven services. Sunrise to midnight to sunrise. "Seven times a day do I praise thee."

**Final: The Timekeeper's Scroll.** Mixed practice — scripture reference → time → what happened; hour service → what it commemorates; convert ancient hour to modern clock and back.

### Reverent Mode — a UI convention for Stage II specifically

The rest of the trilogy is quiz-shaped: right answer / wrong answer / red or green feedback. For the Passion Timeline stage, that shape is wrong. A child getting "Wrong! Christ actually died at the 9th hour" as a red-buzzer response is jarring even if factually correct.

**Reverent Mode design:**

- **No wrong-answer punishment.** The child works with a clock face and a set of event cards ("Christ nailed to the Cross," "Darkness begins," "Christ cries out," "The veil is torn," "Christ dies").
- **Placement, not selection.** The child drags each card onto the clock face. If they place it in the right spot, it locks in with a soft chime. If they place it elsewhere, it drifts gently back to the tray. No red X, no buzz. The interaction *shows* wrongness by returning, doesn't *announce* it.
- **Guided prompts, not questions.** Text prompts like "Mark reads: *And it was the third hour, and they crucified Him.* Where does this event go?" The scripture leads; the child follows.
- **Slower pacing.** No timer, no scoring pressure. When all events are placed, the timeline is shown intact with a moment of quiet before the "Continue" button appears — 2 seconds of just the completed clock face on screen. The interaction ends by looking at what happened, not by celebrating a score.
- **No badge for "winning" the Passion Timeline stage** — instead, the badge is awarded for *placing* all events. Completion, not correctness. (Since Reverent Mode doesn't let you fail, this is honest.)
- **Music-cue equivalent:** if any audio is added to the trilogy later, this is the stage where silence or a single sustained cello note is right, not a triumphant fanfare.

This is the only stage that departs from the standard quiz shape. Every other stage (including elsewhere in Book III) uses the normal correct/wrong loop. The Passion Timeline is Reverent Mode; everything else is Quiz Mode. Convention documented once here, applied once.

### Book III badges (proposed)
- The Sundial → **The Bronze Gnomon**
- The Passion Timeline → **The Sixth-Hour Sun** (a darkened sun)
- The Hours of Prayer → **The Prayer Rope**
- The Little Hours → **The Four-Pointed Star**
- The Full Cycle → **The Seven-Fold Lampstand**
- Final → **The Timekeeper's Scroll**

### Latin (and Greek) roots picked up in Book III
*Hora* (hour), *Kalendae/Nonae/Idus* (recall from Book II), *Prima/Tertia/Sexta/Nona* (Little Hour names), *matutinum* (matins), *vesper* (evening / Vespers), *compline* (from *completorium*, completion), Greek *Ώρα* (Hōra — the same word), Greek liturgical names in transliteration where a child might encounter them in Orthodox context.

---

## Cross-cutting notes

### Working-with-Claude conventions

- **URL formatting preference (saved in Claude's memory):** always paste URLs in fenced code blocks so the chat UI renders a copy button. Inline backticks don't.
- **Paste HTML/JS into triple-backticks** when sharing with Claude. Without code fences, Markdown auto-linkification corrupts property accesses (`s.name`, `e.target.closest`, `.map`) roughly 10 places per file, and the game breaks silently until they're all found and fixed. This is real, learned in Book I.
- **Claude has no session memory** except what's in the memory system. This document is the durable handoff — start any new session by pasting or referencing it, plus the current `index.html` / `game.html` if working on code.
- **Prefer targeted edits over full-file rewrites** for large files (`game.html` is now ~1500 lines) — reduces risk of typos in unrelated sections.

### Build order (recommended, revised)

1. **Play the Autumn Mystery prototype** (`autumn-mystery-prototype.html`) with your son and confirm the mechanic lands. If the "aha" hits, the trilogy concept validates.
2. **Ship Book II (Calendar)** using the file-layout pattern from Book I. Reuse everything from the design system, multi-player system, Vade Mecum pattern, parent guide pattern.
3. **Update the landing page (`index.html`)** to become the trilogy hub — three doorway cards (Book I / Book II / Book III), with Book III marked "Coming Soon" until it ships.
4. **Design Book III (Hours)** with fresh care — the theological weight deserves it. Sketch the Reverent Mode UI for Stage II before writing any code for it. The Sundial interactive (hour lengths breathing across seasons) is also worth prototyping standalone.
5. **Ship Book III.**
6. **Optional retrofit pass**: shared trilogy-wide player identity (single localStorage namespace across all three books), read-aloud mode, printable completion certificates.

---

## Open questions to revisit

- **Shared player identity across the trilogy.** Should Julia's Book I profile carry into Books II and III? Would require a shared localStorage key like `marcus-trilogy-players-v1`. Simplifies UX; requires refactoring Book I's `marcus-players-v2` key. Decision deferred until Book II ships and the shape of a "player" record is proven across two books.
- **Read-aloud mode.** For younger readers, an option to have questions read aloud (Web Speech API is free and built into the browser). Retrofit across all three books at once, once decided.
- **Printable completion certificate.** At the end of each book, generate a personalized printable certificate with the child's name and the date. Same aesthetic as the Vade Mecum. Would look like "Be it known, by the Senate and People of Rome, that **[Name]** has mastered the Roman Numerals on the **III day of MAY, MMXXVI**."
- **Days of the week — depth.** Currently one stage in Book II. If it wants to grow (Norse mythology depth, Romance language expansion, planet/god associations in more depth), could become its own tiny Book 2.5 — but one stage feels right for now.
- **Accessibility commitments.** Keyboard navigation, aria-labels on SVG illustrations, WCAG AA contrast checks. Book I has partial coverage; Books II and III should aim for full from day one.
- **Mobile / tablet UX.** Kids play on iPads. All three books should be verified on tablet form factors, not just desktop. Book I is passable but not tested exhaustively.
