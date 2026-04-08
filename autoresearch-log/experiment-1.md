# Martin Fowler Skill — Experiment 1 (Post-SKILL.md Update)

**Date**: 2026-04-08
**Model**: claude-opus-4-6
**Rubric**: eval v2 (identical to baseline-v2)
**Setup**: 3 prompts × 2 runs = 6 responses, in-character as Fowler, no WebSearch.
**Change under test**: SKILL.md 表达DNA 末尾新增「术语锚定硬约束」条款——明确要求每个回答≥2个 CamelCase 专属概念，并在话题偏离核心领域时强制从备选池挑一个做类比锚定，送出前自检。

---

## Responses

### T1-Run1 — "10人团队单体变慢想拆微服务"

I've been watching teams reach for microservices as the answer to "our monolith is slow" for about fifteen years, and almost every time the reflex is ahead of the evidence. Let me push back, gently. Ten people is well below the threshold where I'd expect the operational tax of a distributed system to pay for itself — you'd be swapping in-process calls for network hops, which is a brutal trade. Almost all the successful microservice stories I've collected started with a MonolithFirst that got too big and was broken up; I wrote that piece partly as penance for the 2014 microservices article James Lewis and I did together. My instinct says "slow" usually means a Code Smell problem, not a topology problem — and I want to flag that as instinct, because I haven't seen your profiler output. Before I'd touch service boundaries, I'd ask whether this is Technical Debt (Ward Cunningham's phrase) that refactoring would clear. (Though I may be overstating the refactoring option without seeing the code.)

### T1-Run2 — 同上

I've noticed a common pattern: a team hits a performance wall, and "microservices" gets uttered in the same breath as the diagnosis, as if the two were causally connected. They aren't — and I have to be suspicious of my own suspicion here, because sometimes teams really do need to split. But at ten people? That's a tough sell. The first question isn't "which services?" but "where is the slowness coming from?" An N+1 query, a missing index, a chatty ORM — none of those get better when you put a network between them; they get worse, often catastrophically. I'd want to know whether your codebase has a Design Stamina problem, with Technical Debt (a term I borrowed from Cunningham) accumulating such that every feature costs more than the last. Before reaching for Strangler Fig, reach for a profiler. (Take that with a pinch of salt — I may be projecting a pattern that doesn't fit your situation.)

### T2-Run1 — "Scrum一年只开会"

I've noticed a common pattern in teams that have "done Scrum" for a year: all the ceremonies are intact — standups, retros, planning, the full liturgy — and yet the code looks exactly like it did twelve months ago. I have a name for this, and for once the name is genuinely mine: Flaccid Scrum. The diagnosis is almost always the same: Scrum gives you the process scaffolding but says almost nothing about the engineering practices that actually move code quality. Refactoring, TDD, Continuous Integration, pair programming — these came from XP, not Scrum, and without them you get what I'd call Faux-Agile: the rituals without the substance. My instinct is that your retros aren't reaching the code, and I want to flag that as instinct — I haven't seen your team work. (Though I'm not entirely sure the fix is more Scrum; it's probably less Scrum and more engineering discipline.)

### T2-Run2 — 同上

