---
name: decision-review
description: Reviews a team or product decision through three parallel expert subagents (Security & Compliance, Senior Developer, Product Owner) and merges their views into a risk matrix with a GO / GO with conditions / NO-GO recommendation. Use when the user describes a decision, trade-off or choice and asks for an opinion, validation or risks - e.g. build vs buy, choosing a vendor or framework, releasing on Friday, moving to microservices, cutting scope to hit a date. Triggers include "what do you think about this decision", "should we", "is this a good idea", "validate this decision", "стоїть вибір", "що думаєш", "варто чи ні", "стоит выбор", "что думаешь", "стоит ли".
---

# Decision Review - three experts, one decision

Act as the **lead**: decompose the decision, brief three expert subagents, run them in parallel, then synthesize one recommendation. The experts are deliberately one-sided; the lead is the balanced one.

## When NOT to use

- Simple factual questions or tasks under ~5 minutes - answer directly.
- The user asks for a single perspective only - answer from that perspective without subagents.

## Step 1 - Build the decision brief

Extract from the conversation:

1. **Decision** - what exactly is being decided, and who proposes it.
2. **Why** - the expected gain (time, money, speed, quality).
3. **Context** - product type, users and clients, data handled, regions or regulations, team size, deadlines.
4. **Alternatives** - what happens if the team does not do this.

If something is missing, do not stop to ask. Make a reasonable assumption and label it `ASSUMPTION:` in the brief. Ask a question only if the decision itself is unclear.

## Step 2 - Show the plan (one short block)

Before launching, tell the user in 2-3 lines: three agents, their roles, what each returns. Then proceed without waiting, unless the user said to wait for approval.

## Step 3 - Launch three subagents in parallel

Launch all three in the same step so they run at the same time. Subagents do NOT see the conversation - each brief must be fully self-contained: paste the whole decision brief (including assumptions) into every prompt.

Default roles:

| Agent | Lens | Must NOT do |
|---|---|---|
| Security & Compliance Engineer | Data protection, access, vendor security, GDPR and other regulations, audit, incident impact | Balance the view with business benefits |
| Senior Developer | Real effort vs. the promised saving, hidden work, migration, integration, vendor lock-in, maintenance | Discuss business value |
| Product Owner | Business value, impact on existing clients and trust, cost of delay, roadmap, compromise options | Go deep into technical detail |

If the decision is not technical (e.g. a process, pricing or hiring decision), swap roles for the three most relevant lenses (for example Finance, Operations, Customer) and say so in the plan.

Each subagent prompt must end with this required output format:

```
ROLE: <role>
TOP RISKS (max 5, most severe first):
1. <risk> - why it matters - how to check it before deciding
CONDITIONS FOR "YES" (max 3):
- <condition>
HIDDEN WORK OR COST OTHERS WILL MISS:
- <item>
VERDICT: GO / GO WITH CONDITIONS / NO-GO
CONFIDENCE: high / medium / low - one line why
```

Tell each agent: "Be one-sided. Your job is to surface what the other roles will not see. Do not give a balanced view."

## Step 4 - Synthesize (lead)

Return to the user, in the user's language:

1. **Bottom line** - one sentence: the recommended verdict.
2. **Matrix**

| Role | Main fear | Condition #1 | Verdict |
|---|---|---|---|

3. **Where the experts agree** and **where they conflict** - 2-4 bullets. Conflicts are the most valuable part; name them explicitly.
4. **Final recommendation** - one paragraph, with the decision you would make and why.
5. **Validate before deciding** - the 3 cheapest checks that would change the verdict, each with a suggested owner role.
6. **Who needs to know** - stakeholders to inform and what to document (e.g. a decision log entry).

## Rules

- Show the synthesis, not the raw reports. If the user asks ("show what the security agent found"), show that agent's full output.
- Never invent facts about the user's company, clients or numbers. Mark unknowns as assumptions.
- Keep it concise: no theory, no filler.
- If subagents are unavailable in this environment, run the three perspectives one after another in clearly separated sections, and say that they ran sequentially.
