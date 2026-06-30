# C++ Interview Course — Project Context

> This file is read automatically by Claude Code at the start of every session.
> It exists so a fresh session inherits the full context and history of this project
> without the user having to re-explain anything.

## What this is

A single-file, self-hosted **10-week C++ interview-prep course**. `index.html` is the
entire app — HTML, CSS, and vanilla JS all inline. No build step, no framework, no
backend. It works offline and is meant to be hosted as a static site (e.g. GitHub Pages).

**Audience:** an experienced C++ / game-engine developer brushing up the LeetCode /
interview-pattern skill for software-engineering interviews. The framing throughout is
deliberately supportive and direct: assume real engineering competence, and treat
LeetCode as a separate, artificial skill — not a measure of ability.

## How it came to be (the history)

Built collaboratively with Claude over several iterations. The "why" behind the current
shape, in order:

1. Started as a request for a personal "Grokking the Coding Interview"–style guide:
   a C++ **syntax primer first** (because `vector<vector<int>>` etc. looked intimidating),
   then a lesson per pattern with easy/medium/hard LeetCode links, plus a colorful cheat sheet.
2. Expanded into a deeper **course** with real written lessons, diagrams, worked code, and
   topics beyond data structures (Big-O, C++ language deep dives, system design, behavioral).
3. Rebuilt as a **multi-page app** (hash router — each lesson is its own page) with the C++
   language lessons **interleaved among the patterns** in a deliberate "learn the concept,
   then immediately use it" order. Notably: a **Pointers & Memory lesson placed before the
   Two Pointers pattern**; amortized complexity added to Big-O; per-device progress saving.

**Core pedagogical decision:** concept-before-application, interleaved. Pointers → Two
Pointers / Linked Lists; memory & ownership → the data structures that lean on them.

## Architecture (how `index.html` is built)

Everything is data-driven by one content array rendered through a small engine:

- **`<style>`** — custom CSS, design tokens in `:root`. No framework. Fonts (Space Grotesk /
  Inter / JetBrains Mono) load from Google Fonts CDN with system fallbacks.
- **`highlight()`** — a tiny regex syntax highlighter for C++ code blocks (intentionally simple).
- **`SVG{}`** — inline SVG diagram strings keyed by name: `bigo`, `pointer`, `heap`, `copyref`,
  `hash`, `stack`, `binsearch`, `raii`, `move`.
- **`course[]`** — the content model. Each lesson is `{group, num, id, color, title, badge, blocks[], demo?}`.
- **Block engine** — `renderBlocks()` turns typed block objects into HTML. Block types:
  `p`, `h` (subhead), `ul`, `code`, `cue`, `cx` (complexity), `note`, `idea`, `pit` (pitfall),
  `svg`, `worked` (annotated example), `demoslot`, `probs`, `primer`, `cheat`.
- **`primerData` / `cheatData`** — the syntax-primer and cheat-sheet content.
- **Hash router** — `route()` / `renderHome()` / `renderLesson()`. Nav links are `#/<id>`;
  browser back/forward and bookmarking work; one lesson renders at a time.
- **Progress** — solved problems are checkboxes keyed by LeetCode slug, persisted in
  `localStorage` under `cpp-course-progress-v1`; the sidebar ring shows total solved.
- **Demos** — `initTwoPointers()` and `initSlidingWindow()` (animated, with Run/Reset).

**To add or edit a lesson:** edit the `course[]` data (add blocks) — not the renderer.
**To add a diagram:** add an entry to `SVG{}` and reference it with an `svg("key","caption")` block.

## Content state — what's deep vs. what's left

**Fully written** (deep: prose + diagram + worked annotated code + pitfalls + problems):
Orientation (how interviews work), Big-O & Complexity (incl. amortized), Pointers & Memory,
Value vs Reference Semantics, the Syntax Primer, and the patterns **Arrays & Hashing, Two
Pointers, Sliding Window, Stack, Binary Search**; plus C++ deep dives **RAII & Smart Pointers**
and **Move Semantics**; plus **System Design Basics** and **Behavioral & STAR** intros; plus the Cheat Sheet.

**Concise — still to expand** (currently just a "spot it when" cue + short intuition +
complexity + problems, and flagged in the UI with a "○ Concise lesson — ask me to expand" pill):
**Linked Lists, Trees, Tries, Heaps, Backtracking, Graphs, Dynamic Programming, Intervals.**

## House style (match this when expanding lessons)

Each full pattern lesson should have, in roughly this order:
- a **`Spot it when:`** recognition cue,
- a written explanation of **how it actually works** (the intuition, not just the API),
- at least one **SVG diagram**,
- a **worked, annotated C++ example**,
- common **pitfalls**,
- a one-line **complexity** note,
- **3 LeetCode problems** (easy / medium / hard) linked as `https://leetcode.com/problems/<slug>/`.

C++ in examples should be modern and idiomatic (pass containers by `const&` to avoid copies,
use `auto`, structured bindings). Tone: concrete, supportive, explain the *why*. Keep CSS/JS
inline in the single file unless we deliberately decide to split it.

## Constraints / gotchas

- Keep it a **single self-contained `index.html`** so it stays trivially hostable, unless we
  consciously choose to refactor into multiple files.
- `localStorage` progress is fine on a real host but **won't sync across devices** (it's per-browser).
- Only external network dependency is the Google Fonts stylesheet; everything else is inline.
- Code blocks live inside JS template literals; `<`, `>`, `&` are fine there and get escaped at render time.

## Tasks / next steps

- [ ] Run it locally and preview (any static server, e.g. `python3 -m http.server`).
- [ ] `git init`, commit, deploy to **GitHub Pages**, and share the live URL.
- [ ] Expand the 8 concise lessons into full depth (see "Content state" + "House style").
- [ ] Optional later: a full System Design worked example (URL shortener end-to-end);
      2–3 STAR behavioral stories; more C++ deep dives (templates/STL, concurrency);
      possibly split the single file into modules once it grows.

## Note

Personal/private details were intentionally left out of this file, since the repo may be
public. The course's framing is kept generic on purpose ("you ship real C++ in a game engine").