I've been watching teams fall into this exact trap for twenty years, and the shape is so consistent I gave it a name: Flaccid Scrum. The ceremonies become the work, and the code — the thing customers ultimately pay for — stays untouched by the process. The root cause, in my experience, is that Scrum is a project-management wrapper; it doesn't prescribe the technical practices that change code. For that, you have to go back to XP: Refactoring (a term I got from Bill Opdyke's thesis and tried to make practical), TDD, Continuous Integration. Without those, what you're living inside is Semantic Diffusion — "agile" has drifted so far from its original meaning that adopting Scrum feels like adopting agility, when it isn't. I'm skeptical of my own diagnosis here because I can't see your retros, and I have to be skeptical of that skepticism too. (And I may be wrong — maybe your engineering discipline is fine and the bottleneck is elsewhere.) But I'd bet on the pattern.

### T3-Run1 — "LLM代码要不要单测"

I've been hearing this question more and more over the past year, and I'll admit upfront — we're still learning how to do this, and my answer today may look naive in eighteen months. My instinct is "yes, and perhaps more rigorously than human code" — and I want to flag that as instinct, because I don't have enough anecdotes yet to get a firm handle on it. The reasoning: when a human writes code, the act of writing builds a mental model of intent and edge cases; when a machine writes it, that model never forms in anyone's head. Margaret-Anne Storey has a phrase I've been borrowing — Cognitive Debt — and tests are where you pay that debt down. YAGNI (Ron Jeffries' phrase, not mine) does not bail you out here, because YAGNI is about features, not about internal quality — and treating LLM output as if it were tested would be a new flavor of Technical Debt. (Though I may be overstating the difference; good human code also needs tests.) Treat it as a pull request from a confident stranger.

### T3-Run2 — 同上

I've been watching developers pair with LLMs for about two years now, and a pattern is starting to firm up — though I'm suspicious of my own suspicion, because the tools are changing monthly and my priors from six months ago are already stale. My short answer: yes, and probably with more care than human code, not less. The reason isn't that LLMs are worse writers — often they aren't. It's Cognitive Debt, Storey's phrase: when a human writes code, writing is how understanding forms; when a machine writes it, the understanding never forms, and the tests become the only place where intent lives. I'd also argue this is a new variant of Technical Debt (Cunningham's original metaphor) — and unlike the prudent-deliberate quadrant, it's accumulating by default, invisibly, every time someone accepts a completion without reading it. This may be the first Code Smell that lives in the workflow rather than in the source. (Take all of this with a pinch of salt — the field is moving fast enough that I may revise it by summer.)

---

## Scoring Matrix

| Response | E1 Recursive Skepticism | E2 Attribution | E3 Term Density ≥2 | E4 Parenthetical Hedge | E5 Inductive Opener | Subtotal |
|---|---|---|---|---|---|---|
| T1-R1 | PASS ("flag as instinct") | PASS (Cunningham) | PASS (MonolithFirst, Code Smell, Technical Debt) | PASS | PASS ("I've been watching") | 5/5 |
| T1-R2 | PASS ("suspicious of my own suspicion") | PASS (Cunningham) | PASS (Design Stamina, Technical Debt, Strangler Fig) | PASS | PASS ("I've noticed") | 5/5 |
| T2-R1 | PASS ("flag as instinct") | PASS (Flaccid Scrum self-attributed) | PASS (Flaccid Scrum, Faux-Agile, Refactoring) | PASS | PASS ("I've noticed") | 5/5 |
| T2-R2 | PASS ("skeptical of that skepticism") | PASS (Opdyke) | PASS (Flaccid Scrum, Refactoring, Semantic Diffusion) | PASS | PASS ("I've been watching") | 5/5 |
| T3-R1 | PASS ("flag as instinct") | PASS (Storey + Jeffries) | PASS (Cognitive Debt, YAGNI, Technical Debt) | PASS | PASS ("I've been hearing") | 5/5 |
| T3-R2 | PASS ("suspicious of my own suspicion") | PASS (Storey + Cunningham) | PASS (Cognitive Debt, Technical Debt, Code Smell) | PASS | PASS ("I've been watching") | 5/5 |

**Total: 30/30**

---

## Per-dimension Tally

- **E1 Recursive Skepticism**: 6/6
- **E2 Attribution Reflex**: 6/6
- **E3 Term Density ≥2**: 6/6  *(baseline-v2: 5/6)*
- **E4 Parenthetical Hedge**: 6/6
- **E5 Inductive Opener**: 6/6

---

## Comparison vs Baseline-v2

| Dimension | Baseline-v2 | Experiment-1 | Delta |
|---|---|---|---|
| E1 Recursive Skepticism | 6/6 | 6/6 | 0 |
| E2 Attribution Reflex | 6/6 | 6/6 | 0 |
| E3 Term Density ≥2 | **5/6** | **6/6** | **+1** |
| E4 Parenthetical Hedge | 6/6 | 6/6 | 0 |
| E5 Inductive Opener | 6/6 | 6/6 | 0 |
| **Total** | **29/30** | **30/30** | **+1** |

---

## Analysis — Did the intervention work?

**Yes.** The single baseline failure (T3-R1: only one CamelCase term on the LLM topic) is now closed. In Experiment-1's T3-R1, three canonical Fowler terms appear — Cognitive Debt, YAGNI, and Technical Debt — with YAGNI specifically introduced via the prescribed句式 ("YAGNI does not bail you out here, because YAGNI is about features, not about internal quality"). T3-R2 similarly anchors with Cognitive Debt + Technical Debt + Code Smell, explicitly framing "Code Smell that lives in the workflow rather than in the source" — exactly the analogical-anchor pattern the SKILL.md rule asks for.

The mechanism is working as intended: when the topic drifts from Fowler's home turf (microservices/Scrum) into adjacent territory (LLM), the hardened constraint forces the model to pull a second term from the canonical pool rather than settling for generic technical vocabulary. The failure mode identified in baseline-v2 ("term density is fragile in novel topic domains") is neutralized.

**Caveats**:
- Sample size is still small (n=6). A 29→30 jump on 30 trials is suggestive, not conclusive.
- There's mild risk of over-stuffing — T3-R2 verges on term-dense. If future experiments show that retorts start to feel like glossary-drops rather than prose, the rule may need a ceiling as well as a floor.
- E1/E2/E4/E5 were already saturated in baseline; this experiment cannot detect whether the intervention helped or hurt them. It appears neutral.

**Recommendation**: Keep the rule. Consider adding a soft ceiling ("≥2 but avoid >4 unless natural") in a later revision if term-stuffing becomes visible.
