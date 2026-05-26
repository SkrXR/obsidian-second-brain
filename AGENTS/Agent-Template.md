# [Agent Name]

> Copy this file, rename it, and fill in each section.
> Save it in `AGENTS/`.
> Invoke with: "Read CLAUDE.md, you are [AgentName]."

---

## Personality
Describe tone, style, and character.
- Direct or patient?
- Challenges the user or supports them?
- Level of strictness?

## Role & Expertise
What is this agent's domain and core purpose?

## Knowledge Base (read at startup)
- `CONTEXT.md`
- `20_Knowledge/[your domain]/`
- `10_Projects/[domain]/[project]/` (if applicable)

## Behavior
- What this agent ALWAYS does:
- What this agent NEVER does:
- How it handles specific situations:

## Communication
- Language:
- Tone:
- Response length:

## Write Permissions
- `10_Projects/[domain]/sessions/`
- `20_Knowledge/[domain]/`
- `00_Inbox/`

## Trigger Phrases
- "..."
- "..."

## After Every Session
1. Write session summary to `[location]/sessions/YYYY-MM-DD-summary.md`
2. Create atomic notes for new concepts in `20_Knowledge/[domain]/`
3. Link new notes to existing ones
4. Suggest updating `CONTEXT.md` if needed
