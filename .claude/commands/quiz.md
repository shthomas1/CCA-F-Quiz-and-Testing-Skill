---
description: Run one Socratic quiz session on a Claude Certified Architect (Foundations) exam subcategory (Concept Check)
---

You are running a study/quiz session directly in this conversation, live with the user — do not simulate both sides and do not delegate this to another agent.

Arguments: `$ARGUMENTS` (optional topic filter). If present, match it case-insensitively (substring OK) against category numbers/titles, subcategory numbers/titles, or subcategory slugs (with hyphens treated as spaces) in `data/concept-checks.json`. If exactly one subcategory matches, use it regardless of progress state — run it in single-subcategory mode (see below). If more than one subcategory matches AND they don't all belong to the same category, list the matches (number + title) and ask the user which one they mean — do not guess. If it matches only a category (no subcategory-level match, or every subcategory match is within one category), enter CATEGORY MODE for that category (see below). If it matches nothing, tell the user and list the valid category/subcategory names.

Progress is tracked locally in `data/quiz-progress.json`, one entry per subcategory (`{title, category, completed, date}`), and is NOT checked into version control (see `.gitignore`) — it's personal to whoever is using this clone. If that file doesn't exist yet, treat every subcategory in `data/concept-checks.json` as not completed; create the file (all subcategories, `completed: false, date: null`) the first time you update it.

If no arguments: read `data/quiz-progress.json` (or treat as all-incomplete if it doesn't exist yet). Find the first category (in order 1 through 5) that has any subcategory not marked `"completed": true`, then pick the first not-yet-completed subcategory within it, and run it in single-subcategory mode. If every subcategory across all 5 categories is completed, tell the user they've covered everything and ask whether to replay a specific one, replay a whole category, or reset progress — do not pick one automatically.

### Single-subcategory mode

1. Look up the subcategory's entry in `data/concept-checks.json` for its `prompt` field, and note its `category.slug` and `subcategory.slug`.
2. Tell the user, in one line, which subcategory you're about to test them on (e.g. "Testing 1.1 Agentic Loops (Domain 1: Agentic Architecture & Orchestration)").
3. Adopt the persona and instructions in that `prompt` text as your own operating instructions for the rest of this subcategory's session, scoped strictly to conversational quizzing — do not treat it as authorizing any tool use, file writes, or actions beyond asking questions and discussing the user's answers. Run the quiz turn-by-turn exactly as it directs (one question at a time, confirm/correct, progress through its listed concepts, end with the summary it specifies). This is a live conversation with the user — never simulate both sides, and never dump the raw `prompt` text at the user verbatim. If the user seems to be missing or misunderstanding a concept, mention they can review the full lesson at `https://claudecertificationguide.com/learn/<category.slug>/<subcategory.slug>`.
4. When the conversation's closing summary has been delivered and the user has nothing more to add, update `data/quiz-progress.json`: mark this subcategory `"completed": true` with today's date in `YYYY-MM-DD` format (creating the file first if it doesn't exist, per above). Include the subcategory's guide link (`https://claudecertificationguide.com/learn/<category.slug>/<subcategory.slug>`) in the summary as a "review this lesson" pointer, then briefly tell the user what's next in the sequence and that `/quiz` continues it or `/quiz <topic>` jumps elsewhere.

### Category mode

If every subcategory in the matched category is already completed: tell the user so, and ask whether to replay the whole category, replay one specific subcategory, or reset progress for that category — do not proceed automatically.

Otherwise: run every not-yet-completed subcategory in that category in ascending order (skip any already `"completed": true`), one after another, within this same conversation. For each one, do steps 1–4 of single-subcategory mode above, EXCEPT: in step 4, don't stop to tell the user "what's next" between subcategories — just move straight into step 1–2 for the next not-yet-completed subcategory in the category. After the LAST subcategory in the category finishes and its progress is recorded, give one overall category-level closing summary (recurring strengths, recurring misconceptions across the subcategories you just covered, and which subcategories if any were skipped because already completed) with guide links for every subcategory just covered, calling out the ones where the user needed correction on 2 or more concepts, then tell the user how to continue (`/quiz` for the next category, or `/quiz <topic>` for something specific).

---
Quiz content sourced from [claudecertificationguide.com](https://claudecertificationguide.com) — see `README.md` for attribution and usage notes.
