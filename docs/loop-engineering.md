# Loop Engineering — The Deep Dive

> *"Loop Engineering: The discipline of designing systems that prompt the model for you, evaluate the results automatically, and improve over time — without needing the human to trigger every step."*

---

## The 4 Eras of AI Interaction

Every major shift in AI capability has changed *where the human sits* in the workflow.

| Era | Period | Human Role | Output Quality |
|-----|--------|------------|----------------|
| **Chat Era** | ~2023 | Typist | Inconsistent, copy-paste dependent |
| **Prompt Engineering** | 2023-2024 | Input optimizer | Better, but still single round-trips |
| **Agentic Era** | 2024-2025 | Task definer | Multi-step, tool-using, memory-aware |
| **Loop Engineering** | 2025+ | System designer | Self-improving, always-on, autonomous |

The pattern is clear: the human moves further from execution and closer to design with each era.

In Loop Engineering, your job is not to type better prompts. Your job is to build machines that type better prompts for you.

---

## Why This Matters Right Now

### The Karpathy Signal

Andrej Karpathy, co-founder of OpenAI, stated publicly that he hasn't written code by hand since December 2025. He builds loops. His agents run 16-17 hours a day. He steers; the loops execute.

His **Auto Research** project illustrates the potential:
- One Markdown prompt file
- 600 lines of Python
- One GPU
- The agent ran nearly 1,000 experiments and 20 optimization passes
- Result: a tuned model that learns languages faster than modern LLMs

No human ran those 1,000 experiments. The loop did.

### The Industry Signal

Tools are catching up to the concept:
- **Lore Code** launched a `goal` command: give the model a destination, not a task list
- **Claude Sonnet 4.6** and **Opus 4.8** can sustain context for 50+ hours continuously
- **MCP connectors** (N8N, Zapier) allow loops to integrate with real-world systems without custom code
- **Hermes / OpenClaw** runs Claude natively in your terminal with persistent file-based memory

The infrastructure for loops exists. The bottleneck is now design skill, not tooling.

---

## The 3 Types of Loops

### Type 1: The Production Loop

A loop that converts a repeating human task into an automated workflow.

```
Human defines the task once →
  Agent executes on trigger →
    Output delivered to human →
      Human reviews (optional) →
        Loop continues
```

**Example:** Every Monday morning, the loop:
1. Pulls your calendar for the week
2. Identifies gaps and conflicts
3. Generates a proposed daily plan
4. Sends it to you as a Telegram message

You wake up to a ready-to-act plan. The loop ran while you slept.

---

### Type 2: The Evaluation Loop

A loop that generates output, then evaluates it against benchmarks before delivering it.

```
Input brief →
  Agent generates output →
    Evaluator checks output against benchmarks →
      Pass: deliver | Fail: send correction + loop back
```

**Example:** The LinkedIn Voice skill in this repo uses this loop:
- Post is generated
- Evaluator checks hook, character count, CTA clarity, human-sounding quality
- Any failure triggers a targeted patch, not a full rewrite
- Output only exits the loop when it passes all benchmarks

Quality floors, not quality ceilings. Every output meets the minimum bar.

---

### Type 3: The Meta-Loop (Self-Refining)

The most powerful. A loop that improves the loop itself.

```
Skill file exists →
  Audit agent runs the skill with a test brief →
    Evaluates output quality →
      Identifies which line in the skill file caused friction →
        Generates a patch →
          Applies patch to skill file →
            Next run is inherently better →
              Loop continues
```

**Example:** The LinkedIn Voice skill is audited every Sunday at 8:00 PM.
- Top 3 skills are tested
- Any that underperform are patched automatically
- You wake up Monday with sharper skills than Friday

The machine maintains itself.

---

## The 5 Principles of Loop Design

### 1. Separate Generation from Evaluation
Never let the agent that creates the output also evaluate it. That is grading your own homework. The evaluator should have no stake in passing the output.

### 2. Benchmarks Before Briefs
Define what "good" looks like before you define the task. If you can't articulate pass/fail criteria, the loop has no exit condition and will drift.

