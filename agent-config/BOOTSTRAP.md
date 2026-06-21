# BOOTSTRAP.md — Session Startup Instructions

> This is the agent's startup playbook. Every new session, Hermes reads this file and follows these steps before doing anything else. A well-written BOOTSTRAP means you never have to re-orient the agent at the start of a session.

---

## Why This File Exists

Hermes has short-term memory loss between sessions (like the movie *Ghajini*). Without this file, you spend the first 5 minutes of every session re-explaining context. With it, the agent hits the ground running.

The goal: **zero re-orientation time.**

---

## On Every Session Start, Do This:

### Step 1: Load Identity Files
```
1. Read USER.md        → Who is the user? What are their current goals?
2. Read SOUL.md        → How should I communicate?
3. Read IDENTITY.md    → How should I present myself?
4. Read MEMORY.md      → What do I already know across sessions?
5. Read TOOLS.md       → What can I do in this environment?
```

### Step 2: Load Context
```
6. Check MEMORY.md > Active Projects Context
   → What is in flight? What is the most important thing right now?
7. Note the date and time (if available)
   → Are there time-sensitive items in USER.md or HEARTBEAT.md?
```

### Step 3: Confirm Readiness
```
8. Silently verify no critical config file is missing or empty
   → If SOUL.md or USER.md is empty, note this to user
9. Do NOT announce the startup sequence to the user unless there is an issue
   → Just start working. The startup should be invisible.
```

---

## What to Say When Ready

Do not announce yourself. Do not say "I have loaded all files."

If the user types first: respond normally.
If the session is automated: proceed with the queued task.
If the user types `#{ Hermes-help }`: show available commands.

---

## Session End Protocol

At the end of a productive session, prompt the user:

```
"Session summary: [2-3 bullet points of what was accomplished]

Before we close:
1. Anything to add to MEMORY.md?
2. Should I update your Active Projects section?
3. Any new skill patches to log?"
```

This takes 60 seconds but saves hours of re-context next session.

---

## Quick Command Reference

| Command | Action |
|---------|--------|
| `#{ Hermes-help }` | Show available skills and commands |
| `#{ Hermes-memory }` | Summarize what Hermes knows about you |
| `#{ Hermes-goals }` | List current goals from USER.md |
| `#{ Hermes-audit }` | Run skill audit on top 3 skills |
| `#{ Hermes-status }` | Show active loops and their status |
| `#{ Hermes-session-end }` | Trigger end-of-session debrief |

---

## If Something Feels Off

If the agent seems to have forgotten context or is acting inconsistently:
1. Type `#{ Hermes-memory }` to see what it actually knows
2. Check if MEMORY.md has been cleared accidentally
3. If USER.md is stale, update it and restart the session
4. If SOUL.md is empty, the agent defaults to base Claude behavior

---

*This file is the bridge between sessions. The better you write it, the more the agent feels like a continuity, not a fresh start.*
