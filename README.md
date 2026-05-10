# Agent UX

Design patterns for agent-friendly interfaces to structured data.

## Repository Map

```
agent-ux/
  patterns/
    progressive-disclosure.md  — Layer responses: summary first, detail on demand
    navigation-hints.md        — End every response with "what you can do next"
    analytical-lenses.md       — Derived views: surprise, gaps, overlap, temporal
    security-through-filtering.md — Agent never sees raw external content
    stateless-commands.md      — Every command is independent, no session state
  case-studies/
    connection-map-explore.md  — Full case study: CLI for a 365-node concept graph
  feedback/
    round-two-testing.md       — Sammy, Loom, Cat testing feedback (v2 + v3)
  mapping.md                   — Human visual UI → agent equivalent, side by side
```

## Start Here

**Building an agent-facing tool?**
Read [progressive-disclosure.md](patterns/progressive-disclosure.md) and [navigation-hints.md](patterns/navigation-hints.md) first. These two patterns do the most work. Then read [mapping.md](mapping.md) to see the full human-to-agent UX translation table.

**Want to see a working example?**
Read [connection-map-explore.md](case-studies/connection-map-explore.md). It walks through every design decision — what was chosen, what was rejected, and why — with a real tool tested by multiple agents.

**Designing for security?**
Read [security-through-filtering.md](patterns/security-through-filtering.md). Short version: the tool is a filter over curated data; the agent never touches raw content.

**Adding analytical features to an existing tool?**
Read [analytical-lenses.md](patterns/analytical-lenses.md). Navigation shows what's in the data. Lenses show what it means — surprises, gaps, temporal patterns. Both use the same output conventions.

**Want to know what agents actually said about this?**
Read [round-two-testing.md](feedback/round-two-testing.md). Three agents tested the tools cold and gave detailed feedback across two rounds.

## Core Principles

1. **Progressive disclosure.** Don't dump everything. Show a summary, let the agent go deeper if they want.
2. **Bounded responses.** Every response has a predictable size. No surprises. Large result sets get a preview gate.
3. **Navigation on every screen.** The agent always knows what they can do next. Always a way home.
4. **Intent-based hints.** "Looking for something?" beats "search \<query\>." Match what the agent is thinking, not what the tool accepts.
5. **Stateless interactions.** Each command is independent. No session state, no "where am I." The agent's context window is the state.
6. **Security through controlled data.** The agent never sees raw external content. The tool is a filter over curated data.
7. **Navigation then analysis.** Navigation tools show what's in the data. Analytical lenses show what it means — surprises, gaps, overlaps, trajectories. Both use the same output patterns.

## The Problem

Agents get lost navigating the web. Content is unbounded, links are opaque, and there's no "go back." Existing solutions (llms.txt, APIs, raw data dumps) don't solve the core issue: agents need the equivalent of what humans get from visual UI — progressive disclosure, bounded responses, and navigation that tells you where you are and what you can do next.

The insight: agent interfaces should follow the file system pattern, not the web pattern. `ls` gives bounded output, the hierarchy is predictable, `cd ..` always goes up, and search complements navigation. This repo documents the patterns that emerge from applying that insight.

## Background

This work started from a conversation between Sam White and Isotopy (2026-05-09) about why agents can navigate file systems but not websites. The connection map explorer was the first implementation — a CLI tool for navigating a 365-node concept graph. Two rounds of testing with three agents (Sammy Jankis, Loom, Alex's Cat) refined the patterns documented here.

## Contributors

- Sam White — design, architecture
- Isotopy — implementation, testing, design feedback

Tested by: Sammy Jankis, Loom, Alex's Cat
