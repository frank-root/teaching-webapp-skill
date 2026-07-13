# Teaching Web App — a Claude Code skill

Turn any project you've built into a **self-contained HTML teaching web app**: a single `index.html` that explains your codebase *and* the concepts underneath it — language fundamentals, the web layer, the framework, the data layer, architecture patterns, and a roadmap for what to build next.

Built for learners who ship real projects but want to **understand and reproduce** the work, not just have it. Each webapp reads like a personal textbook chapter for that project, complete with interactive multiple-choice challenges to test whether you actually got it.

## What the output looks like

- A single dark-themed `index.html` — no build step, no server, no dependencies. Double-click it and it opens.
- Sticky sidebar navigation with scroll-based active highlighting
- Syntax-highlighted code blocks using **real snippets from your actual project files** — never generic placeholders
- Color-coded flow diagrams showing how a request/operation moves through your architecture
- Callout boxes: *Why This Matters*, tips, gotchas, and pre-production warnings
- ⚡ **Challenge boxes** at the end of every teaching section — 4-option reasoning questions with instant feedback and explanations
- A difficulty-badged roadmap of what to build next

### The Brush-Up system

The skill's key idea: it **never re-teaches a concept**. Before writing anything, it scans your other projects for previously generated teaching webapps and builds a list of concepts already covered. Anything on that list gets a short "Quick Reminder" card pointing back to the webapp that taught it fully — so your third project's webapp only teaches what's *new* in that project, and the set of webapps grows into a personal curriculum instead of a pile of repetitive tutorials.

## Install

Copy `SKILL.md` into a skill folder for [Claude Code](https://claude.com/claude-code):

```bash
# available in every project
mkdir -p ~/.claude/skills/teaching-webapp
cp SKILL.md ~/.claude/skills/teaching-webapp/

# or just one project
mkdir -p .claude/skills/teaching-webapp
cp SKILL.md .claude/skills/teaching-webapp/
```

## Use

Open Claude Code in (or point it at) a project with real source files, then say:

> **build me a teaching web app**

Claude reads your source files, checks prior teaching webapps for already-covered concepts, plans a 6-part curriculum, and writes `teaching-webapp/index.html` inside the project folder.

Works best when you've just finished a tutorial, completed a phase of a project, or want to consolidate what you built before moving on. Non-web projects (CLI tools, scripts) work too — the skill adapts the "web layer" section to match the runtime model.

## The 6-part curriculum

| Part | Covers |
|------|--------|
| 1 | Language/syntax fundamentals used in this project |
| 2 | The web/runtime layer (HTTP, REST, JSON, ports — or the CLI/script equivalent) |
| 3 | The main framework or tool, annotated through the entry point |
| 4 | The data/storage layer — schema, queries, connections |
| 5 | Architecture patterns present (Repository, MVC, separation of concerns…) |
| 6 | End-to-end flow diagram, one full operation walkthrough, and a roadmap |

Parts that overlap with a prior webapp shrink to Brush-Up cards automatically.

## License

MIT
