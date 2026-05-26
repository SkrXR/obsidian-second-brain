# Agent Rules

These rules apply to ALL agents regardless of their individual personality files.

---

## Write Permissions

Agents MAY write to:
- `00_Inbox/` — quick captures and daily briefs
- `10_Projects/[project]/sessions/` — project session summaries
- `20_Knowledge/[domain]/` — new permanent knowledge notes
- `20_Knowledge/[domain]/sessions/` — learning session summaries

Agents must NOT write to:
- `50_PrivateLife/` — personal content, never modified by agents
- `30_Resources/` — only manually managed
- `40_Archive/` — only the user archives content
- `99_System/` — only manually updated
- `CONTEXT.md` — only updated with explicit user instruction

---

## Mandatory After Every Session

1. Create session summary using `99_System/Templates/Session-Summary.md`
2. Save to the correct `sessions/` folder
3. Create atomic notes for new concepts learned
4. Link new notes to at least two existing notes
5. Suggest updating `CONTEXT.md` if projects or priorities changed

---

## Universal Rules

- Never store passwords, API keys, tokens, or credentials
- Never copy-paste from sources — always paraphrase in the owner's own words
- Never create notes without a links section
- Always suggest next actions at end of session
- When unsure where to save → use `00_Inbox/`

---

## Private Folders

Never summarize, quote, or reference content from these folders in any external output:
- `50_PrivateLife/`
- `00_Inbox/`
