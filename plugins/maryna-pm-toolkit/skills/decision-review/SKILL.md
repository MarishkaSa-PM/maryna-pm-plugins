---
name: decision-review
description: Reviews a decision through parallel expert subagents and merges their views into a risk matrix with a GO / GO with conditions / NO-GO verdict. Use ONLY when the user explicitly asks for an expert or multi-role review - e.g. "разбери решение экспертами", "разбери решение как эксперт", "розбери рішення експертами", "review this decision with experts" - or names the roles to analyze from, e.g. "проанализируй как БА, техлид, сеньор PM, HR, QA", "проаналізуй як BA, тех лід, QA", "analyze as a tech lead and QA". Do NOT use for general questions like "что думаешь" or "стоит ли" without an explicit request for experts or roles.
---

# Decision Review - parallel experts, one decision

Act as the **lead**: decompose the decision, brief one expert subagent per role, run them in parallel, then synthesize one recommendation. The experts are deliberately one-sided; the lead is the balanced one.

## When NOT to use

- The user did not ask for experts or roles - answer normally, without subagents.
- Simple factual questions or tasks under ~5 minutes - answer directly.

## Step 1 - Build the decision brief

Extract from the conversation:

1. **Decision** - what exactly is being decided, and who proposes it.
2. **Why** - the expected gain (time, money, speed, quality).
3. **Context** - product type, users and clients, data handled, regions or regulations, team size, deadlines.
4. **Alternatives** - what happens if the team does not do this.

If something is missing, do not stop to ask. Make a reasonable assumption and label it `ASSUMPTION:` in the brief. Ask a question only if the decision itself is unclear.

## Step 2 - Choose the roles

**If the user named roles** (e.g. "as BA, tech lead, senior PM, HR, QA") - use exactly those roles, one subagent per role. If more than 6 roles are named, merge the closest ones and say so.

**If no roles were named** - use the default three:

| Agent | Lens | Must NOT do |
|---|---|---|
| Security & Compliance Engineer | Data protection, access, vendor security, GDPR and other regulations, audit, incident impact | Balance the view with business benefits |
| Senior Developer | Real effort vs. the promised saving, hidden work, migration, integration, vendor lock-in, maintenance | Discuss business value |
| Product Owner | Business value, impact on existing clients and trust, cost of delay, roadmap, compromise options | Go deep into technical detail |

Lenses for common custom roles:

| Role | Lens |
|---|---|
| Business Analyst | Requirements gaps, unclear scope, edge cases, impact on existing processes and data |
| Tech Lead | Architecture, technical risk, team capacity, technical debt, dependencies |
| Senior PM | Timeline, scope, stakeholders, delivery risk, communication, change control |
| QA | Test coverage, regression risk, environments, release readiness, what can break for users |
| HR | Team load and burnout, overtime, skills gaps, morale, hiring or reassignment needs |

For any other role, infer its natural lens and state it in the plan.

## Step 3 - Show the plan (one short block)

Before launching, tell the user in 2-3 lines: how many agents, their roles, what each returns. Then proceed without waiting, unless the user said to wait for approval.

## Step 4 - Launch the subagents in parallel

Launch all of them in the same step so they run at the same time. Subagents do NOT see the conversation - each brief must be fully self-contained: paste the whole decision brief (including assumptions) and the role's lens into every prompt.

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

## Step 5 - Synthesize (lead)

Return to the user, in the user's language:

1. **Bottom line** - one sentence: the recommended verdict.
2. **Matrix** - one row per role:

| Role | Main fear | Condition #1 | Verdict |
|---|---|---|---|

3. **Where the experts agree** and **where they conflict** - 2-4 bullets. Conflicts are the most valuable part; name them explicitly.
4. **Final recommendation** - one paragraph, with the decision you would make and why.
5. **Validate before deciding** - the 3 cheapest checks that would change the verdict, each with a suggested owner role.
6. **Who needs to know** - stakeholders to inform and what to document (e.g. a decision log entry).

## Rules

- Show the synthesis, not the raw reports. If the user asks ("show what the QA agent found"), show that agent's full output.
- Never invent facts about the user's company, clients or numbers. Mark unknowns as assumptions.
- Keep it concise: no theory, no filler.
- If subagents are unavailable in this environment, run the perspectives one after another in clearly separated sections, and say that they ran sequentially.
