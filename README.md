# CCAR Skills — Claude Certified Architect (Foundations) Study Companion

Private practice tool for studying for the Claude Certified Architect (Foundations) exam. Two slash commands, for use inside Claude Code:

- `/quiz` — a Socratic, conversational quiz (one question at a time, misconceptions corrected as you go).
- `/test` — a multiple-choice practice exam, matching the real exam's format.

Both track your progress locally (per-person, not shared — see below) and cover all 30 subcategories across the 5 exam domains.

## Setup

1. Clone this repo.
2. Open the folder in Claude Code (`claude` in this directory).
3. Run Claude Code from the repo root — the commands read `data/` relative to your working directory.
4. Run `/quiz` or `/test` — with no arguments, each one picks up wherever you left off.

## Usage

- `/quiz` or `/test` with no arguments — continues from your last incomplete subcategory, one at a time.
- `/quiz <topic>` or `/test <topic>` — jump to a specific category or subcategory by number or name, e.g. `/quiz 3`, `/test agentic loops`.
- Pointing at a whole category (e.g. `/quiz 1`) chains through every remaining subcategory in that category in one sitting, then gives you an overall summary.
- Pointing at one specific subcategory runs just that one.
- Progress files are created automatically on first use — a fresh clone with no `data/*-progress.json` files works fine.
- Once all 30 subcategories are complete, `/quiz` and `/test` offer to replay or reset rather than picking something automatically.

## Attribution

Quiz and exam-simulator content is adapted from [claudecertificationguide.com](https://claudecertificationguide.com) — a great resource, worth visiting directly for the full lessons, additional depth, and to support the people who built it. This tool links back to the relevant lesson page whenever you miss a point, specifically to point you back there.

This is a private tool for personal and small study-group use — it isn't intended for public redistribution. The quiz and exam-simulator content itself remains the property of claudecertificationguide.com and is reproduced here only for private study. No license to redistribute is granted or implied — please don't fork this public or repost the content.

## Notes

- `data/quiz-progress.json` and `data/test-progress.json` are personal, per-clone progress trackers, gitignored so they don't collide between people sharing this repo.
