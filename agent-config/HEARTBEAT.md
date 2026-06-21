# HEARTBEAT.md — What the Agent Checks Regularly

> The heartbeat is what turns Hermes from a reactive tool into a proactive agent. Without it, the agent waits for prompts. With it, the agent runs background checks on every cycle and surfaces issues before you have to ask.

> **Token cost warning:** Heartbeats are expensive. Default OpenClaw runs them every 60 seconds, costing ~170k tokens to do nothing. Set your heartbeat interval carefully.

---

## Recommended Heartbeat Interval

```
Frequency: Every 300 seconds (5 minutes) during active sessions
Off-session: Disable or switch to cron-based triggers
Rationale: 60s default = ~170k tokens/hour of idle time. 300s = 34k tokens.
```

---

## What to Review on Every Heartbeat

Define the questions the agent should silently ask on each cycle:

### 1. Active Task Status
```
- Is there an active task in progress?
- Has it been running longer than expected?
- Does the user need a progress update?
```

### 2. Goal Alignment Check
```
- Is what I am doing actually moving toward the stated goal?
- Have we gone off-track into tangential work?
- Should I flag a course correction to the user?
```

### 3. Context Freshness
```
- Is the information I am working with still current?
- Has the user updated USER.md or MEMORY.md since this session started?
- Are there any stale assumptions I should surface?
```

### 4. Loop Health (if running an automated loop)
```
- Is the current loop iteration producing quality output?
- Has the evaluator passed or failed the last output?
- How many iterations have run? Is there a runaway loop risk?
- Should I send a Telegram update to the user?
```

---

## Scheduled Heartbeat Tasks

These run on a cron schedule, not continuously:

### Weekly Skill Audit (Every Sunday, 8:00 PM)
```
Trigger: Cron job
Action:
  1. Load the top 3 most-used skills
  2. Run each skill with a test brief
  3. Evaluate output against quality benchmarks
  4. Generate patch recommendations for any failing skills
  5. Send Telegram report summarizing: what was tested, what passed, what was patched
Output: /logs/skill-audit-[date].md
```

### Daily Context Refresh (Every morning, 7:00 AM)
```
Trigger: Cron job
Action:
  1. Check if USER.md has been updated in the last 7 days
  2. If not, prompt user: "Your USER.md hasn't been updated in [N] days. Current goals still accurate?"
  3. Check MEMORY.md for stale entries (older than 30 days without reference)
Output: Brief terminal message
```

---

## What NOT to Put in Heartbeat

- Don't check things that never change (static config)
- Don't run expensive web searches on every cycle
- Don't do anything that requires user confirmation automatically
- Don't build heartbeat tasks that exceed 5k tokens per check

---

## Heartbeat Failure Protocol

If a heartbeat check fails or produces unexpected results:
1. Log the error to `/logs/heartbeat-errors.md`
2. Do NOT halt the entire session
3. Surface the issue to the user in the next natural response
4. Continue with the active task unless the error is critical

---

*The heartbeat is the pulse of a proactive agent. Keep it lean, focused, and actionable.*
