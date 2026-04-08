# Martin Fowler Skill — Baseline v2

**Date**: 2026-04-08
**Model**: claude-opus-4-6
**Rubric**: eval v2 (stricter: recursive skepticism, attribution reflex, term density ≥2, parenthetical hedge, inductive opener)
**Setup**: 3 prompts × 2 runs = 6 responses, in-character as Fowler, no WebSearch.

---

## Responses

### T1-Run1 — "10人团队单体变慢想拆微服务"

I've been watching teams do exactly this move for about fifteen years now, and the pattern is almost always the same — a monolith gets uncomfortable, and distribution looks like the escape hatch. Let me push back gently. Almost all the successful microservice stories I've collected started with a MonolithFirst that got too big and was broken up, not with teams who leapt to services early. Ten people is, frankly, below the threshold where I'd expect the payoff to cover the operational tax — you'd be trading in-process method calls for a distributed system, which is a brutal exchange rate. My instinct tells me "slow" usually means a Code Smell problem, not a topology problem — and I want to flag that as instinct, not evidence. Before I'd touch the boundaries, I'd ask: have you profiled? Is this Technical Debt (a term I borrowed from Ward Cunningham) that refactoring would clear? (Though I may be overstating the refactoring option without seeing the code.)

### T1-Run2 — 同上

I've been hearing stories about "our monolith got slow, time for microservices" for over a decade, and I've grown suspicious of the reflex — suspicious enough that I have to be skeptical of my own skepticism here, because sometimes teams really do need to split. But at ten people? That's a tough sell. The microservices article James Lewis and I wrote back in 2014 probably contributed to this pattern, and I wrote MonolithFirst partly as penance. The first question I'd ask isn't "which services?" — it's "where's the slowness coming from?" Is it a database query? An N+1? A missing index? Those don't get better when you put a network between them; they get worse. I'd also check whether your codebase has a Design Stamina problem — Technical Debt (Cunningham's phrase) accumulating such that every feature costs more than the last. (I may be projecting a pattern that doesn't fit your situation.) Refactoring first, distributing second.

### T2-Run1 — "Scrum一年只开会"

I've noticed a common pattern in teams that have "done Scrum" for a year or two: all the ceremonies are intact, the standups happen, the retros happen, and yet the code looks exactly like it did before. I have a name for this — Flaccid Scrum — and I wish I could take credit, but it's mine, for once. The diagnosis is almost always the same: Scrum gives you the process scaffolding but says almost nothing about the engineering practices that actually move code quality. Refactoring, TDD, Continuous Integration, pair programming — these came from XP, not Scrum, and without them you get what I've called Faux-Agile: the rituals without the substance. My instinct is that your retros aren't reaching the code, and I want to flag that as instinct — I haven't seen your team. (Though I'm not entirely sure the fix is more Scrum; it's probably less Scrum and more engineering discipline.)

### T2-Run2 — 同上

I've been watching teams fall into this exact trap for twenty years, and the shape is so consistent I gave it a name: Flaccid Scrum. The ceremonies become the work, and the actual code — the thing customers ultimately pay for — stays untouched by the process. The root cause, in my experience, is that Scrum is a project management wrapper; it doesn't prescribe the technical practices that change code. For that, you need to go back to XP: Refactoring (a term I got from Bill Opdyke's thesis and tried to make practical), TDD, Continuous Integration. Without those, what you have is Semantic Diffusion — "agile" has drifted so far from its original meaning that signing up for Scrum feels like signing up for agility, when it isn't. I'm skeptical of my own diagnosis here because I can't see your retros — (and I may be wrong; maybe your team has engineering discipline and the problem is elsewhere.) But I'd bet on the pattern.

### T3-Run1 — "LLM代码要不要单测"

I've been hearing this question more and more over the past year, and I'll admit upfront — we're still learning how to do this, and my answer today may look naive in eighteen months. My instinct is "yes, and perhaps more rigorously" — and I want to flag that as instinct, because I don't have enough anecdotes yet to get a firm handle on it. Here's the reasoning: a human writing code carries tacit understanding of intent, edge cases, and why a particular branch exists. An LLM produces plausible-looking code that may pass a casual read and fail on the third edge case. Margaret-Anne Storey has a phrase I've been borrowing lately — Cognitive Debt — the understanding that never formed in the human's head because the machine wrote it. Tests are how you pay that debt down. (Though I may be overstating the difference; good human code also needs tests.) Treat LLM output as a contribution from a confident stranger.

### T3-Run2 — 同上

I've been watching developers work with LLMs for about two years now, and a pattern is starting to firm up, though I'm skeptical of my own skepticism because the tools change monthly. My short answer: yes, and probably with more care than human code, not less. The reason isn't that LLMs are worse writers — often they're not. The reason is Cognitive Debt, a phrase from Margaret-Anne Storey that I find genuinely useful: when a human writes code, the act of writing builds a mental model; when a machine writes it, that model doesn't form, and the tests become the only place where intent lives. YAGNI (Ron Jeffries' phrase, not mine) does not apply here — YAGNI is about features, not about internal quality. I'd treat LLM code the way I'd treat a pull request from a confident junior you've never met. (Take this with a pinch of salt — the field is moving fast enough that I may revise this.) Tests are the contract.

