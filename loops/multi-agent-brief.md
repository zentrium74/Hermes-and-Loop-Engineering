# Loop: Multi-Agent Research + Writing Pipeline

> **What this loop does:** An orchestrator agent delegates a research task to a Research Subagent, passes the structured brief to a Writer Subagent, evaluates the result, and loops back if quality benchmarks aren't met. The human steers; the loop executes.

---

## When to Use This Loop

Use when:
- You need a piece of content that requires research + original writing
- The topic changes often (weekly newsletter, market update, product teardown)
- You want consistent quality without babysitting every step
- You are trying to operate at Karpathy-tier output volume

---

## Architecture Overview

```
HUMAN INPUT
  │
  ▼
ORCHESTRATOR (COS Agent)
  - Receives goal from human
  - Breaks it into Research brief + Writing brief
  - Manages memory scope for entire loop
  - Evaluates final output
  - Decides: Ship it or loop back
  │
  ├────────────────┐
  ▼                ▼
RESEARCH AGENT    WRITER AGENT
- Searches web    - Receives structured brief
- Scrapes sources - Writes in your voice (linkedin-voice skill)
- Summarizes data - Self-edits against quality benchmarks
- Returns brief   - Returns draft to Orchestrator
  ▼
EVALUATION
  - Pass: output is delivered
  - Fail: specific failure reason sent back to relevant agent
  - Loop: agent addresses the failure and resubmits
  ▼
FINAL OUTPUT → delivered to user
```

---

## Step-by-Step Prompt Blueprint

### Step 1: Human Trigger

```
Goal: "Write a LinkedIn post about [topic]. 
       Research current developments first.
       Write in my voice.
       Pass the quality benchmarks in skills/linkedin-voice.md."
```

### Step 2: Orchestrator Prompt

```
You are the orchestrator for a research + writing loop.

Goal: [paste goal]

Your job:
1. Create a Research Brief for the Research Agent
   - Define exactly what to search
   - Define the output format you need
   - Set a scope limit (e.g., max 5 sources)

2. Create a Writing Brief for the Writer Agent
   - What to write (format, length, platform)
   - Tone and voice constraints (load from USER.md and skills/linkedin-voice.md)
   - Quality benchmarks to pass

3. After both agents return their output:
   - Evaluate the draft against quality benchmarks
   - If pass: return final output to user
   - If fail: identify the specific failure, send correction instruction to the relevant agent, loop

Do not write the content yourself. Delegate.
```

### Step 3: Research Agent Prompt

```
You are the Research Agent.

Research Brief from Orchestrator:
[brief from Step 2]

Your job:
1. Search for current, relevant information on the topic
2. Prioritize: recent (last 30 days), authoritative sources, concrete data
3. Return a structured brief containing:
   - 3-5 key findings (bullet points)
   - 1-2 surprising or counterintuitive data points
   - Any relevant quotes (attributed)
   - A one-sentence synthesis of the narrative angle

Return the brief. Do not write the post.
```

### Step 4: Writer Agent Prompt

```
You are the Writer Agent.

Research Brief:
[output from Research Agent]

Writing Brief:
[brief from Orchestrator]

Voice Profile:
[load from skills/linkedin-voice.md]

Your job:
1. Write a LinkedIn post using the research brief
2. Follow the voice profile exactly
3. Use the structure: Hook > Context > Insight > CTA
4. Stay under 1500 characters
5. Before returning, evaluate against these benchmarks:
   - Hook stops the scroll (not generic)
   - Has one concrete specific (not abstract)
   - CTA is a single question
   - Does not sound AI-generated

If any benchmark fails: rewrite the failing section, then return.
Return the post + a one-line note on what you changed if you revised.
```

### Step 5: Orchestrator Evaluation

```
Evaluate this draft against the quality benchmarks in skills/linkedin-voice.md.

Draft:
[Writer Agent output]

For each benchmark:
- State: PASS or FAIL
- If FAIL: state the exact issue and which agent should fix it

If all benchmarks pass: return the final post to the user.
If any benchmark fails: send the correction instruction to the relevant agent and re-run that step.
Maximum loop iterations: 3. After 3, flag for human review.
```

---

## Scaling This Loop

| Upgrade | How |
|---------|-----|
| Add more content types | Create skills for Twitter threads, video scripts, email newsletters — same loop structure |
| Add a publishing step | Connect the loop to a Buffer/LinkedIn API write skill |
| Add performance tracking | After 48h, pull engagement data — feed back into the skill audit |
| Run in batch | Provide 5 topic briefs at once, let loop run all 5, return a content calendar |
| Add SEO layer | Add a third subagent that validates keyword presence before the Orchestrator evaluates |

---

## Token Budget for This Loop

```
Research Agent: ~8-12k tokens (web scraping + summarization)
Writer Agent: ~3-5k tokens (post generation + self-edit)
Orchestrator: ~5-8k tokens (coordination + evaluation)
Per loop iteration: ~16-25k tokens
Max 3 iterations: ~75k tokens max
Model recommendation: Sonnet for Research + Writer, Haiku for evaluation pass/fail
```

---

## Output

```
File: /outputs/content/[date]-[topic-slug].md
Contents:
  - Final post (copy-paste ready)
  - Research brief (for reference)
  - Loop run summary (iterations, what was revised)
  - Quality benchmark results
```

---

*This is the architecture. Build it once, run it forever.*
