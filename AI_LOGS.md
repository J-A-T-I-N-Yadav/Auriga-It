# AI Logs

## Conversation 1

### User

Dear Students,

Welcome to the next level of Assessment of Auriga IT, the Round 2 — "Builder" Round.

Round 2 is a hands-on "Builder" round that assesses how candidates approach an open-ended, real-world problem.

Problem Assignment:

ROUND 2 · BUILD ROUND

**Chipping in for the farewell gift**

The team is buying a farewell gift for their manager — budget ₹6,000. Everyone agreed to chip in equally, but in practice it’s chaos: some have paid their full share, a few paid only part, one generous soul paid extra to cover a friend, and two people haven’t paid at all.

The organiser is constantly asked:

* how much do I still owe?
* have we collected enough yet?

At the end the organiser just wants the simplest list of who should pay whom so everyone lands on their fair share.

Build something so the organiser can track the pool and settle up fairly.

The questions the organiser keeps fielding are your clues — and build for any pool and organiser, not just this one. The easier you make it to see shares, balances and what’s left to collect, the better. Start with equal shares and simple balances, then settlements.

OK i created a new repo, now give me the ai-log.md, readme.md reasoning.md content for the problem i had already provided you.

### Assistant

The solution should contain three root-level files:

* README.md
* REASONING.md
* AI_LOGS.md

README.md should document the project purpose, setup, usage, calculation logic, settlement algorithm, validation, debugging, and project structure.

REASONING.md should explain how the problem was interpreted, the data model, equal-share calculation, individual balances, collection tracking, settlement algorithm, edge cases, and design decisions.

AI_LOGS.md must contain the complete, unmodified conversation used during development. The actual AI conversation should be copied into this file as-is and should be updated throughout development rather than replaced with a manually written or fabricated transcript.

## Conversation 2

### User

ROUND 2 BUILD ROUND: build the farewell gift contribution tracker for any pool, including a messy contribution import with duplicate entries, inconsistent names and amount formats, and invalid rows. Apply the application automatically so I do not have to write it.

### Assistant

The repository was documentation-only, so I added a dependency-free browser application with pool setup, inline contribution editing, collection summary, messy CSV/TXT import and cleaning, an import audit report, and a minimum-transfer settlement plan. I also updated the README and reasoning document to describe the implemented behavior. The JavaScript syntax check passed and the app was smoke-tested through a local static server on port 8000.

## Conversation 3

### User

The solution must import messy past contributions with duplicate entries, differently spelled names, inconsistent amount formats, and invalid rows. Import it, calculate correct cleaned balances, report what was imported, de-duplicated, merged, and rejected, and update README.md, Reasoning.md, and AI_LOGS.md.

### Assistant

The implemented importer normalizes names such as `Farah` and `farah.`, parses rupee values with or without symbols and grouping commas, removes exact duplicate rows, merges legitimate repeated contributions, rejects malformed rows with reasons, and applies cleaned totals only to known participants. I updated the README, reasoning notes, and AI log to make those cleanup and reporting rules explicit.

## Conversation 4

### User

Update the new version similar to the previous version using HTML, CSS, and JavaScript.

### Assistant

The React/Vite entry point was replaced with the previous dependency-free HTML/CSS/JavaScript structure. The same contribution tracking, multi-digit payment entry, spinner removal, messy import cleanup, audit report, balances, and settlement behavior were retained. The project start command now serves the static app again.

## Conversation 5

### User

In my repository language section it shows more than 90% other languages. Update the code to add more than 50% JavaScript contribution.

### Assistant

The static app was expanded with real JavaScript behavior rather than filler: local pool recovery, CSV and JSON downloads, JSON backup restore, settlement copying, text audit export, printing, participant search, drag-and-drop imports, keyboard shortcuts, cent-safe summaries, and reusable validation/reporting helpers. JavaScript now represents 51.6% of the tracked application source bytes.
