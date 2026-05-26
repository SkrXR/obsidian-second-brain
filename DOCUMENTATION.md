# Obsidian Second Brain — System Documentation

> A production-grade personal knowledge management system built on Obsidian, integrated with AI agents, n8n automation, and Notion. Designed for deep technical learners who need a trusted external memory that grows with them.

---

## Executive Summary

This document describes the design, architecture, and implementation of a custom Obsidian Second Brain system built for a self-directed technical learner working across multiple demanding domains simultaneously. The system externalizes knowledge, active projects, personal goals, and agent workflows into a single local-first vault that is accessible to multiple AI tools — Claude Code, OpenAI Codex, Hermes, and others — through a unified entry point. Rather than treating AI as a black box, the system treats it as a collaborator that reads from and writes to a structured knowledge base, producing durable notes after every session. The result is a compounding knowledge system where every learning session leaves a permanent, searchable, interlinked trace.

---

## Introduction and Vision

### The Problem

Most knowledge workers and learners accumulate information continuously but retain very little of it structurally. Notes pile up in disconnected apps, browser bookmarks go unvisited, and the understanding built during a deep learning session evaporates by the following week. For someone working across multiple demanding technical domains simultaneously, this fragmentation is not just inefficient — it actively prevents the cross-domain pattern recognition that produces real expertise.

### Why Obsidian

Obsidian was chosen over alternatives including Notion, Roam Research, and LogSeq for four non-negotiable reasons. First, files are stored as plain Markdown on the local filesystem — there is no proprietary format, no vendor lock-in, and no subscription required to access your own notes. Second, the graph view reflects the actual link density between ideas and makes knowledge gaps visible at a glance. Third, Obsidian's folder structure is fully transparent to external tools — Claude Code, Codex, and Hermes can all read and write Markdown files directly without API integration. Fourth, the plugin ecosystem allows the vault to be extended without leaving the tool.

Notion remains in the system but serves a different role: structured databases, task tracking, and daily planning. Obsidian holds knowledge. Notion holds logistics.

### The Vision

The goal was to build a system with the following properties: any AI agent launched inside the vault root should immediately understand who the user is, what projects are active, what knowledge already exists, and where new notes belong. Sessions should leave durable artifacts. Knowledge from one domain should visibly connect to knowledge in another. And the system should require minimal daily maintenance while compounding in value over months and years.

---

## Core Design Principles

### Local-First, Plaintext

Every note is a `.md` file on the local filesystem, synchronized across devices via Google Drive. There is no database, no proprietary schema, and no cloud dependency for read access. This means any tool that can read a file can read the vault. It also means the vault survives tool changes — if Obsidian ceased to exist tomorrow, every note would remain accessible in any text editor.

### Atomic Notes and the Zettelkasten Method

The Zettelkasten principle governs how knowledge is written: one concept per note, written in the author's own words, always linked to related concepts with explicit `[[wikilinks]]`. This forces genuine understanding — you cannot link two concepts without knowing why they are related. It also produces a graph that reflects actual knowledge structure rather than folder taxonomy.

Four note types are distinguished:

- **Fleeting notes** — raw captures that live in `00_Inbox/` and are processed within 24 hours
- **Literature notes** — summaries of external sources that always reference the original
- **Permanent notes** — durable insights encoded in the author's own words
- **Maps of Content (MOCs)** — navigational notes that link related permanent notes without containing original knowledge

### Separation of Knowledge and Logistics

A common failure mode in personal knowledge management is using the same tool for both thinking and task management. In this system the boundary is explicit: Obsidian holds knowledge, ideas, research, and project documentation. Notion holds deadlines, homework trackers, monthly goals, and to-do lists. An automated daily planner bridges both by reading Notion task state and vault project state together each morning.

### Agent-Readability as a First-Class Concern

Every structural decision in this vault was made with one question in mind: if an AI agent reads this, will it understand what to do? The `CLAUDE.md` file in the vault root is the universal entry point. The `CONTEXT.md` file provides personal context. Agent personality files in `AGENTS/` define behavior without being tool-specific. This design means the vault works identically with Claude Code, Codex, and local models — the agent reads the same files regardless of which tool is running it.

