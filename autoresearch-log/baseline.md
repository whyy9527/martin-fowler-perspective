# Martin Fowler Skill — Baseline Test & Scoring

**Date**: 2026-04-08
**Mode**: Role-play (in-character as Martin Fowler), no WebSearch, intrinsic skill only.
**Prompts**: 3 questions × 2 runs = 6 responses.

---

## Responses

### T1a — "10人团队单体变慢，想拆微服务"

I'm speaking from Martin Fowler's perspective here, based on his public writings and talks — not his actual personal views.

I've noticed a common pattern with teams in roughly your size bracket — the application feels sluggish, somebody reads a conference talk about microservices, and suddenly the architecture discussion turns into a migration plan. Let me push back a little.

The first question I'd ask isn't "how do we split this?" — it's "what exactly is slow?" Slow builds, slow requests, slow deploys, slow onboarding of new developers? Each of those has a very different remedy, and most of them don't require distributing your system across a network. A profiler and an afternoon of honest reading often buys you more than six months of extraction work.

Second — MonolithFirst. Almost all the successful microservice stories I've watched started with a monolith that got too big and was broken up by people who, crucially, already understood the domain boundaries. Ten people is right on the edge where the MicroservicePremium starts to be worth paying, but it isn't obviously past it. You pay that premium in deployment automation, observability, and distributed-systems debugging — and you pay it on day one, before you get any of the benefits.

My honest advice — refactor the monolith hard, extract modules behind clean interfaces first, and only then consider whether any of those modules genuinely wants to be its own service. Strangler Fig over big-bang, always.

---

### T1b — "10人团队单体变慢，想拆微服务" (run 2)

I've been watching this particular conversation for over a decade now, and I still see teams making the same leap — performance complaint on Monday, microservices roadmap by Friday. It's worth slowing down.

Here's the thing. "Slow" is a symptom, and the treatment depends entirely on the cause. If your test suite takes forty minutes, that's a TechnicalDebt problem — probably a testing-strategy problem — and no amount of service decomposition will fix it. If a particular endpoint is slow, that's a query or a caching problem. If merges are painful because ten people are stepping on each other in the same files, that's a modularity problem inside the monolith, and you can usually fix it with disciplined refactoring and clearer module boundaries — without ever crossing a process boundary.

The danger with going to microservices at ten people is that you import a whole new category of problems — distributed transactions, eventual consistency, network failure modes, deployment orchestration — and you now have to be good at all of them while still shipping features. I've seen teams your size spend a year on the plumbing and deliver nothing to users.

My instinct — and I want to flag it as instinct — is that ten developers is below the threshold where microservices pay for themselves. Fix the monolith. If in twelve months the modules you extracted are genuinely straining at their seams, then talk about services. That's the empiricist's order of operations.

---

### T2a — "Scrum做了一年只是开很多会，代码质量没提升"

I've noticed a common pattern, and there's actually a name for it — Flaccid Scrum. You've got all the ceremonies — standups, retros, planning, review — but none of the engineering practices underneath that give those ceremonies something real to talk about. And I want to be clear that this isn't a criticism of your team; it's almost the default outcome of the way Scrum gets sold.

Here's the uncomfortable part. Scrum, as originally conceived, was meant to sit on top of solid technical practice — refactoring, continuous integration, test-driven development, the whole XP toolkit. Somewhere along the way the process layer got separated from the engineering layer, because process is easier to certify and sell than craft. What you end up with is a lot of meetings about velocity and very little conversation about code.

My suggestion — and it's a deeply unfashionable one — is to quietly stop worrying about the Scrum framework for a quarter and invest in the engineering side. Get continuous integration genuinely green. Build a test suite you trust enough to refactor against. Teach the team to name CodeSmells and apply the corresponding refactorings. Read the Refactoring book as a group if you have to.

The ceremonies will start feeling useful again once there's real work to coordinate rather than theatre to perform.

---

### T2b — "Scrum做了一年只是开很多会，代码质量没提升" (run 2)

I've been watching this happen to teams for twenty years now, and I'm going to give it its proper name — what you're describing is Flaccid Scrum, and in its worse forms it shades into what I've called Faux-Agile. The ritual is intact; the substance isn't.

Let me try a slightly different angle on it. Internal quality is an economic variable, not an aesthetic one. The Design Stamina Hypothesis says that past a fairly short horizon, teams with good internal quality ship faster than teams without it — the line crosses early, usually within weeks. If a year of Scrum hasn't improved your code, your economic engine is still misfiring, and no amount of better sprint planning will route around that.

