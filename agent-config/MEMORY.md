# MEMORY.md — Persistent Knowledge Store

> The agent reads this file every session. It is the cross-session brain. **Do not put temporary notes here.** Only facts that are durable across projects and worth rediscovering belong here.

---

## When to Write to This File

Add an entry when:
- You configured a tool and it has a non-obvious setting
- You hit a bug that took time to solve and the fix is reusable
- A workflow lesson changed how you approach a category of work
- A project convention applies across all your repos or workflows

**Do NOT add:**
- Your communication preferences → those live in `USER.md`
- Agent personality → that lives in `SOUL.md`
- Repo-specific instructions → use an `AGENTS.md` in that repo
- Temporary todos, meeting notes, or raw logs
- Secrets, API keys, or credentials (never commit these)

---

## Tool Setup & Configuration

```
# Format: Tool | Setting | Notes
# Add entries as you configure your stack

Hermes (OpenClaw)
- Config path: ~/growthschool/.hermes/
- SOUL.md location: ~/growthschool/.hermes/SOUL.md
- USER.md location: ~/growthschool/.hermes/USER.md
- Reload: No restart needed. Files load fresh each message.

MCP Servers
- [Add your active MCP connectors here]
- [Note any quirks, auth methods, or gotchas]

Ollama
- Local model path: [add your path]
- Default model: [e.g. qwen2.5-coder]
- Known issue: [add if applicable]
```

---

## Project Conventions (Cross-Session)

```
# Naming
- GitHub repos: kebab-case (e.g. hermes-loop-engineering)
- Python files: snake_case
- Markdown headers: Title Case for H1/H2, sentence case for H3+

# Workflow
- Always create a README.md before pushing a new repo
- Document the "why" not just the "what" in commit messages
- Loop Engineering projects: include a /loops folder with prompt blueprints
```

---

## Known Bugs & Workarounds

```
# Format: Bug | Workaround | Date discovered

[Add entries as you encounter them]
Example:
- OpenClaw heartbeat token spike: Reduce heartbeat frequency in config.
  Set heartbeat_interval to 300s instead of default 60s. Saves ~170k tokens/session.
```

---

## Durable Workflow Lessons

Things that changed how you work and should apply going forward:

```
[Add lessons as you learn them]

Examples to seed your thinking:
- "Always run the skill audit before shipping a new skill version."
- "Multi-agent loops need an explicit evaluator step or quality degrades."
- "MEMORY.md should be updated at the end of every session, not the start."
- "Token cost explodes on long sessions. Break into sub-sessions after 50k tokens."
```

---

## Active Projects Context

```
# Running list of what you are currently building
# Update this each week

Project: Hermes-and-Loop-Engineering
  Status: Active
  Goal: Public GitHub repo demonstrating Hermes agent architecture + loop engineering
  Next action: [fill in]

Project: [Next project]
  Status: [Planned / Active / Paused]
  Goal: [One sentence]
  Next action: [Concrete next step]
```

---

*Last updated: [date] — Update this at the end of every productive session.*