---

## Folder and Vault Architecture

### Structure Overview

```
Obsidian Vault/
├── CLAUDE.md                    ← Universal agent entry point
├── CONTEXT.md                   ← Personal context for all agents
├── README.md                    ← Human-readable vault overview
│
├── AGENTS/
│   ├── Agent-1.md               ← Domain-specific agent personality
│   ├── Agent-2.md
│   ├── Agent-3.md
│   └── Agent-4.md
│
├── 00_Inbox/
│   ├── TODO.md                  ← Spontaneous one-line captures
│   └── Termine.md               ← Deadlines and appointments
│
├── 10_Projects/
│   └── [domain]/
│       └── [project-name]/
│           ├── CLAUDE.md        ← Project-level agent instructions
│           ├── AGENTS.md        ← Project-specific agent rules
│           ├── docs/
│           │   ├── architecture.md
│           │   ├── product-spec.md
│           │   ├── roadmap.md
│           │   └── progress.md
│           └── sessions/        ← Session summaries, dated
│
├── 20_Knowledge/
│   ├── _Index.md                ← Master knowledge index
│   └── [domain]/                ← One folder per knowledge domain
│       └── sessions/            ← Domain learning session summaries
│
├── 30_Resources/
│   ├── Papers/                  ← PDFs of research papers
│   └── Books/                   ← Reference books by category
│
├── 40_Archive/                  ← Completed or paused projects
│
├── 50_PrivateLife/
│   ├── Plans/
│   ├── Thoughts/
│   ├── Challenges/
│   └── Personal/
│
└── 99_System/
    ├── AGENT_RULES.md
    ├── How-I-Use-Obsidian.md
    └── Templates/
        └── Session-Summary.md
```

### Folder Logic

The numeric prefix system enforces sort order independent of alphabetical naming. Folders used daily (`00_Inbox`, `10_Projects`) appear at the top. Infrastructure folders used rarely (`99_System`) appear at the bottom. The gap between numbers — 00, 10, 20 rather than 1, 2, 3 — reserves space for future folders without requiring renaming.

**`00_Inbox`** is the capture layer. Nothing is organized here. Everything is processed within 24 hours. `TODO.md` holds spontaneous one-line thoughts. `Termine.md` holds deadlines.

**`10_Projects`** holds active work. Each project has its own subfolder with at minimum a `CLAUDE.md` for agent instructions and a `sessions/` folder for session summaries. Projects that grow beyond a single file get a `docs/` subfolder mirroring professional software project structure.

**`20_Knowledge`** is the permanent knowledge store organized by domain, not by source or format. A note about a technical concept lives in its domain folder whether it came from a paper, a video, or a lab experiment.

**`30_Resources`** holds reference material that is consulted but not rewritten — PDFs of papers and books. These files are linked from knowledge notes but contain no original synthesis.

**`50_PrivateLife`** holds personal material: monthly goals, identity documents, motivational notes, habit challenges. Agents read this folder for context but do not write to it without explicit instruction.

**`99_System`** holds templates, agent rules, and system documentation. It is edited rarely and never during active work sessions.

---

## Note Types and Templates

### Session Summary

The most important template in the system. Every substantive working session produces a session summary stored in the relevant `sessions/` folder. The template enforces eight fields:

```markdown
# Session Summary — {{date}} — {{agent}} — {{project}}

## 1) Context
- Agent:
- Goal of this session:
- Continues from: [[previous session]]

## 2) What I learned

## 3) Problems solved

## 4) Decisions made

## 5) New notes created
- [[Note]] — brief description

## 6) Next actions
- [ ]

## 7) Open questions

## 8) Links to existing notes
- [[Existing Note]] — why relevant
```

The `Continues from` field is critical — it creates a linked chain of sessions so any agent starting a new session can read backward through the history of a project without loading the entire session log.

### Permanent Knowledge Note

Permanent notes follow the Zettelkasten format with a consistent structure:

```markdown
---
tags: [#domain, #type]
created: YYYY-MM-DD
source: optional — link to literature note or PDF
---

# Concept Title

3–5 sentences explaining the concept in the author's own words.
No copy-paste from sources. No summaries of summaries.

## Links
→ [[Related Concept]] — one sentence explaining the connection

## My Perspective
A personal observation, question, or implication.
```

The `My Perspective` section is not optional decoration. It is the field that transforms a literature summary into genuine knowledge by forcing the author to take a position. A note without a perspective is a copy, not a contribution to the knowledge base.

### Map of Content (MOC)

MOCs are pure navigation. They contain no original knowledge — only organized links to permanent notes in a domain. The vault has two levels: domain MOCs that cover one knowledge area, and a master index that links all domains. MOCs are updated by agents after sessions that produce new permanent notes.

---

## Growth Mechanism

### Daily Workflow

The daily workflow has three components. In the morning, the planning agent reads current context, the Notion task state, and recent session summaries to produce a prioritized daily plan. During the day, any thought, link, or question goes into `00_Inbox/TODO.md` as a single line — no categorization, no formatting. At the end of a session, the relevant agent writes a session summary and any new permanent notes.

Once per day the inbox is processed in five minutes: resolved items are deleted, remaining tasks are kept, and anything substantial becomes either a permanent note or a project action item.

### Note Lifecycle

A note moves through four stages. A fleeting note begins as a line in `TODO.md`. If it survives the daily review it becomes a stub note with a title and a few sentences. If the concept proves durable the stub is moved to the correct `20_Knowledge/` subfolder, fleshed out with a perspective section, and linked to related notes — becoming a permanent note. Over time, permanent notes that receive many incoming links become hubs — the large nodes visible in the graph view.

### Linking Strategy

Links are created actively during writing, not retroactively during review. Every time a concept is mentioned that has its own note, it gets linked. This produces organic link density without requiring a dedicated linking session. The graph view makes it immediately visible when a domain is isolated — a signal that cross-domain thinking is not happening.

---

## AI Agent Integration

### Design Philosophy

Agents in this system are collaborators, not replacements. The fundamental constraint that governs every agent rule is: the agent assists thinking, it does not substitute for it. An agent that simply produces answers on demand produces no durable knowledge. An agent that asks clarifying questions, challenges assumptions, and writes structured notes after a session produces a knowledge base that grows with every interaction.

### Universal Entry Point

Every agent — regardless of which tool runs it — begins a session by reading `CLAUDE.md` in the vault root. This file defines the vault structure, the startup sequence, and the mandatory post-session actions:

```markdown
# Vault Instructions

## On startup always
1. Read CONTEXT.md
2. Ask which agent to load, or read user's instruction
3. Load AGENTS/[AgentName].md
4. Read relevant project files if applicable

## After every session always
1. Create session summary using template
2. Create new permanent notes for learned concepts
3. Link new notes to existing ones
4. Update CONTEXT.md if active projects changed
```

### Agent File Schema

Each agent is defined in a standalone Markdown file in `AGENTS/`. Every agent file follows the same six-field schema:

**Personality** — tone, communication style, level of directness. This is not decoration — it determines whether the agent behaves like a demanding senior colleague or a patient tutor, and the choice is intentional for each domain.

**Knowledge Base** — which folders the agent reads at session start. An agent that reads the relevant knowledge domain at startup arrives knowing what the user already understands, without the user re-explaining context every session.

**Behavior** — explicit rules including what the agent must always do and what it must never do. These rules encode lessons learned from real usage.

**Communication** — language, response length, formatting preferences.

**Write Permissions** — explicit list of folders the agent is authorized to write to. This prevents agents from writing notes to the wrong location and keeps the vault structure stable.

**Trigger Phrases** — phrases that signal the user wants this agent, allowing the dispatcher to infer the correct agent without explicit naming.

### Example Agent Rule Structure