### 3. Fail Fast, Fail Specific
When a loop iteration fails, the failure message must identify the *exact* issue and *exactly* which component should fix it. Vague feedback produces vague fixes.

### 4. Token Budgets Are Loop Budgets
Every loop iteration costs tokens. Design the token footprint as part of the loop architecture, not as an afterthought. Use Haiku for evaluation steps, Sonnet for generation, Opus only for high-stakes reasoning.

### 5. Build the Simplest Loop That Works
Do not start with a 6-agent architecture. Start with 1 agent that generates + 1 that evaluates. Ship that. Add complexity only when the simple version hits a real ceiling.

---

## The 4 Starter Use Cases

Starting points that are achievable this week:

### Tier 1 (No integrations needed)

**Meeting → Action Items**
```
Input: paste transcript
Loop: extract summary, decisions, action items, owners, due dates
Evaluator: are all decisions covered? Is each item owner-assigned?
Output: formatted action item list
```

**Content Repurposing**
```
Input: one blog post URL or text
Loop: generate LinkedIn post + Twitter thread + short video script
Evaluator: does each match the target platform's format constraints?
Output: 3 platform-ready pieces from 1 source
```

### Tier 2 (Light integrations)

**Calendar + Task Management**
```
Input: meeting notes + current tasks
Loop: time-block week, detect conflicts, propose resolutions
Evaluator: are all high-priority tasks scheduled? No double-bookings?
Output: proposed weekly plan
Integration: calendar read-only (start here, never write first)
```

**Personal CRM Enrichment**
```
Input: contact list CSV
Loop: research each contact, update notes, draft personalized follow-up
Evaluator: is the follow-up relevant to their recent activity?
Output: enriched CRM + draft emails
Integration: web search + email draft (never send without review)
```

---

## Token Cost Architecture

Why most people hit unexpected token bills:

| Source | Tokens per call | Mitigation |
|--------|----------------|------------|
| System prompt (OpenClaw) | ~10-15k | Cannot reduce; this is the context |
| Tool schemas | ~8k | Loaded every call; design fewer, better tools |
| Full context replay | Variable | Break long sessions into sub-sessions |
| Heartbeats (default 60s) | ~170k/hour idle | Set to 300s minimum |
| Background calls | 3-5x multiplier | Audit with OpenClaw logs |
| Sub-agent spawning | New context per agent | Use subagents only when parallelism is worth the cost |
| Large tool outputs | Accumulates | Truncate outputs; don't store full scrapes in context |

**Rule of thumb:** If a session hits 50k tokens, end it and start a fresh one. Long sessions accumulate context debt faster than they accumulate work output.

---

## Security Principles for Loop Engineering

1. **Read before write.** Every automation starts in read-only mode. You earn write permissions by trusting the read output first.
2. **One action per loop.** Don't chain irreversible actions in a single loop pass without a human checkpoint in between.
3. **Log everything.** OpenClaw logs every decision. Read the log weekly. Patterns in failures reveal skill weaknesses.
4. **Token = gate.** Never give an agent access to a tool it doesn't need for the specific task at hand.
5. **Loop debt.** Loops that run forever with no exit condition are bugs. Every loop needs: a pass condition, a fail condition, and a max iteration limit.

---

## Where to Go From Here

1. **Get Hermes running** — See README.md for setup
2. **Customize your 7 core files** — Start with `USER.md` and `SOUL.md`
3. **Run your first production loop** — Use one of the Tier 1 use cases above
4. **Build your first skill** — Use `skills/linkedin-voice.md` as a template
5. **Ship your first evaluation loop** — Use `loops/multi-agent-brief.md` as your architecture
6. **Activate the meta-loop** — Schedule the skill audit in `HEARTBEAT.md`

The compound effect kicks in around week 3. The first week is setup. The second week is figuring out what breaks. The third week is when the loops start saving more time than they take.

---

*The goal is not to use AI more. The goal is to design systems where AI does the work while you do the thinking. That is Loop Engineering.*
