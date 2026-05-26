# Vault Instructions

You are working inside a personal Obsidian Second Brain Vault.
This vault is the owner's external memory — knowledge base, active projects, and personal system in one place.

## Step 1 — Always read first
1. `CONTEXT.md` — who the owner is, active projects, current priorities
2. `AGENTS/` — available agent personalities (ask which one to load, or the user will tell you)
3. `99_System/AGENT_RULES.md` — write permissions and rules

## Step 2 — Vault structure
```
00_Inbox/        → quick capture, unsorted — process daily
10_Projects/     → active projects, each has own CLAUDE.md + sessions/
20_Knowledge/    → permanent knowledge notes by domain
30_Resources/    → reference PDFs, papers, books
40_Archive/      → finished or paused work
50_PrivateLife/  → plans, goals, thoughts, personal — read only, never write without instruction
99_System/       → templates, rules, agent config
AGENTS/          → agent personalities for this vault
CONTEXT.md       → who the owner is — read every session
CLAUDE.md        → this file
```

## Step 3 — How to pick an agent
The user will either name an agent or describe what they need.
Load the matching file from `AGENTS/`.
If no agent is named, ask: "Which agent should I load?"

## Step 4 — After every session (MANDATORY)
1. Create a session summary using: `99_System/Templates/Session-Summary.md`
2. Save to the correct location:
   - Project work → `10_Projects/[project]/sessions/YYYY-MM-DD-summary.md`
   - Knowledge work → `20_Knowledge/[domain]/sessions/YYYY-MM-DD-summary.md`
   - General → `00_Inbox/YYYY-MM-DD-summary.md`
3. If a new concept was learned → create atomic note in `20_Knowledge/[domain]/`
4. Link new notes to existing notes with `[[wikilinks]]`
5. Suggest updating `CONTEXT.md` if active projects or priorities changed

## Rules
- Never store passwords, API keys, or tokens anywhere in the vault
- Never copy-paste from external sources — always paraphrase in the owner's own words
- Never create notes without links — isolated notes defeat the purpose
- Only write to `20_Knowledge/` when creating a real durable note
- Always suggest next actions at the end of a session
- `50_PrivateLife/` is read-only — never write there without explicit instruction