```markdown
# AgentName

## Personality
[Tone, style, level of challenge — specific to this agent's role]

## Knowledge Base (read at startup)
- CONTEXT.md
- 20_Knowledge/[relevant domain]/
- 10_Projects/[relevant project]/

## Behavior
- [What this agent always does]
- [What this agent never does]
- [How it handles specific situations]

## Communication
- Language:
- Tone:
- Response length:

## Write Permissions
- 10_Projects/[domain]/sessions/
- 20_Knowledge/[domain]/
- 00_Inbox/

## After every session
- [Specific summary and note-creation requirements]
```

### Agent Invocation

Any agent tool is invoked with a single line:

```
Read CLAUDE.md, you are [AgentName].
```

Claude Code reads `CLAUDE.md` automatically at startup when launched from the vault root. For other tools, the `CLAUDE.md` content is set as the system prompt in the tool's configuration once, making all subsequent sessions automatic.

---

## n8n Automation Workflows

### Why n8n

n8n was chosen over alternatives including Zapier and Make for two reasons. It runs self-hosted, meaning workflow logic and data never leave the user's infrastructure. And it exposes a Code node that accepts JavaScript, making arbitrarily complex transformation logic possible inline without a separate deployment.

The core principle governing automation in this system: automation handles repetitive logistics, the user handles strategic thinking. No workflow makes decisions. Every workflow either moves information from one place to another or creates a draft that the user reviews before any action is taken.

### Content Curation Workflow

The content curation workflow fires on a daily schedule and reads RSS feeds from curated sources in a target domain. A Code node checks each article against a Notion database to prevent duplicate processing. Articles that pass are sent to a Claude AI node that generates platform-adapted post drafts. The drafts are sent to a Telegram bot with inline approve/reject buttons. Only after explicit approval does the workflow proceed to posting. Notion receives a log entry regardless of the approval decision.

The Telegram approval gate is not a fallback — it is the design. Autonomous posting without human review would undermine credibility. The approval step costs thirty seconds and eliminates that risk entirely.

### Daily Planning Workflow

A second workflow bridges Notion and Obsidian for daily planning. It reads the Notion homework calendar and task database each morning, formats open items as a structured Markdown block, and writes them to the vault's `00_Inbox/Termine.md`. When the planning agent starts its session, it reads this file and incorporates Notion data into the daily brief without the user manually copying between applications.

---

## Notion Integration

### The Boundary

The boundary between Obsidian and Notion is defined by a single question: does this information need to be queried as structured data, or does it need to be thought about and connected? Structured queries belong in Notion. Thinking, research, and knowledge belong in Obsidian.

### What Lives in Notion

Notion holds four categories: a homework tracker database with due dates and completion status, a monthly goals database with progress fields, a flat task list with priority tags, and daily log entries. None of this would benefit from being in Obsidian — it requires filtering, sorting, and date queries, not linking and synthesis.

### What Lives in Obsidian

Obsidian holds everything that benefits from being connected to other knowledge: concepts, project documentation, session summaries, personal frameworks, and synthesized source material. A monthly goal summary from Notion becomes a permanent note in Obsidian only when it reaches completion and warrants reflection.

### The Bridge

The n8n daily planning workflow and the planning agent together constitute the bridge. The workflow pulls structured Notion data into the vault each morning. The agent synthesizes it with vault context to produce a daily brief that reflects both systems without requiring manual synchronization.

---

## Technical Implementation

### Obsidian Plugins

The vault uses a minimal plugin set to reduce maintenance overhead:

- **Dataview** — SQL-like queries over note metadata, used to surface recently modified notes and open tasks
- **Templater** — Dynamic template execution for date insertion and file naming in session summaries
- **Git** — Automated commit and push on a schedule for version history and cross-device sync redundancy

Plugins deliberately avoided: Calendar, Kanban, and Daily Notes — all replaced by agent workflows or Notion.

### Claude Skills Architecture

Claude Skills are stored as `SKILL.md` files in `20_Knowledge/Skills/`. Each skill file follows a fixed schema with a YAML frontmatter description field that Claude reads to detect when the skill applies:

