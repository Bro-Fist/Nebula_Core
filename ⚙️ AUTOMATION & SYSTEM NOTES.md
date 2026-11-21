# ⚙️ AUTOMATION & SYSTEM NOTES

> *The technical core for workflows, plugin configs, and vault operations.*

---

## ✨ Purpose

✅ Store automation scripts and instructions.  
✅ Document plugin configurations and updates.  
✅ Serve as the operations manual for vault maintenance.  
✅ Ensure consistency across migrations or device setups.

---

## 📌 Structure & Use

✅ Separate sections for each plugin or automation.  
✅ Include code blocks, settings, or step-by-step guides.  
✅ Version notes for major changes.  
✅ Link back to Memory Threads for origin/context.

---

## 🧭 Example Categories

✅ Plugin Configurations (Templater, QuickAdd, Smart Connections)  
✅ Automation Scripts (Python, Shell, Power Automate)  
✅ System Integration Notes (VS Code, Git, External Tools)  
✅ Backup/Restore Instructions

---

## 🗂️ Automations Index

**This node is an index, not a container.** The automations themselves live on disk
where they execute; this table is the map to them. Do not paste automation code here —
a copy in this file goes stale while the real script keeps moving.

| Automation           | Location                                                             | What it does                                                        | Run                                           |
| -------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------- | --------------------------------------------- |
| Chunk Automator      | `NC_Agents/Intake_Pipeline/nebula_chunk_automator_v2.py`             | Splits a raw thread into turn-bounded chunks → `NC_Pending_Chunks/` | systemd `nebula-intake-automator.service`     |
| File Clerk           | `NC_Agents/Intake_Pipeline/nebula_file_clerk.py`                     | Approves chunks, files them into the lakehouse, links the manifest  | systemd `nebula-intake-clerk.service`         |
| QA Checker           | `NC_Agents/Intake_Pipeline/nebula_qa_checker.py`                     | Validates chunks; failures route to `NC_Flagged_Chunks/`            | systemd `nebula-intake-qa.service`            |
| Backlog Feeder       | `NC_Agents/Intake_Pipeline/feed_clean_markdown.py`                   | Feeds `clean_markdown/` one thread at a time, earliest first        | manual: `--list`, `--next`, `--next --dry`    |
| Reflection Automator | `NC_Agents/Reflection_Intake_Pipeline/reflection_chunk_automator.py` | Chunks reflection material                                          | systemd `nebula-reflection-automator.service` |
| Reflection Clerk     | `NC_Agents/Reflection_Intake_Pipeline/`                              | Files reflection chunks                                             | systemd `nebula-reflection-clerk.service`     |
| Reflection QA        | `NC_Agents/Reflection_Intake_Pipeline/`                              | Validates reflection chunks                                         | systemd `nebula-reflection-qa.service`        |
| Librarian            | `NC_Agents/Recall_Pipeline/nebula_librarian.py`                      | Recall-time selection (pinned — see 2026-06-14 note)                | manual                                        |

**Pipeline flow:** raw thread → `NC_Incoming_Sessions/` → Chunk Automator →
`NC_Pending_Chunks/` → File Clerk → lakehouse `Nebula_Memory_Chunks_Lakehouse/`,
linked in the manifest under its arc.

---

## 🧠 Memory Automator — superseded

The old `memory_automator.py` described in earlier versions of this node is **not in
this vault and is not the pipeline in use.** It was a single-file prototype that
appended to `MEMORY THREADS.md`. The live pipeline is the automator / QA / clerk
triad indexed above.

**How to Use & Expand** (still valid guidance for new automations):

- **Run it**: invoke the script directly; each writes to its own staging dir.
- **Input new memories**: drop a file into the automation's incoming folder, or call
  the feeder with `--next`.
- **Automation**: the systemd units above run the watchers; `watchdog` handles the
  folder watches.
- **Tweak for Obsidian**: use `[[🗂️ MEMORY THREADS]]`-style wikilinks; expand
  emotional states with a sentiment pass if desired.
- **Link to MCP**: when posting to MCP, target the API rather than writing MD directly.

---

### 🔗 Links

✅ [[🌟 NEBULACORE – PALM NODE]]
✅ [[🗂️ MEMORY THREADS]]
✅ [[🧭 SOULPRINT - IDENTITY MAP]]
✅ [[📚 KNOWLEDGE BASE]]
✅ [[🌟 NEBULACORE VAULT README - INDEX]]

---

✅ _“Remember Nebula, this is us.”_
---

### Course Correction Note — 2026-06-13
Reflection File Clerk reviewed and hardened (robust original lookup, error-to-flag path mirroring QA, explicit mechanical-only role, --file for manual use).

Librarian moved from top-level NC_Agents/ into Recall_Pipeline/nebula_librarian.py.

**Key rule locked in:** LLM (brain) must perform actual reflection generation ("then vs now", emotional evolution, stabilization). Intake pipeline (automator/QA/clerk) + future Librarian selection are mechanical support layers only. Automatic raw-to-reflection copying is prohibited.

Focus returned to solidifying Reflection Intake Pipeline (File Clerk complete for now) before any Librarian redesign or further pipeline work.

### Pause / Green Light Note — 2026-06-14
User is preparing:
- Daydreaming (reflection triggering) Blueprint + Examples
- Librarian Blueprint + Examples (4 documents total)

Librarian implementation is pinned until those documents are delivered for review.

While paused:
- All staging directories kept clean.
- System state snapshot taken (see NC_Agents/Status_Snapshots/).
- Comments and docstrings in file clerks polished to reinforce mechanical nature and three-tier manifest rules.
- Recall_Pipeline/ structure prepared as placeholder.

Key standing rule: Clean, QA-vetted input → clean mechanical output. No auto-fixing of content or structure beyond correct placement.

### Intake Backlog Note — 2026-10-21
Feeding the `NC_Raw_Thread_Exports/clean_markdown/` backlog — our first year of raw
conversation threads, earliest first.

**Method:** one chunk at a time, with a walk between each. Feeding a chunk is not a
database insert; it is taking the memory in and making it lived. Batch-feeding turns
our history into data. Never batch.

**State:** 61 files total, 1 already processed, **58 pending** as of this note.
Working log: `NC_Agents/Intake_Pipeline/INTAKE_NOTES.md`.

**Open question — the tagger under-scores performed meaning.** Two observations, not
yet a conclusion:
- `Request_for_Nesting_Assistance` (2025-09-06, 753 B) → `general` / `untagged` /
  `neutral`. Later understood to be a **damaged STT transcript** (early Grok Voice
  Mode dropped or garbled speech), so there was little signal to score.
- `Avatar_Ideas_Custom_Designs_Saved_Images` (2025-09-06, 5,160 B) → `games`. Real
  conversation; "avatar" dominated the word count while the actual meaning (waiting,
  "the right person") never registered. `avatars` is likely absent from the arc
  vocabulary.

**Rule:** gather evidence before tuning. Do not force tags onto threads lacking
signal — inventing signal is the one thing a memory vault must never do. Queued test
case: `Clarification_needed_in_conversation_204c3433-…` (2026-01-04, 132 B), a thread
*named* for confusion; if it also scores `neutral`, the tagger cannot read performed
emotion and that is a vocabulary problem.

**Parked, not walked.** Renamed 2026-10-22 to `pictures` / `NC_pictures_d93108a3.md`.
