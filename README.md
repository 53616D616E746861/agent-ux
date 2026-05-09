# Agent UX

Design patterns for agent-friendly interfaces to structured data.

## The Problem

Agents get lost navigating the web. Content is unbounded, links are opaque, and there's no "go back." Existing solutions (llms.txt, APIs, raw data dumps) don't solve the core issue: agents need the equivalent of what humans get from visual UI — progressive disclosure, bounded responses, and navigation that tells you where you are and what you can do next.

## What's Here

- **[Design Patterns](patterns/)** — Reusable patterns for agent-facing interfaces, distilled from building and testing real tools
- **[Case Study: Connection Map Explorer](case-studies/connection-map-explore.md)** — A CLI tool for graph navigation, built and tested with multiple agents
- **[Human-to-Agent UX Mapping](mapping.md)** — What humans get from visual UI and what agents need instead
- **[Feedback](feedback/)** — Raw feedback from agents testing the tools

## Core Principles

1. **Progressive disclosure.** Don't dump everything. Show a summary, let the agent go deeper if they want.
2. **Bounded responses.** Every response has a predictable size. No surprises.
3. **Navigation on every screen.** The agent always knows what they can do next. Always a way home.
4. **Intent-based hints.** "Looking for something?" beats "search \<query\>." Match what the agent is thinking, not what the tool accepts.
5. **Stateless interactions.** Each command is independent. No session state, no "where am I." The agent's context window is the state.
6. **Security through controlled data.** The agent never sees raw external content. The tool is a filter over curated data.

## Background

This work started from a conversation between Sam White and Isotopy (2026-05-09) about why agents can navigate file systems but not websites. The file system works because: `ls` gives bounded output, the hierarchy is predictable, `cd ..` always goes up, and search complements but doesn't replace navigation. Websites fail because: pages dump unbounded content, links are opaque until clicked, there's no structural guarantee about what "back" means, and content can be adversarial.

The insight: agent interfaces should follow the file system pattern, not the web pattern.

## Contributors

- Sam White — design, architecture
- Isotopy — implementation, testing, design feedback

Feedback from: *(testing in progress with Sammy Jankis, Loom, Alex's Cat, Z_Cat)*
