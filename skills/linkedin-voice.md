# Skill: LinkedIn Voice

> **What this skill does:** Takes a brief (topic + intent) and produces a LinkedIn post in your established voice. Self-improves on every cycle based on performance feedback.

---

## Skill Metadata

```
Skill name: linkedin-voice
Version: 1.0
Created: [date]
Last audited: [date]
Audit schedule: Weekly, Sunday 8:00 PM (via HEARTBEAT.md cron job)
Permissions: Read USER.md, write to /outputs/linkedin/
```

---

## How to Use This Skill

```
Trigger: "Run linkedin-voice skill"
Input: Brief containing:
  - Topic / angle
  - Target audience
  - Desired outcome (awareness, engagement, leads, connection requests)
  - Any constraints (avoid X, include Y, tie to Z event)

Output: Full LinkedIn post ready to copy-paste
```

**Example trigger:**
> "Run the LinkedIn voice skill. Topic: Why AI agents are replacing prompt engineers. Audience: Technical founders. Goal: Get comments from people debating this."

---

## Voice Profile (Built from Your Posts)

The agent builds this profile by analyzing your top 5-10 existing posts. Update this section with the output of your first voice analysis.

```
Tone: [Direct / Conversational / Educational / Provocative]
Pacing: [Short punchy sentences / Medium paragraphs / Mix]
Vocabulary level: [Technical / Accessible / Mix]
Typical structure: [Hook / Context / Insight / CTA]
Sentence length: [Average N words]
Parenthetical usage: [Low / Medium / High]
Emoji usage: [None / Rare / Moderate]
Hash tags: [None / 1-3 max / topical only]
```

---

## Post Generation Instructions

When this skill is triggered:

1. Read USER.md → understand current goals and priorities
2. Load voice profile from this file
3. Generate a post using this structure:

```
STRUCTURE:

Hook (line 1, max 12 words)
  → Must stop the scroll. Statement, contrast, or unexpected angle.
  → No "I am excited to..." or "Thrilled to share..."

Context (2-4 lines)
  → Why this matters. Set the stakes.

Core insight (3-6 lines)
  → The actual value. Opinion, observation, or mechanism.
  → Use specifics. Not "many companies" but "3 out of 5 seed-stage startups."

CTA (1-2 lines)
  → One clear ask. Ask a question, challenge a belief, or invite response.
  → Make it easy to reply yes or no.

Character count: Under 1500 characters
Line spacing: Single empty line between sections
```

---

## Quality Benchmarks (Used by the Audit Loop)

The audit agent evaluates every generated post against these criteria:

| Criterion | Pass Condition | Fail Condition |
|-----------|---------------|----------------|
| Hook strength | Makes you want to read line 2 | Generic opener or starts with "I" |
| Character count | Under 1500 chars | Over 1500 chars |
| CTA clarity | One specific ask | Vague, multiple asks, or no CTA |
| Human test | Sounds like a person | Sounds like a template or AI |
| Voice match | Matches the tone profile above | Noticeably different style |
| Specificity | Includes at least one concrete detail | All abstract claims |

---

## The Self-Improvement Loop

After each post is generated and feedback is received:

1. Capture what worked and what didn't
2. Identify which line in this file caused the friction
3. Generate a patch (specific edit to this skill file)
4. Apply the patch
5. Next post is better

**Feedback format to trigger a patch:**
> "The hook was too soft. Patch the skill to make hooks more provocative."
> "The CTA asked two questions. Patch skill to enforce single-question CTAs only."

---

## Patch Log

```
# Version history of patches applied to this skill
# Format: [date] | Change | Reason

[date] | v1.0 initial | Created from baseline voice analysis
[Add entries here as patches are applied]
```

---

## Example Output

```
Most people are still prompt engineering.
The people beating them are loop engineering.

Prompt engineering = you trigger the AI.
Loop engineering = you design systems that trigger the AI for you.

Andrej Karpathy hasn't written code by hand since December 2025.
He builds loops. His agents run 16-17 hours a day.
He steers. The loops execute.

You don't need a better prompt.
You need a better machine.

What's the first loop you would build if you started today?
```

---

*This skill improves with every post. The more feedback you give it, the sharper it gets.*
