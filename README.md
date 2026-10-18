# Topic Mastery Quiz

A Claude skill that turns any topic into a **50-question teach-then-test course**, delivered as a single interactive HTML page.

Every question teaches something. Each one has a short concept card (summary, code, key points) shown before an MCQ. After you answer, it explains why the right answer is right, why each wrong option is wrong, a hidden fact or gotcha, and a one-line recap.

## Courses built with it

| Course | Level | Open |
| --- | --- | --- |
| Android (Kotlin) | Experienced, interview-prep depth | [Open the course](https://dhruv-d02.github.io/topic-mastery-quiz/android/) |
| RAG: Retrieval-Augmented Generation (Python) | Basic experience | [Open the course](https://dhruv-d02.github.io/topic-mastery-quiz/rag/) |
| MCP: Model Context Protocol (Python, JSON) | Beginner starting real work; based on spec 2026-07-28 and Python SDK 2.x | [Open the course](https://dhruv-d02.github.io/topic-mastery-quiz/mcp/) |

All courses are also listed at [dhruv-d02.github.io/topic-mastery-quiz](https://dhruv-d02.github.io/topic-mastery-quiz/).

## What the skill does

1. Asks the topic, your current level, your goal (concepts, hands-on or interview prep) and the code language.
2. Plans a curriculum of 50 distinct concepts in learning order, grouped into modules: 15 easy, 20 medium, 15 hard.
3. Writes each item: concept card → MCQ with realistic distractors → explanation, why-wrong lines, hidden fact and recap.
4. Builds one self-contained HTML page with:
   - progress and live score
   - filters by difficulty and module
   - a recap screen after each level
   - a results page with weak areas and a cheat sheet of all 50 recaps
   - retry-wrong-answers mode, saved progress, keyboard shortcuts, dark mode and phone layout
5. Checks question counts, answer keys and answer-position spread before delivering.

## Install the skill

The skill lives in [`skills/topic-mastery-quiz/SKILL.md`](skills/topic-mastery-quiz/SKILL.md).

- **Claude (claude.ai / desktop app):** zip the `topic-mastery-quiz` folder and upload it as a custom skill in Claude's settings.
- **Claude Code:** copy the folder into `~/.claude/skills/` (personal) or `.claude/skills/` in a project.

Then ask something like: *"Teach me Kubernetes with a 50-question mastery quiz."*

## Repository layout

```
skills/topic-mastery-quiz/SKILL.md   the skill
android/index.html                   Android course (served by GitHub Pages)
rag/index.html                       RAG course
mcp/index.html                       MCP course
index.html                           list of courses
```

Courses on fast-moving topics state the version they were written against (for example, the MCP course covers spec 2026-07-28 and also explains the older `initialize` handshake you'll still meet in existing servers).

To add a course, generate it with the skill and save it as `<topic>/index.html`, then add a row to the table above and a link in `index.html`.

Progress in each course is saved in your browser only, so it doesn't sync between devices.
