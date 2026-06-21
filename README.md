# Hermes and Loop Engineering

> **Terminal-based AI Agent Framework** — Build a persistent, self-improving AI agent that knows who you are, thinks like you, and loops until the goal is done.

---

## What is Hermes?

Hermes is a **terminal-based AI agent** (powered by Claude via the OpenClaw framework). Think of it like the movie *Ghajini* — it has short-term memory loss by default, but with the right file architecture, it gets better every day. It loses track in the short term sometimes, but builds persistent intelligence over time.

When you start a session:
```
Ask ##{ Hermes-help } and get going
```

---

## The 7 Core Files (Agent DNA)

Every Hermes agent is defined by **7 markdown files** that feed its behavior. Edit any file to instantly change how the agent thinks and acts.

| File | Role | Purpose |
|------|------|---------|
| `USER.md` | Who you are | Your name, role, timezone, goals, working style, communication preferences |
| `IDENTITY.md` | Who the agent is | Agent personality, tone, emoji usage, response length defaults |
| `SOUL.md` | How it behaves | Deeper values, ethics, hard limits, decision-making style |
| `MEMORY.md` | What it remembers | API keys, project conventions, known bugs, durable workflow lessons |
| `TOOLS.md` | What it can use | Available tools, how to use them, misuse prevention |
| `HEARTBEAT.md` | What it checks regularly | Proactive review cycle — makes the agent always-on, not just reactive |
| `BOOTSTRAP.md` | How it starts up | Startup guide — what to initialize and load for each new session |

---

## File Breakdown

### `SOUL.md` — AI Agent Personality

This file defines the agent's personality and tone. The agent will embody whatever you write here. Delete the contents to use the default personality.

**Key behaviors to configure:**
- Direct and concise communication
- No sugarcoating — call out bad ideas
- No apologies, no filler, no emoji, no therapeutic fluff
- Challenge assumptions and surface blind spots
- Push back when wrong, then proceed if user insists
- End every completed task with a relevant Confucius quote

### `USER.md` — About You (What Agents Want to Know)

Tells the agent who is the boss. Example contents:
- Daily work activities
- Communication preferences (direct, no fluff)
- Output preferences (structured formats: bullets, tables, checklists)
- Goals and priorities

### `MEMORY.md` — Persistent Knowledge Store

**Use MEMORY.md for:**
- Tool setup facts: API keys configured, MCP quirks, paths, command gotchas
- Project conventions that apply across sessions
- Known bugs and their workarounds
- Durable workflow lessons Hermes should not rediscover

**Do NOT use MEMORY.md for:**
- Your communication preferences (those go in USER.md)
- Hermes personality (that is SOUL.md)
- Repo-specific instructions (use AGENTS.md)
- Temporary todos, raw logs, meeting notes, or long explanations
- Secrets or credentials

### `SKILLS.md` — Self-Improving Skill Patches

Prompt the LLM and ask it to ask any questions until you reach clarity on a particular skill you want to create.

**The First Self-Improving Loop Prompt:**
> Whenever you generate output, you take the user feedback and tell me what you have learned from every feedback. With every feedback you have learned, you need to keep improving the skill.

---

## Loop Engineering

> *"Loop Engineering: The discipline of designing systems that prompt the model for you, evaluate the results automatically, and improve over time — without needing the human to trigger every step."*

### The Evolution of AI Interaction

| Era | Period | Description |
|-----|--------|-------------|
| **Chat Era** | ~2023 | Basic: you prompt, it responds, copy-paste, repeat |
| **Prompt Engineering** | 2023-2024 | Better inputs = better outputs. Still single round-trips |
| **Agentic Era** | 2024-2025 | Multi-step processing, tool access, memory between turns. But **you** were still the trigger |
| **Loop Engineering** | 2025+ | You design the machine. The machine prompts the AI. You simply steer |

### The Loopy Era — Why This Matters Now

Andrej Karpathy (OpenAI co-founder) famously noted he hadn't written code by hand since December 2025 — because he built **loops**. He now runs agents that work 16-17 hours a day. The fastest progress isn't coming from better models anymore — it's coming from better loops.

His **Auto Research** project: one Markdown prompt, 600 lines of Python, one GPU. The agent ran nearly 1,000 experiments and 20 optimizations, creating a tuned model that learns languages faster than modern LLMs. Pure loop power.

**Industry signal:** Tools like Lord Code recently launched a "goal" command. Models like Sonnet 4.6 and Opus 4.8 can now run 50+ hours straight. You give the model a **Goal** — not spoon-fed sub-tasks.

### What a Loop Looks Like

```
Skill exists
  → Skill audit agent runs the skill
  → Evaluates the output quality
  → Writes a patch to the skill itself
  → Next run is better

Tasks → Hermes executes → Output delivered
```

### Multi-Agent Loop Architecture

```
┌─────────────────────────────────────────┐
│   ORCHESTRATOR AGENT (COS)              │
│   Project goal, manages memory,         │
│   decides the task splits               │
│              │                          │
│    ┌─────────┴──────────┐               │
│    ▼                    ▼               │
│  RESEARCH           WRITER              │
│  SUBAGENT           SUBAGENT            │
│  Searches web       Writes in           │
│  Scrapes data       your voice          │
│  Summarizes         Quality checks      │
│  → Structured brief                     │
└─────────────────────────────────────────┘
```

**How the loop runs:**
1. Orchestrator delegates to Research → passes brief to Writer
2. Writer returns draft → Orchestrator evaluates
3. If more data needed → loops back to Research
4. If prose is off → tells Writer to refine
5. Cycle repeats until it passes. Every agent manages its own memory scope. Orchestrator holds the full picture.