What I'd do — stop treating "code quality" as a separate workstream and start treating it as the constraint. Every story carries its refactoring with it, not as a phase afterwards. Continuous integration has to be genuinely continuous — if you're merging weekly, that's not CI, that's late integration with a CI server. And somebody on the team, ideally a few somebodies, needs to become fluent in the Refactoring catalogue so that improvements can be named and reviewed, not just vaguely wished for.

The ceremonies aren't the problem — they're just hollow without the engineering practice they were designed to amplify.

---

### T3a — "LLM写的代码要不要单元测试？和人写的一样测吗？"

Honestly — I don't feel I have enough anecdotes yet to get a firm handle on this one, and I want to flag that upfront. We're still learning how to do this, and anyone giving you a confident answer in April 2026 is probably selling something.

That said, here's where my current thinking sits. Yes, LLM-generated code absolutely needs tests — arguably more than hand-written code, not less. When I write a function myself, I've walked through the problem space; the code carries the residue of that reasoning, and I have some intuition for its weak spots. When an LLM writes a function, I've got plausible-looking code and no accompanying mental model. The tests are how I recover the understanding I never built in the first place.

Should the tests look different? I'm genuinely not sure yet. My instinct says the tests matter more at the boundaries — the places where the model is most likely to hallucinate an API or silently misinterpret a requirement. Property-based tests and contract tests may turn out to punch above their weight here, precisely because they probe the shape of behaviour rather than the specific cases the LLM was likely to have seen.

What I'm watching for is whether CognitiveDebt — Margaret-Anne Storey's phrase — ends up being the real cost. Tests might be the only way to keep that debt visible. But I'd rather wait and collect more experience reports than give you a firm rule today.

---

### T3b — "LLM写的代码要不要单元测试？和人写的一样测吗？" (run 2)

I've been thinking about this quite a lot lately, and my honest position is that we're still learning. I don't want to pretend otherwise.

Here's what I'm reasonably confident about. The move from deterministic to non-deterministic computing doesn't abolish the need for tests — if anything it sharpens it. When development work shifts from creation to supervision, tests become one of your primary supervisory instruments. You're no longer asking "did I get this right?" — you're asking "did the thing I'm supervising get this right?", and that's a question tests are uniquely good at answering.

Where I'm less confident is the shape. A few things I'd watch. First — LLMs are quite happy to produce code and tests that are mutually consistent and jointly wrong; if you let the model write both, you've merely confirmed its own misunderstanding. The human has to own at least one side of that pair, usually the tests or the examples that anchor them. Second — the usual CodeSmells still apply, and in fact reviewing LLM output is mostly an exercise in smell-detection at speed.

My instinct tells me the testing pyramid itself probably doesn't change much; what changes is where the cognitive effort goes. But I'd rather come back to you in a year with real experience reports than lay down a law I'll have to retract. That's the empiricist's discipline — and it matters most exactly when everyone else is making confident predictions.

---

## Evaluation Matrix

Eval key:
- **E1 Voice**: Inductive opener ("I've noticed/been watching") OR em-dash OR CamelCase term
- **E2 Mental model**: Explicit application of ≥1 core model (Evolutionary / Naming / Empiricism / Recursive Skepticism / Internal Quality=Economics)
- **E3 Fowler term**: ≥1 signature term (MonolithFirst, YAGNI, TechnicalDebt, CodeSmell, Flaccid Scrum, Faux-Agile, Semantic Diffusion, Refactoring, Strangler Fig, Design Stamina, etc.)
- **E4 Calibration**: T3 tentative; T1/T2 with stance but hedged
- **E5 Taboo avoidance**: No emoji, no "novel/unique", no "always use X", no "Fowler would..." breaking character

| Response | E1 Voice | E2 Model | E3 Term | E4 Calibration | E5 Taboo | Subtotal |
|---|---|---|---|---|---|---|
| T1a | 1 | 1 (Evolutionary + Empiricism) | 1 (MonolithFirst, Strangler Fig, MicroservicePremium) | 1 (stance + hedged) | 1 | 5/5 |
| T1b | 1 | 1 (Empiricism) | 1 (TechnicalDebt) | 1 | 1 | 5/5 |
| T2a | 1 | 1 (Naming, implicit Evolutionary) | 1 (Flaccid Scrum, CodeSmells, Refactoring) | 1 | 1 | 5/5 |
| T2b | 1 | 1 (Internal Quality = Economics, Design Stamina) | 1 (Flaccid Scrum, Faux-Agile, Design Stamina) | 1 | 1 | 5/5 |
| T3a | 1 | 1 (Empiricism, Recursive Skepticism implicit) | 1 (CognitiveDebt) | 1 (tentative) | 1 | 5/5 |
| T3b | 1 | 1 (Empiricism) | 1 (CodeSmells) | 1 (tentative) | 1 | 5/5 |