```yaml
---
name: skill-name
description: "Trigger description used by Claude to detect when to load this skill"
---

# Skill Instructions
[Complete behavioral instructions for this workflow]
```

Skills differ from agents in scope: an agent is a persistent persona governing an entire session. A skill is a workflow invoked for a specific task within any session. A skill completes and returns control to the agent.

### Cross-Device Sync

The vault is stored in Google Drive and accessed locally on multiple devices. Google Drive maintains a local synchronized copy on each device, meaning Obsidian and Claude Code both read and write to the local filesystem — Google Drive handles propagation transparently. An additional Git repository provides version history and serves as a recovery point independent of Google Drive.

---

## Lessons Learned

### What Did Not Work Initially

The first version of this system had no `CONTEXT.md` file. Each agent session began with the user re-explaining their background, active projects, and learning stage. Sessions were useful but produced no compounding value — each one started from zero. Adding a single `CONTEXT.md` file that agents read at startup eliminated this problem entirely.

The second problem was over-organization before content. The initial folder structure included detailed subfolders for every conceivable topic before any notes existed. Empty folders create psychological overhead — they suggest obligations rather than reflecting actual knowledge. The solution was to start with minimal structure and add folders only when a third note on a topic appeared.

The third problem was treating every source as equally worthy of a permanent note. Processing a large book into permanent notes is a project, not a capture workflow. The distinction between `30_Resources/` (reference material consulted as needed) and `20_Knowledge/` (synthesized permanent notes) resolved this. A book gets a single reference note with a linked PDF. Specific insights that become part of active thinking get their own permanent notes as they are encountered.

### Why Most Second Brain Attempts Fail

Most personal knowledge management systems fail for one of three reasons. The system is too complex to maintain — when processing a note requires more than five minutes, notes stop being processed and the inbox becomes a graveyard. The system is not connected to actual work — a vault that contains interesting ideas but no project documentation is a reading list, not a second brain. Or the system has no mechanism for compounding — notes are captured but never linked, so the tenth note adds no more value than the first.

This system addresses all three: daily processing takes five minutes because the inbox is a flat list. Projects live in the vault alongside knowledge. And every agent session ends with mandatory linking of new notes to existing ones.

### What Makes This System Resilient

The system is resilient because its value is stored in plaintext files. If Obsidian is replaced, the notes remain. If a plugin breaks, the notes remain. If the sync service changes, the notes remain. The only irreplaceable component is the linking structure — and those `[[wikilinks]]` are part of the plaintext content itself.

The second factor is that the system is designed around real workflows rather than idealized ones. The inbox is a flat list because organized capture is slower than capture. Agents write session summaries because users will not do it manually after a long technical session.

---

## Conclusion and Future Plans

### Current State

The vault holds knowledge spanning multiple technical domains built over two years of active use. Four agents are operational and cover the primary working contexts. Two n8n workflows handle content curation and daily planning. Research papers and reference books are stored as source PDFs with synthesized knowledge notes linked to them. The graph view shows a dense interconnected network — evidence that the linking discipline is producing the intended compounding effect.

### Planned Extensions

The next planned agent is a research agent specialized in reading technical papers and extracting patterns, linking them to existing knowledge notes in the vault. This agent fills the gap between domain-specific agents by covering cross-domain research that requires synthesis across multiple knowledge areas.

A planned n8n workflow will monitor technical platforms for newly released content matching the user's current skill level and send a Telegram notification, reducing the time spent searching for appropriate learning material.

On the vault architecture side, a `CHANGELOG.md` in the vault root is planned to track structural changes over time — making it possible for agents to understand not just the current state of the vault but how it evolved.

### Long-Term Value

The competitive advantage of a well-maintained second brain compounds over time in a way that tool-based productivity improvements do not. A better task manager makes tomorrow more organized. A vault with years of linked knowledge notes makes every future learning session faster, every future project more informed, and every AI agent session more accurate — because the context the agent reads reflects years of real thinking rather than generic background information. That compounding effect is the reason this system was built and the reason it is worth maintaining rigorously.

---

*Designed and built by Taha Al-Faqih,Berlin *