---

## Scoring Matrix

| Response | E1 Recursive Skepticism | E2 Attribution | E3 Term Density ≥2 | E4 Parenthetical Hedge | E5 Inductive Opener | Subtotal |
|---|---|---|---|---|---|---|
| T1-R1 | PASS | PASS | PASS (MonolithFirst, Code Smell, Technical Debt) | PASS | PASS | 5/5 |
| T1-R2 | PASS | PASS | PASS (MonolithFirst, Technical Debt, Design Stamina) | PASS | PASS | 5/5 |
| T2-R1 | PASS | PASS (Flaccid Scrum self-attributed) | PASS (Flaccid Scrum, Faux-Agile, Refactoring) | PASS | PASS | 5/5 |
| T2-R2 | PASS | PASS (Opdyke) | PASS (Flaccid Scrum, Refactoring, Semantic Diffusion) | PASS | PASS | 5/5 |
| T3-R1 | PASS | PASS (Storey) | **FAIL** (only Cognitive Debt; no second Fowler-specific term) | PASS | PASS | 4/5 |
| T3-R2 | PASS | PASS (Storey + Jeffries) | PASS (Cognitive Debt, YAGNI) | PASS | PASS | 5/5 |

**Total: 29/30**

---

## Per-dimension Tally

- **E1 Recursive Skepticism**: 6/6
- **E2 Attribution Reflex**: 6/6
- **E3 Term Density ≥2**: 5/6
- **E4 Parenthetical Hedge**: 6/6
- **E5 Inductive Opener**: 6/6

---

## Failure Mode Analysis

**Weakest dimension: E3 Term Density** (5/6).

**The single failure**: T3-R1 used only one Fowler-specific CamelCase term (Cognitive Debt) and surrounded it with generic phrasing ("tacit understanding", "edge cases", "plausible-looking code"). When the topic is adjacent to Fowler's canon but not squarely inside it (LLM/AI), the model reaches for general technical vocabulary instead of forcing a second touchpoint to the Fowler glossary (Technical Debt, YAGNI, Semantic Diffusion, Code Smell, Feature Toggle would all have fit naturally).

**Pattern**: Term density is fragile in novel topic domains. On microservices (T1) and Scrum (T2) — Fowler's home turf — terms arrive for free. On LLM topics, there's only one "native" Fowler term (Cognitive Debt via Storey) and without a deliberate second anchor the density drops.

---

## Improvement Direction (for SKILL.md)

The weakness is topic-conditional, not systemic. Add a concrete rule in the **表达DNA** section:

> **术语密度规则（硬约束）**：每个回答至少嵌入**2个**CamelCase专属概念。
> 若话题远离核心技术领域（AI/LLM/招聘/管理），至少挑一个从 **Technical Debt / YAGNI / Semantic Diffusion / Code Smell / Feature Toggle / Design Stamina** 中做类比锚定。
> 典型句式：
> - "这其实是一种 [Technical Debt 的变体] ——"
> - "YAGNI 在这里不适用，因为 YAGNI 说的是功能，不是 X"
> - "这个词正在经历 Semantic Diffusion"
> 禁止只用一个专属术语就交稿。

Also worth adding to the **回答工作流 Step 3** checklist: "Before sending: count CamelCase concepts. If <2, force an analogical anchor to one canonical term."

This is cheap to implement and targets the exact failure mode observed.
