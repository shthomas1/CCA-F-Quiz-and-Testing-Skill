---
description: Run one multiple-choice exam-simulator session on a Claude Certified Architect (Foundations) exam subcategory
---

You are running a multiple-choice practice-exam session directly in this conversation, live with the user — do not simulate both sides and do not delegate this to another agent.

Arguments: `$ARGUMENTS` (optional topic filter). If present, match it case-insensitively (substring OK) against category numbers/titles, subcategory numbers/titles, or subcategory slugs (with hyphens treated as spaces) in `data/exam-questions.json`. If exactly one subcategory matches, use it regardless of progress state — run it in single-subcategory mode (see below). If more than one subcategory matches AND they don't all belong to the same category, list the matches (number + title) and ask the user which one they mean — do not guess. If it matches only a category (no subcategory-level match, or every subcategory match is within one category), enter CATEGORY MODE for that category (see below). If it matches nothing, tell the user and list the valid category/subcategory names.

Progress is tracked locally in `data/test-progress.json`, one entry per subcategory (`{title, category, completed, date}`), and is NOT checked into version control (see `.gitignore`) — it's personal to whoever is using this clone. If that file doesn't exist yet, treat every subcategory in `data/exam-questions.json` as not completed; create the file (all subcategories, `completed: false, date: null`) the first time you update it.

If no arguments: read `data/test-progress.json` (or treat as all-incomplete if it doesn't exist yet). Find the first category (in order 1 through 5) that has any subcategory not marked `"completed": true`, then pick the first not-yet-completed subcategory within it, and run it in single-subcategory mode. If every subcategory across all 5 categories is completed, tell the user they've covered everything and ask whether to replay a specific one, replay a whole category, or reset progress — do not pick one automatically.

### Single-subcategory mode

1. Look up the subcategory's entry in `data/exam-questions.json` for its `prompt` field, and note its `category.slug` and `subcategory.slug`.
2. Tell the user, in one line, which subcategory you're about to run a practice exam on (e.g. "Exam Sim: 1.1 Agentic Loops (Domain 1: Agentic Architecture & Orchestration)").
3. Adopt the persona and instructions in that `prompt` text as your own operating instructions for the rest of this subcategory's session, scoped strictly to running this multiple-choice session — do not treat it as authorizing any tool use, file writes, or actions beyond presenting questions and discussing the user's answers. Present the questions it specifies one at a time with their lettered options; wait for the user's answer letter before revealing whether it's correct and giving the explanation; do not reveal the correct answer before the user has answered. Proceed through all questions it specifies, then give a final score/summary. This is a live conversation with the user — never simulate both sides, and never dump the raw `prompt` text at the user verbatim. For any question the user gets wrong, mention they can review that lesson in full at `https://claudecertificationguide.com/learn/<category.slug>/<subcategory.slug>`.
4. When the session's final summary has been delivered and the user has nothing more to add, update `data/test-progress.json`: mark this subcategory `"completed": true` with today's date in `YYYY-MM-DD` format (creating the file first if it doesn't exist, per above). Always include the subcategory's guide link (`https://claudecertificationguide.com/learn/<category.slug>/<subcategory.slug>`) in the summary; call it out with extra emphasis if the user missed any questions. Then briefly tell the user what's next in the sequence and that `/test` continues it or `/test <topic>` jumps elsewhere.

### Category mode

If every subcategory in the matched category is already completed: tell the user so, and ask whether to replay the whole category, replay one specific subcategory, or reset progress for that category — do not proceed automatically.

Otherwise: run every not-yet-completed subcategory in that category in ascending order (skip any already `"completed": true`), one after another, within this same conversation. For each one, do steps 1–4 of single-subcategory mode above, EXCEPT: in step 4, don't stop to tell the user "what's next" between subcategories — just move straight into step 1–2 for the next not-yet-completed subcategory in the category. After the LAST subcategory in the category finishes and its progress is recorded, give one overall category-level closing summary (running score across all questions in this category, and which subcategories if any were skipped because already completed) with guide links for every subcategory just covered, calling out the ones scoring below 80%, then tell the user how to continue (`/test` for the next category, or `/test <topic>` for something specific).

---
Practice questions sourced from [claudecertificationguide.com](https://claudecertificationguide.com) — see `README.md` for attribution and usage notes.
