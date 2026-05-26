# Obsidian Second Brain

A personal knowledge management system built on Obsidian — designed to work with AI agents, automation, and your existing tools. Local-first, plaintext, agent-ready from day one.

---

## How to use this for yourself

### Step 1 — Clone or download this repo
Download the ZIP or clone the repository. This gives you the complete folder structure ready to use.

### Step 2 — Open the folder as an Obsidian Vault
Open Obsidian → "Open folder as vault" → select the downloaded folder.

### Step 3 — Fill in CONTEXT.md
Open `CONTEXT.md` and fill in your own information:
- Your name and background
- Your active projects
- Your current learning topics
- What you want agents to know about you

This is the most important file — every agent reads it at the start of every session.

### Step 4 — Create your first agent
Copy `AGENTS/Agent-Template.md`, rename it, and fill in each section:
- What domain does this agent cover?
- What tone and personality?
- Which folders does it read at startup?
- What can it write to?

### Step 5 — Start a session with any AI tool
With Claude Code (launched from the vault root):
```
You are [AgentName]. Read CLAUDE.md and start.
```

With any other tool (Codex, Hermes, etc.):
```
Read CLAUDE.md, you are [AgentName].
```

### Step 6 — Let the vault grow
After every session the agent writes a session summary to `sessions/` and creates new knowledge notes in `20_Knowledge/`. The vault builds itself over time.

---

## What to keep in mind

- **Fill CONTEXT.md first** — without it agents have no personal context and work generically
- **One concept per note** — atomic notes that link to each other are more valuable than long documents
- **Use the Inbox** — capture everything in `00_Inbox/TODO.md` without organizing, process once a day
- **Private content stays private** — `50_PrivateLife/` is never touched by agents unless you explicitly ask
- **The .gitignore protects you** — `10_Projects/`, `20_Knowledge/`, `00_Inbox/`, and `50_PrivateLife/` are excluded from git by default so your notes never accidentally get pushed

---

## Folder structure

```
Obsidian Vault/
├── CLAUDE.md              ← Agent entry point — read by all tools
├── CONTEXT.md             ← Fill this with your information
├── AGENTS/                ← Your agent personality files
├── 00_Inbox/              ← Daily capture, process in 5 min
├── 10_Projects/           ← Active projects with sessions/
├── 20_Knowledge/          ← Permanent knowledge by domain
├── 30_Resources/          ← Papers, books, PDFs
├── 40_Archive/            ← Finished work
├── 50_PrivateLife/        ← Goals, plans, personal
└── 99_System/             ← Templates and rules
```

---

## Full documentation

Complete design philosophy, agent architecture, n8n automation, Notion integration, and lessons learned:

→ [DOCUMENTATION.md](./DOCUMENTATION.md)

---

*Built on Obsidian, plain Markdown, and the idea that knowledge compounds.*