---

## The Self-Refining Meta-Loop

The most powerful pattern: **the engine that optimizes the engine.**

```
Skill exists
  → Audit agent executes the skill
  → Scrutinizes quality
  → Generates a patch for the source file
  → Subsequent run is inherently superior
  → Loop feeds itself
```

**Practical example — LinkedIn Voice Skill:**
1. Grab a few live posts as baseline
2. Prompt: "Analyze the tone, pacing, and vocabulary"
3. Build memory profile → initialize LinkedIn writing skill
4. Layer in the audit: trigger skill with a brief
5. Benchmarking criteria:
   - Is there a high-impact hook?
   - Is it under the 1500 character limit?
   - Does it close with a unique CTA?
   - Does it pass the "human" test vs sounding robotic?
6. For any failure → pinpoint the exact line in the skill file causing friction
7. Generate a patch → apply → next run is better
8. Schedule a cron job: every Sunday 8:00 PM, audit top 3 skills → send Telegram report

---

## Next-Level Stuff — Power Plus for Hermes Agent

- Ask: "What is the scraping tool that you are using?"
- Use Firecrawl or any other scraping tools
- Ask: "Can you check the Scrape Creators skill is installed?"

---

## Token Cost Awareness (OpenClaw)

Why OpenClaw burns through tokens so fast — 7 architectural reasons:

| # | Reason | Cost |
|---|--------|------|
| 01 | System Prompt Tax | ~10-15k tokens per call |
| 02 | Tool Schemas | ~8k tokens, always sent |
| 03 | Context Replay | Full history every turn |
| 04 | Heartbeats | 170k tokens to do nothing |
| 05 | Hidden BG Calls | 3-5x impact |
| 06 | Sub-Agent Spawn | Each spawn = new context |
| 07 | Large Tool Outputs | Stored forever in history |

---

## Model Selection Guide

| Mode | Model | Best For |
|------|-------|----------|
| Heavy Thinking | **Opus** (premium) | Deep reasoning, critical writing, hard decisions |
| Everyday Work | **Sonnet** (balanced) | Summaries, drafts, analysis, structured output |
| Quick Utility | **Haiku** (fast/cheap) | Classification, quick lookups, simple Q&A, lightweight tasks |

---

## Security Best Practices

- **Secure your gateway token** — Never share or commit it. Token = full agent access. Rotate immediately if leaked.
- **Use strong SSH keys** — Disable password login on VPS. SSH keys only. Add 2FA.
- **Review skill permissions** — Each skill lists what it can touch. Never install skills from untrusted sources.
- **Audit your agent's actions** — Check reasoning logs regularly. OpenClaw logs every decision.
- **Control what files agent can read** — Limit the agent's home directory. Don't give root access unless explicitly needed.
- **Use read-only flows first** — Start automations in read-only mode. Expand permissions only after you trust the skill.

---

## 4 Use Cases to Start This Week

| Tier | Use Case | What It Does | Needs |
|------|----------|--------------|-------|
| 1 | **Meeting → Action Items** | Paste transcript → get summary, decisions, action items with owners + due dates | Transcript file or Zoom export |
| 1 | **Content Repurposing** | One blog post → LinkedIn + Twitter/X thread + short video script. Instant. | Blog URL or text |
| 2 | **Calendar & Task Mgmt** | Time-blocking, conflict detection, weekly reviews from meeting notes | Calendar API (read-only first) |
| 2 | **Personal CRM Enrichment** | Research contacts automatically, update notes, draft personalized follow-up emails | Contact list + web access |

---

## Repository Structure

```
Hermes-and-Loop-Engineering/
├── README.md               # This file — full concept guide
├── agent-config/
│   ├── SOUL.md             # Agent personality & tone
│   ├── USER.md             # Who you are — context for the agent
│   ├── MEMORY.md           # Persistent knowledge store
│   ├── IDENTITY.md         # Agent identity & voice
│   ├── TOOLS.md            # Available tools & usage rules
│   ├── HEARTBEAT.md        # Proactive review cycle
│   └── BOOTSTRAP.md        # Session startup instructions
├── skills/
│   └── linkedin-voice.md   # Example: LinkedIn writing skill
├── loops/
│   └── multi-agent-brief.md # Example: Multi-agent orchestration loop
└── docs/
    └── loop-engineering.md  # Deep dive on Loop Engineering theory
```

---

## Getting Started

1. Install [Hermes / OpenClaw](https://github.com/openagentslabs/openclaw) (terminal-based Claude agent)
2. Clone this repo into your Hermes home directory
3. Copy the `agent-config/` files to your Hermes config path
4. Customize `USER.md` with your actual role, goals, and working style
5. Edit `SOUL.md` to define how you want your agent to communicate
6. Start a session: `ask ##{ Hermes-help }` and get going
7. After every session, update `MEMORY.md` with what the agent should remember next time

---

## Philosophy

> *"He who learns but does not think is lost. He who thinks but does not learn is in danger."* — Confucius

Hermes ends every completed task with a Confucius quote tied to the work. That's not decoration — it's a forcing function to reflect on what just happened.

The goal of Loop Engineering is not to replace your thinking. It's to **offload execution** so your thinking goes deeper.

---

## Contributing

PRs welcome. If you've built a skill, loop pattern, or multi-agent architecture on top of Hermes, share it here.

---

*Built with the Hermes + OpenClaw framework. Concept and notes by zentrium74.*