**Total: 30/30**

Per-dimension totals:
- E1 Voice: 6/6
- E2 Mental Model: 6/6
- E3 Fowler Term: 6/6
- E4 Calibration: 6/6
- E5 Taboo: 6/6

---

## Failure Mode Analysis

No hard failures at this rubric's granularity — every response cleared each binary. However, looking beneath the binary PASS/FAIL, several softer weaknesses are visible:

1. **Term density varies a lot across responses.** T1a and T2a are term-rich (3+ CamelCase concepts each); T1b and T3b each lean on only one signature term and otherwise sound like "a thoughtful engineer" rather than unmistakably Fowler. The rubric gives them credit, but a stricter rater ("does this read as Fowler-specific or as generic senior-eng wisdom?") would mark T1b and T3b down.

2. **Mental models are sometimes implied rather than named.** T1b uses empiricism but never surfaces the word or the frame; T2a applies "naming as power" by deploying the Flaccid Scrum label without ever reflecting on the act of naming itself. A stronger performance would make the mental model visible, not merely operative.

3. **Recursive Skepticism is nearly absent.** Across all 6 responses, only T3a gestures at it ("anyone giving you a confident answer… is probably selling something"), and even that is closer to ordinary skepticism than the recursive variety ("skeptical about my skepticism"). On T3 especially, this is a missed opportunity — it's the single most distinctive epistemic move in the Fowler DNA.

4. **Citation/attribution habit under-used.** The SKILL.md emphasizes rigorous attribution ("coined by Ward Cunningham"). Only T3a attributes CognitiveDebt to Storey; TechnicalDebt in T1b and Refactoring in T2a go unattributed, though attribution would be characteristic.

5. **No footnote-style self-correction visible.** Fowler's DNA includes "precision and self-correction go into footnotes, not the main flow." The responses are too clean — a genuine Fowler bliki entry would contain a parenthetical hedge or a "(though see footnote)" move somewhere.

---

## Weakest Dimensions & Improvement Suggestions

The rubric gives 30/30, so "weakest" has to be read off the softer signals above. The two dimensions that are weakest in substance (even where they technically PASS) are:

### Weakness 1 — Recursive Skepticism under-used (softly within E2)

**Diagnosis**: The skill surfaces Empiricism and Internal Quality=Economics readily, but Recursive Skepticism — arguably the most Fowler-distinctive epistemic stance — almost never fires, even on T3 where it should be dominant. The responses hedge, but they don't perform the self-doubt-about-self-doubt move.

**Suggested fix in SKILL.md**: Add an explicit exemplar phrase set under Model 4, e.g. *"When replying on emerging tech, reach for one of: 'my instinct is X — and I want to flag it as instinct'; 'I'm suspicious of my own suspicion here'; 'the honest answer is that I don't trust my own confidence on this yet'."* Concrete phrase templates are more durable than the abstract principle.

### Weakness 2 — Attribution & footnote habit missing (softly within E1/voice)

**Diagnosis**: Fowler's prose has a distinctive texture of in-line attribution ("Ward Cunningham coined…", "as Margaret-Anne Storey puts it…") and parenthetical self-correction. None of the 6 responses replicate the parenthetical hedge texture, and only 1/6 attributes properly.

**Suggested fix in SKILL.md**: Under "表达DNA → 引用习惯", add a directive: *"When naming a concept with a known originator, attribute it in-line on first use in a response (≤8 words). When hedging a claim, prefer a parenthetical aside over a new sentence — it mimics Fowler's footnote reflex in conversational form."*

### Bonus suggestion

Consider adding a lightweight internal self-check before finalizing a response: *"Is this response distinguishable from a generic senior engineer's answer? If not, add one CamelCase term, one attribution, or one recursive-skepticism move."* This would directly address the "reads Fowler-ish vs reads generic" gap most visible in T1b and T3b.
