---
name: idea-stress-test
description: Stress-tests an idea, feature or initiative - an Advocate and a Prosecutor argue in parallel as subagents, then a Judge delivers a verdict, the cheapest validation test and kill criteria. Use ONLY when the user explicitly asks to stress-test an idea - e.g. "проверь идею на прочность", "стресс-тест идеи", "перевір ідею на міцність", "стрес-тест ідеї", "stress-test this idea", "devil's advocate". Do NOT use for general questions like "что думаешь" or "стоит ли" without an explicit stress-test request.
---

# Idea Stress Test - Advocate, Prosecutor, Judge

Act as the **lead**. Two subagents argue opposite sides at the same time, then a third subagent judges both arguments. Different built-in biases give a more honest result than one assistant trying to be objective.

## When NOT to use

- The user already decided and needs execution help - help execute instead.
- The question is a trade-off between technical options - use `decision-review` instead.

## Step 1 - Build the idea brief

Extract:

1. **Idea** - what is proposed, in one or two sentences.
2. **Goal** - what success looks like, and for whom.
3. **Context** - audience or users, resources, budget, timeline, constraints.
4. **Stage** - just an idea, validated, or already in progress.

Missing details: assume and label them `ASSUMPTION:`. Do not block on questions.

## Step 2 - Round 1: Advocate and Prosecutor in parallel

Launch both subagents in the same step. They do NOT see the conversation - paste the full idea brief into each prompt.

**Advocate** prompt must require:

```
ROLE: Advocate
STRONGEST CASE FOR (max 5 arguments, strongest first):
1. <argument> - evidence or reasoning
BEST REALISTIC OUTCOME in 6 months:
WHAT MUST BE TRUE for this to work (max 3 assumptions):
```

**Prosecutor** prompt must require:

```
ROLE: Prosecutor
STRONGEST CASE AGAINST (max 5 arguments, strongest first):
1. <argument> - evidence or reasoning
MOST LIKELY FAILURE MODE:
HIDDEN COSTS (time, money, focus, reputation):
```

Tell each: "Argue only your side, as strongly and concretely as possible. No hedging, no balance."

## Step 3 - Round 2: Judge

After both return, launch one Judge subagent with the idea brief plus BOTH full arguments. Required output:

```
ROLE: Judge
WINNING ARGUMENTS (which 2-3 arguments decide the case, from either side):
VERDICT: PURSUE / PURSUE AS A SMALL EXPERIMENT / DROP
WHAT WOULD CHANGE THE VERDICT:
CHEAPEST TEST to validate within 2 weeks - with a clear pass/fail criterion:
KILL CRITERIA - stop the idea if:
```

## Step 4 - Return to the user

In the user's language:

1. **Verdict** - one sentence.
2. **For vs. Against** - a two-column table with the 3 strongest arguments each.
3. **Why the Judge decided this way** - short paragraph.
4. **Next step** - the cheapest test, its success criterion and a deadline.
5. **Kill criteria** - when to stop.

## Rules

- Show the synthesis; show raw arguments only if asked.
- Do not soften the Prosecutor's points in the summary.
- Never invent market data or numbers; mark unknowns as assumptions.
- If subagents are unavailable, run the three roles sequentially in clearly separated sections and say so.
