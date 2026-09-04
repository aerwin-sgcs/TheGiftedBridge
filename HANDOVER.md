# The Gifted Bridge — Handover Notes

Read this first in any new chat working on this project. It captures the project's decisions, constraints, and current status so a new conversation can pick up without re-deriving any of it.

## What this is

A single-file HTML/CSS/JS teacher-facing website called **The Gifted Bridge**, helping K–12 teachers (NSW/NESA) differentiate for underachieving gifted students. Built by the same pattern as the sibling project **HelpMeLearn** (a student-facing site at https://aerwin-sgcs.github.io/HelpMeLearn/).

- Live site: https://aerwin-sgcs.github.io/TheGiftedBridge/
- Git repo (local, on the user's Mac): `~/Desktop/2026 CLAUDE ACCESS/Teachers Gifted/TheGiftedBridge/`
- The single site file: `index.html` in that folder. Everything — CSS, HTML, JS — lives in that one file.
- Deployment: GitHub Pages, pushed manually by the user via **GitHub Desktop**. Claude does not push directly (see Workflow rules below).

## Hard rules — do not violate these

1. **Never use the word "prototype" anywhere in the site's actual content.** It's fine to call it a prototype in conversation with the user, but it must never appear on the page (the user is presenting this to staff).
2. **No Claude branding, login screens, or Claude-specific UI may ever be visible to the user when viewing the site.** The user views it as a plain website in a real browser (GitHub Pages), never through Claude's inline artifact/preview links — those show a claude.ai login wall or an "open external link" dialog that looked like a bug but wasn't; it's just an artifact of Claude's preview surface, not the real site.
3. **Do not use "2e" / "twice exceptional" / "disability".** Preferred terms: **"Multi-Exceptional"** for gifted + diagnosed learning difficulty, and **"learning difficulty"** instead of "disability", throughout.
4. **Save location matters, exactly:** every file delivered in this chat — the site file, handover notes, task cards, anything — goes directly into
   `~/Desktop/2026 CLAUDE ACCESS/Teachers Gifted/TheGiftedBridge/`
   (the git repo folder itself), never the parent `Teachers Gifted/` folder. This is the default for **all** files from this project, not just `index.html`, so that anything relevant ends up in the folder that pushes to GitHub. Do not add "extra" verification/re-save steps or explain the save mechanism — just write to the correct path. The user pushes to GitHub manually via GitHub Desktop each time; that is their established, working process. Claude should not attempt `git push` directly (sandbox permissions + no git identity configured there mean it doesn't work — already tried and abandoned).
5. **Idea development happens through dialogue, not monologue.** When discussing new ideas (as opposed to executing an already-agreed build task), ask one question at a time and wait for the answer before asking the next. Don't take over the direction — this is a collaborative teaching-consultant relationship, not Claude designing solo. (This constraint applied most strongly to the original ideation phase; the build itself is now underway with the user directing specific additions.)

## Design system

CSS custom properties (palette, from an RPB furniture supplier screenshot the user liked):
```
--navy:#1D3557       --cream:#FBF9F3      --white:#FFFFFF
--royal:#3457A6       --olive:#8A9A3B      --magenta:#C05C7E
--honey:#E29A34       --deepred:#7A2430    --teal:#1B6E75
--teal-light:#E4F1F1  --border:#E4E0D4     --text-soft:#5B6472
```

## Site structure (9 nav items, 8 page sections)

Top nav is split into two explicit rows (`.navrow` divs inside `nav.mainnav`, `flex-direction:column` on the nav, each row `flex-wrap:wrap; justify-content:flex-end`) — 6 items on row 1, 3 on row 2. This was a deliberate fix requested by the user because the old single flex-wrap nav wrapped unevenly.

Row 1: Home · Foundations · Why Underachieve · Behaviour · Social-Emotional · Multi-Exceptional
Row 2: Enrichment Tasks · Curated Links · Help Students Learn

Pages (`<section class="page" id="page-...">`, shown/hidden via `showPage(id)` JS toggling a `.visible` class):

- **Home** (`page-home`) — hero + "two doors" homepage pattern: Door 1 "I have a student in mind" (situation-first → Underachieve/Behaviour/SEL), Door 2 "I'm planning a lesson" (subject-first → Enrichment Tasks). Doors are cross-linked, not silos.
- **Foundations** (`page-intro`) — 4 concept cards + Gagné DMGT anchor card + Williams Model anchor card (introduces the 8 Williams thinking/feeling processes: fluency, flexibility, originality, elaboration, risk-taking, complexity, curiosity, imagination — used as a flexible tagging system, not a rigid structure).
- **Why Underachieve** (`page-underachieve`) — anchored on Gagné's DMGT (gifts vs talents; intrapersonal/environmental catalysts) as the primary framework, with supporting research (2018–2026) listed as citations, treated as equal-weight evidence (no single source, e.g. the Heliyon review, should be singled out by name while others are generic — all citations are named equally or not at all).
- **Behaviour** (`page-behaviour`) — anchored on James T. Webb (misdiagnosis, boredom-driven disruption). 4 concept cards including "The passion connection" (recently added: behaviour that looks like defiance often eases once a student has a genuine passion area to go deep on — not a reward for good behaviour, but often the missing piece that reconnects effort with purpose).
- **Social-Emotional** (`page-sel`) — anchored on Dabrowski (overexcitabilities, asynchronous development, perfectionism).
- **Multi-Exceptional** (`page-multiexceptional`) — anchored on Susan Baum's Talent Centered Model (6 elements), covering masking effects (giftedness masks difficulty / difficulty masks giftedness / mutual cancellation) causing under-identification. Research citations include Reis/Baum/Burke 2014 and Baum's Dual Differentiation. Deliberately avoids NZ sources per the user (Australian teachers will trust it less).
- **Enrichment Tasks** (`page-tasks`) — the task bank. Fully built out for **Science, Stage 4 & 5** (12 tasks in the `SCIENCE_TASKS` array in the `<script>` block) with filter pills tagged by Williams process. All other subjects (Maths, English, Geography, History, PDHPE, Technology, Music, Art, and all Stage 6 courses) are "coming soon" stub cards in `stub-grid` — **not yet built**. A sample History task card was drafted in conversation but not yet added to the live task bank.
  - **Task card content template** (validated, reuse this exactly): Title, Stage/Year, Topic/Strand, Task description, Williams processes tagged, Why it suits underachievers.
  - Core distinction taught on this page: **enrichment ≠ extension**. Enrichment = different in kind, deeper/open-ended. Extension = more of the same, harder/faster.
- **Curated Links** (`page-links`) — currently just a "coming soon" stub. **Not yet built.** When it is: both external links and internal cross-references count, format is an annotated list explaining why each link is useful, and it's meant to be an open, growing bookmark list (not a fixed curated set).
- **Help Students Learn** (`page-helpstudent`) — links out to the sibling site https://aerwin-sgcs.github.io/HelpMeLearn/ (built the same single-file way; see its own `HANDOVER.md` in that project's folder for reference). Currently just a link-out; **does not yet have its own original teacher-facing content** — that's a backlog item if the user wants it.

## Known technical quirk (not a bug to keep re-explaining)

Files written to the repo folder via the remote-device bridge sometimes aren't picked up by GitHub Desktop's change-detection right away, even though the file's actual bytes are correct on disk. Root cause was never fully diagnosed. **Resolution the user has accepted:** just write to the correct path every time, no extra steps, no explanation of the mechanism. The user keeps their own backup copy (`indexv2.html`) and pushes manually. Do not raise this issue proactively or apologize about it — it's a settled, working process now.

## Working style notes for whoever picks this up

- The user is a K–12 Australian teacher working to a real deadline (originally ~2 weeks from early in this project; check current date against that).
- The user wants **direct, concise, non-defensive** responses. Avoid repeated apologies, avoid lengthy technical explanations of infrastructure issues, avoid hedging language that reads as blame-shifting.
- Verify visual/layout changes with a Playwright screenshot (`executable_path='/opt/pw-browsers/chromium'`) before delivering — the user has been shown screenshots of proposed changes (like the two-line nav) and this has worked well.
- Deliverable flow for any site update: edit `index.html` in the cloud workspace → screenshot-verify if it's a visual change → `SendUserFile` → `device_commit_files` straight to `~/Desktop/2026 CLAUDE ACCESS/Teachers Gifted/TheGiftedBridge/index.html` → tell the user it's ready for them to push via GitHub Desktop. Nothing more.

## Backlog (not yet done)

1. Curated Links page — still a stub.
2. Enrichment task bank for every subject other than Science Stage 4/5 (History has one drafted sample card not yet added; everything else is untouched stub cards).
3. "Help Students Learn" page — currently just an outbound link; could get its own original content.
4. Anything the user raises next — check with them before assuming priority order.
