# TOOLS.md — What the Agent Can Use

> List every tool the agent has access to. This prevents misuse, helps the agent pick the right tool for each task, and makes token usage more efficient by reducing trial-and-error.

---

## Why This File Matters

Without TOOLS.md, the agent guesses which tool to use. With it, the agent makes intentional choices. Each tool entry should include:
- What it does
- When to use it
- When NOT to use it
- Any known quirks

---

## Web & Research Tools

### Firecrawl / Scrape Creators
```
Purpose: Scrape web pages, extract structured data, crawl sites
Use for: Research tasks, content aggregation, competitor analysis
Do not use for: Simple lookups (use web search instead)
Cost: Per-page pricing. Confirm skill is installed before use.
Install check: Ask "Can you check the Scrape Creators skill is installed?"
```

### Web Search (Perplexity / Brave)
```
Purpose: Real-time web queries
Use for: Current events, quick facts, source verification
Do not use for: Deep page scraping or structured data extraction
```

---

## File System Tools

### Read File
```
Purpose: Read contents of a file in the agent's home directory
Use for: Loading context, reading config files, reviewing outputs
Security: Never read outside the designated home directory
```

### Write File
```
Purpose: Write or update files
Use for: Creating skill files, updating MEMORY.md, saving outputs
Security: Always confirm path before writing. Never overwrite without consent.
```

### Edit File (Patch)
```
Purpose: Apply targeted edits to existing files
Use for: Skill patches, incremental updates, loop-driven improvements
Prefer over full rewrites: Yes, when only specific lines are changing
```

---

## Code & Execution Tools

### Python Execution
```
Purpose: Run Python scripts in a sandboxed environment
Use for: Data processing, calculations, automation scripts
Do not use for: Network calls unless explicitly enabled
Security: Read-only filesystem access by default
```

### Shell / Bash
```
Purpose: Run terminal commands
Use for: File operations, git commands, package management
Security: NEVER run rm -rf or destructive commands without explicit confirmation
Restriction: Never access outside designated project directories
```

---

## Communication & Integration Tools

### Telegram Bot
```
Purpose: Send messages to a configured Telegram channel/chat
Use for: Automated reports, cron job summaries, loop completion alerts
Setup: Requires bot token in config (not in this file)
Example: Weekly skill audit report every Sunday at 8:00 PM
```

### Email (if configured)
```
Purpose: Send formatted email reports
Use for: Stakeholder updates, weekly digests
Security: Never send to addresses not in the approved contacts list
```

---

## Calendar & Scheduling

### Cron Job Manager
```
Purpose: Schedule recurring loops and automations
Use for: Weekly skill audits, content publishing schedules, regular digests
Example cadence: Sunday 8:00 PM = skill audit loop + Telegram report
Setup: Use read-only calendar access first. Expand after testing.
```

---

## Skills (Custom Tool Modules)

```
Skills are pre-built task modules. Before using any skill:
1. Check it is installed: "Can you verify [skill-name] is active?"
2. Review what it touches (each skill has a permissions block)
3. Never install skills from untrusted sources
4. Run in read-only mode first, then expand permissions

Active skills: [List your installed skills here]
Example: linkedin-voice, content-repurpose, research-brief
```

---

## Tool Selection Decision Tree

```
Task: Research a topic
  Is it a quick fact? → Web Search
  Is it structured scraping? → Firecrawl
  Is it document analysis? → Read File + Python

Task: Update agent behavior
  Personality change? → Edit SOUL.md
  New context about user? → Edit USER.md
  Recurring fact to remember? → Edit MEMORY.md
  New task module? → Create a skill file in /skills

Task: Send a report
  Instant notification? → Telegram
  Formal summary? → Email
  Recurring? → Cron job
```

---

*Security reminder: Each tool use is logged by OpenClaw. Review logs weekly. If something looks wrong, check the reasoning log first.*
