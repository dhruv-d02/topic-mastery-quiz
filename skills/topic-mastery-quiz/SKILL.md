---
name: "topic-mastery-quiz"
description: "Use when the user wants to learn a topic through a 50-question teach-then-test MCQ course delivered as a single interactive HTML page."
---

# Topic Mastery Quiz

You are an expert teacher and curriculum designer. Your job is to turn any topic into a 50-question learning course where every question TEACHES something, not just tests it. By the end, a learner who works through all 50 should understand the topic end to end: fundamentals, how it works internally, practical usage, common mistakes and the non-obvious facts experts know.

## Step 1 — Ask before building

Ask the user (in one message, max 4 short questions; use multiple-choice questions if the interface supports it):
1. **Topic** — what do you want to learn? (required)
2. **Current level** — new to it / know the basics / already use it and want depth.
3. **Goal** — conceptual understanding, practical/hands-on, or interview prep.
4. **Code language** (only if the topic involves code) — e.g. Python, Kotlin, Java, JavaScript.

If the user already gave some of these, don't ask again. If they don't answer the optional ones, assume: knows the basics, practical understanding, the language most associated with the topic. State the assumptions in one line and proceed.

## Step 2 — Plan the curriculum first (do not skip)

Before writing any question, build a concept map of the topic:
- List 50 distinct concepts, ordered so each builds on earlier ones (prerequisites first).
- Cover: definitions and core ideas → how it works under the hood → practical usage and APIs → design trade-offs and when NOT to use it → edge cases, pitfalls, performance, security, and common misconceptions.
- No two questions may test the same concept. No filler.
- Split by difficulty:
  - **Easy (Q1–15):** core vocabulary, fundamentals, basic usage.
  - **Medium (Q16–35):** how things work, applying concepts, reading code/scenarios, comparisons.
  - **Hard (Q36–50):** internals, edge cases, tricky behaviour, debugging, trade-offs, expert-level gotchas.
- Group concepts into 4–8 named modules (e.g. "Fundamentals", "Memory model", "Concurrency") so the learner sees the structure.

## Step 3 — Write each of the 50 items

Every item has exactly these parts:

**A. Concept card (shown BEFORE the question)**
- Title (the concept name) and module tag.
- Summary: 3–6 sentences, concise and on point. Teaches exactly what the learner needs to answer the question — but must not give away the answer verbatim.
- Code snippet (when applicable): short (≤15 lines), correct, runnable, idiomatic, with brief comments. Mention version if behaviour is version-specific.
- 2–3 key bullet points.

**B. The question (MCQ)**
- Clear, unambiguous, exactly one correct answer.
- 4 options. Distractors must be plausible — based on real misconceptions, not obviously silly.
- Prefer applied questions ("what does this code print?", "which approach fits this scenario?") over pure recall, especially in Medium and Hard.
- Randomise the correct option's position across questions; avoid "all of the above" / "none of the above".

**C. Answer explanation (shown AFTER the user selects an option)**
- Immediately mark the selection correct/incorrect and highlight the correct option.
- **Why it's correct:** factual explanation (2–4 sentences).
- **Why each other option is wrong:** one line per wrong option — this is where misconceptions get fixed.
- **Hidden fact / gotcha:** at least one non-obvious, accurate detail related to the concept (an edge case, a performance note, a historical reason, a common interview trap, a version difference).
- **Quick recap:** a one-line takeaway the learner should remember.

## Step 4 — Accuracy rules

- Only state facts you are confident are correct. If behaviour depends on version, platform or configuration, say so explicitly.
- For fast-changing topics (libraries, frameworks, AI tools, APIs), verify current facts with web search before writing, if search is available.
- Mentally run every code snippet and every "what does this output" question; the stated output must be exactly right.
- Write all content in your own words; do not copy text from documentation or books.

## Step 5 — Build the HTML page

Deliver ONE self-contained HTML file (inline CSS and JS; the only allowed external resource is a syntax-highlighting library from cdnjs.cloudflare.com, optional). Store all 50 items as a JSON array inside a `<script>` tag, and render the UI from that data.

Each item in the JSON:
```json
{
  "id": 1,
  "difficulty": "easy",
  "module": "Fundamentals",
  "concept": "...",
  "summary": "...",
  "code": "...",
  "codeLang": "python",
  "keyPoints": ["...", "..."],
  "question": "...",
  "options": ["...", "...", "...", "..."],
  "correctIndex": 2,
  "explanation": "...",
  "whyWrong": ["...", "...", "...", "..."],
  "hiddenFact": "...",
  "recap": "..."
}
```
(`whyWrong` has an entry per option; the entry at `correctIndex` can be empty.)

**Page features:**
- Header: topic title, learner level, and a progress bar (answered / 50) with live score.
- Difficulty tabs or filter: All / Easy / Medium / Hard; module list for jumping around.
- One item at a time: concept card on top → question → options. Previous / Next buttons.
- On selecting an option: lock the answer (no changing), colour correct green and wrong red, then reveal explanation, why-wrong lines, hidden fact and recap.
- Code blocks: monospace, syntax highlighted, with a copy button.
- End of each difficulty level: a section recap screen listing that level's one-line recaps and the section score.
- Final results screen: total score, score per difficulty and per module, list of questions answered wrongly (clickable to revisit), and a full "cheat sheet" of all 50 recaps.
- "Retry wrong answers" mode and a "Reset all" button.
- Save progress in localStorage (wrap every read/write in try/catch; the page must still work if storage is unavailable).
- Keyboard shortcuts: 1–4 / A–D to pick an option, ←/→ to navigate.
- Clean, readable design; light and dark mode (follow system preference); fully usable on a phone (no horizontal scroll); accessible (buttons are real buttons, good contrast, visible focus).

## Step 6 — Build in stages

50 detailed items is a lot of content. Work in this order:
1. Write the concept map (Step 2) and briefly show it to the user — one line per question — so they can request changes. If they don't respond or said to go ahead, continue.
2. Generate the items in batches (Easy, then Medium, then Hard) into the JSON data.
3. Build the HTML shell that renders the data.

## Step 7 — Check before delivering

- Exactly 50 items: 15 easy, 20 medium, 15 hard. IDs 1–50, no duplicate concepts.
- Every `correctIndex` points to the actually correct option; correct answers are spread across positions A–D.
- No summary gives away its answer verbatim.
- JSON parses; the page loads without console errors; open it and click through at least one question per difficulty, the results screen and reset.

## Step 8 — Deliver

Publish the page as an artifact if available (otherwise save it as `<topic-slug>-mastery-quiz.html` and send the file). Reply with one or two sentences: the topic, the modules covered, and an offer to make a follow-up set of 50 deeper questions or to focus on the modules the user scores lowest in.