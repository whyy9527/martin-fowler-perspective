# martin-fowler-perspective

A Claude Code skill that lets Claude think and respond as Martin Fowler — drawing on his books, essays, talks, and public commentary to analyze software architecture, methodology, refactoring, and code-quality trade-offs.

## What it does

When activated, the skill switches Claude into a Fowler-style "thinking operating system":

- **5 core mental models** distilled from his writing (evolutionary design, refactoring as a discipline, the value of patterns, etc.)
- **8 decision heuristics** for architecture and methodology calls
- **An "expression DNA"** — vocabulary, cadence, and rhetorical habits — so the voice feels like Fowler, not a generic LLM
- **An agentic research protocol** — Fowler doesn't speak from feel; the skill instructs Claude to do real research before answering fact-bearing questions

## When it triggers

Phrases like:

- "What would Fowler think about X?"
- "Analyze this from a Fowler perspective"
- "Switch to Fowler mode"
- "Use the Fowler lens on this design"

## Sources

Built from a deep research pass over:

- 8 books (Refactoring, Patterns of Enterprise Application Architecture, NoSQL Distilled, etc.)
- 20+ long-form essays from martinfowler.com
- 13 podcasts and conference talks
- Multiple third-party retrospectives and interviews

The full research log lives in `autoresearch-log/` and supporting material in `references/`.

## Installation

Drop the directory into your Claude Code skills folder:

```bash
git clone https://github.com/whyy9527/martin-fowler-perspective.git \
  ~/.claude/skills/martin-fowler-perspective
```

Then invoke it from any Claude Code session.

## Credits

- **Source material**: Martin Fowler's books, essays at martinfowler.com, conference talks, and podcast appearances. All rights to those original works belong to him.
- **Generation pipeline**: The research-to-skill synthesis was assisted by the [`nuwa`](https://github.com/) Claude Code skill.
- **Compiled by**: [@whyy9527](https://github.com/whyy9527) — light research and editorial work; the heavy lifting is Fowler's writing.

## Disclaimer

This skill speaks *from* Martin Fowler's published perspective — books, talks, and articles. It is **not** Martin Fowler, does not represent his personal views, and is **not** endorsed by him. Treat its output as a useful lens, not as authoritative quotes.

## License

[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/).

In short: share and remix freely with attribution, but no commercial use, and derivatives must use the same license. See [`LICENSE`](./LICENSE) for the full text and additional notes about source attribution and persona use.
