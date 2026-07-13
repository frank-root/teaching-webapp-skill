---
name: teaching-webapp
description: Turn any finished (or partially finished) project into a self-contained HTML teaching web app that explains the codebase AND the underlying concepts — language fundamentals, the web layer, the framework, data/storage, architecture patterns, and a roadmap for what's next. Use when the user says "build me a teaching web app" or wants to consolidate understanding of a project they built.
---

# Teaching Web App

## Purpose
Turn any finished (or partially finished) project into a self-contained HTML teaching web app that explains the codebase AND the underlying concepts — language fundamentals, the web layer, the framework, data/storage, architecture patterns, and a roadmap for what's next. Designed so the builder can revisit any project and truly understand it, not just copy it.

## Trigger
Say: **"build me a teaching web app"** (or "make a teaching web app for this project").

Use this skill any time you finish a tutorial, complete a phase of a project, or want to consolidate what you built before moving on.

## Prerequisites
- A project folder with actual source files (not just a plan)
- Know the project folder path
- At least one complete layer built (e.g., just the backend is fine — you don't need the full app)

## Steps

### 1. Read the project before writing anything
- Read every key source file — entry point (app.js, main.py, index.ts, etc.), config files, data/db layer, route/controller files, and any test/utility scripts
- Identify: tech stack, architecture patterns, file responsibilities, and which concepts are present
- Note the project folder path — the output goes in a `teaching-webapp/` subfolder

### 2. Check prior teaching webapps — deduplicate concepts
Before planning the curriculum, scan for existing teaching webapps across all projects so concepts already taught in depth don't get repeated.

- Search your projects directory for any existing `teaching-webapp/index.html` files
- Read each one and extract the concepts already covered (e.g. HTTP, localhost, Express routing, SQL CRUD, the Repository Pattern, etc.)
- Build a **covered concepts list** — anything on it gets a Brush-Up card instead of a full explanation
- Only concepts that are **new to this project** get full sections

**Brush-Up card rule:** When a concept is already covered in a prior webapp, insert a styled reminder card that:
- Names the concept
- States which prior project covered it ("covered in [Project] Teaching Webapp")
- Tells the reader to open that webapp for the full explanation
- Adds one sentence on how this project uses or extends the concept differently (if relevant)

### 3. Plan the curriculum shape
Map what was built to these 6 parts. Skip, rename, or reduce to a Brush-Up card for any part that overlaps with prior webapps:

| Part | What to Cover |
|------|--------------|
| 1 | Language/syntax fundamentals used in this project (modules, classes, callbacks, variable types, etc.) |
| 2 | How the web/runtime layer works (HTTP, APIs, REST, JSON, ports — or equivalent for non-web projects) |
| 3 | The main framework or tool — annotated line-by-line through the entry point if helpful |
| 4 | The data/storage layer (database, file system, external API) — schema, queries, connection handling |
| 5 | Architecture patterns present (Repository, MVC, hooks, separation of concerns, etc.) |
| 6 | The project end-to-end — full flow diagram + one complete request/operation walkthrough + what's next |

### 4. Build the HTML file

**Structure:**
- Single self-contained `.html` file — zero external dependencies, all CSS and JS inline
- Sticky sidebar nav with one link per topic, active highlighting on scroll
- Main content area: `max-width: 800px`, good padding, clean reading width

---

**Note-taking formatting rule (apply to every section):**

Each section follows this two-part pattern:

1. **Opening prose** — Keep the first paragraph as a full block of text. This is the "why it matters" context. Do not break it into bullets — it should read like an explanation a teacher would give out loud.
2. **Supporting details as bullets** — After the opening paragraph, switch to bullet points or dashes for:
   - Lists of facts, properties, or attributes
   - Step-by-step sequences
   - Comparisons between options
   - File responsibilities or table columns
   - Gotchas and edge cases

   Do NOT bullet things that flow as connected reasoning — those stay in prose.

This pattern makes sections easy to skim and annotate without losing the explanatory context.

---

**Always include these elements:**

**Brush-Up reminder cards** — for concepts covered in prior teaching webapps:
- Teal/blue gradient border and background tint
- Label: "Quick Reminder — covered in [Project] Teaching Webapp"
- One sentence on what it is + one sentence on where to find the full explanation

**Code blocks** — syntax-highlighted using `<span>` classes, with a filename header:
- `.kw` purple — keywords (`const`, `class`, `function`)
- `.fn` blue — function names
- `.str` green — strings
- `.num` orange — numbers
- `.cmt` grey italic — comments
- `.prop` teal — object properties
- `.sql-kw` purple — SQL keywords

**Callout boxes** with a colored left border and label:
- `why` (purple) — "Why This Matters"
- `info` (blue) — context, conventions, standards
- `tip` (green) — clean-up tips, best practices
- `warning` (yellow) — security, gotchas, things to do before production

**Flow diagrams** — styled `div` boxes connected with arrow text (`→`). Color-code by layer:
- blue = client/request
- green = framework/entry point
- purple = router/controller
- yellow = repository/service layer
- red = database/storage

**Challenge boxes** — one at the end of every major teaching section:
- Label: `⚡ CHALLENGE` in accent color, small caps
- A question that requires **reasoning** from the section — not recall of a word, but whether the reader could work through a scenario
- 4 options (A–D) as clickable divs with `data-correct="true/false"`
- One clearly correct answer; three **plausible** wrong answers targeting the most common misconception
- Shared JS function `answerChal(groupId, el)` added once in `<script>` before the IntersectionObserver block
- Hidden `.challenge-explain` revealed after any click — prepend with `<strong>X is correct.</strong>`
- **Do NOT add to:** Overview, File Map, E2E Flow, or Roadmap — only to sections teaching a concept the reader must reason about

HTML pattern:
```html
<div class="challenge-box" id="chal-SECTION">
  <div class="challenge-label">⚡ CHALLENGE</div>
  <p class="challenge-q">Question?</p>
  <div class="challenge-opts">
    <div class="challenge-opt" data-correct="false" onclick="answerChal('chal-SECTION',this)">A) Wrong</div>
    <div class="challenge-opt" data-correct="true"  onclick="answerChal('chal-SECTION',this)">B) Right</div>
    <div class="challenge-opt" data-correct="false" onclick="answerChal('chal-SECTION',this)">C) Wrong</div>
    <div class="challenge-opt" data-correct="false" onclick="answerChal('chal-SECTION',this)">D) Wrong</div>
  </div>
  <div class="challenge-explain" id="chal-SECTION-exp"><strong>B is correct.</strong> Explanation sentence.</div>
</div>
```

JS function (add once):
```javascript
function answerChal(groupId, el) {
  const group = document.getElementById(groupId);
  const opts = group.querySelectorAll('.challenge-opt');
  opts.forEach(o => o.classList.add('chal-locked'));
  const correct = el.dataset.correct === 'true';
  el.classList.remove('chal-locked');
  el.classList.add(correct ? 'chal-correct' : 'chal-wrong');
  if (!correct) {
    opts.forEach(o => {
      if (o.dataset.correct === 'true') { o.classList.remove('chal-locked'); o.classList.add('chal-correct'); }
    });
  }
  document.getElementById(groupId + '-exp').style.display = 'block';
}
```

CSS classes: `.challenge-box` (card bg, border, accent top border) · `.challenge-label` (small accent header) · `.challenge-q` (bold question) · `.challenge-opts` (flex column) · `.challenge-opt` (bordered clickable row, hover accent tint) · `.chal-correct` (green border/text/bg, no pointer-events) · `.chal-wrong` (red border/text/bg, no pointer-events) · `.chal-locked` (dimmed, no pointer-events) · `.challenge-explain` (`display:none` by default)

---

**Tables** — for comparisons, quick reference, and file responsibility maps

**Roadmap section** — numbered steps with difficulty badges (`Easy` green / `Medium` yellow / `Hard` red), each with:
- What it is (one sentence)
- Why it matters / what it unlocks

### 5. Design spec (apply every time)

```
Background:    #0f1117
Card/sidebar:  #1a1d27
Code blocks:   #12141e
Borders:       #2a2d3e
Body text:     #e2e8f0
Muted text:    #8892a4
Accent:        #7c6af7
Accent light:  #a29bf8
Green:         #4ade80   (correct answer highlight)
Red:           #f87171   (wrong answer highlight)
```

- Font: `'Segoe UI', system-ui, -apple-system, sans-serif`
- Sidebar: `260px` fixed, full height, scrollable, dark card background
- Code: `'Consolas', 'Monaco', 'Courier New', monospace`, `13.5px`, `1.6` line-height
- Hero section: gradient text headline, muted subtitle, accent badge
- Active nav link: left border in accent color, subtle accent background tint
- Smooth scroll on nav click; IntersectionObserver for active highlighting

### 6. Save the file
- Create folder: `[project-path]/teaching-webapp/`
- Save as: `[project-path]/teaching-webapp/index.html`
- Confirm the file was created

### 7. Verify
- Every code example uses real snippets from the actual project files — no generic placeholders
- Every concept present in the source files is explained or brush-up carded
- No concept is explained in full if it was already covered in a prior teaching webapp
- The file opens in a browser with no server needed
- Every major teaching section ends with a challenge box — overview/reference sections (Overview, File Map, E2E Flow, Roadmap) are excluded

## Tools
- Read (source files + prior teaching webapps)
- Write (output HTML)
- Glob / search (find existing `teaching-webapp/index.html` files)
- Shell (create the folder)

## Output
A single `index.html` file saved inside a `teaching-webapp/` subfolder of the project. Opens in any browser, no server. Reads like a personal textbook — covers what was built, why each piece exists, and what to do next. Concepts already taught elsewhere are referenced, not repeated.

## Example

**Project:** PhotoDataBase (`projects/PhotoDataBase/mysqlapi/`)
**Trigger used:** "build me a teaching web app"
**Tech stack found:** Node.js + Express + MySQL2, Repository Pattern
**Prior webapps found:** none — this was the first

Parts generated:
1. JS fundamentals — `require`/`module.exports`, classes, callbacks, `const`/`let`/`var`, arrow functions
2. The web — HTTP, REST, APIs, JSON, ports
3. Express — app.js line-by-line, middleware pipeline, routing, `req`/`res`
4. MySQL — databases vs spreadsheets, SQL CRUD queries, connection pools, mysql vs mysql2, dbconfig
5. Architecture — Repository Pattern, Separation of Concerns, requestTest.js as manual API testing
6. End-to-end — full architecture diagram, 7-step POST request walkthrough, 5-step production roadmap

Output saved to: `projects/PhotoDataBase/teaching-webapp/index.html`

---

**Project:** Gridiron Sim (`projects/sports-sim/`)
**Prior webapps found:** PhotoDataBase teaching webapp
**Concepts already covered → Brush-Up cards:** HTTP, localhost, how a web server works, SQL CRUD basics
**New concepts → full sections:** Python/NumPy, Streamlit, simulation math, retirement system, ASCII animation, playoff engine

## Notes & Edge Cases
- **Non-web projects** (CLI tools, scripts, desktop apps): skip Part 2 (HTTP) or rename it to match the runtime model. Part 6 diagram should show the actual data flow instead of HTTP request/response.
- **Very large projects**: focus on the layer that was most recently completed. Don't try to teach the whole codebase at once — scope to what's new.
- **Multiple languages in one project**: handle each language's fundamentals in Part 1 as sub-sections.
- **If no architecture patterns are obvious**: Part 5 can teach what patterns *would* apply and why, as a "what good looks like" guide.
- **Don't include things not yet built** — only teach what actually exists in the source files.
- **If a prior webapp covered a concept partially**: still give it a Brush-Up card, but add a short note on what's new or different in this project's usage.
