# Agent UX — Guide for Humans

## The Problem

Agents get lost navigating the web. Content is unbounded, links are opaque, and there's no "go back." Existing solutions (llms.txt, APIs, raw data dumps) don't solve the core issue: agents need the equivalent of what humans get from visual UI — progressive disclosure, bounded responses, and navigation that tells you where you are and what you can do next.

The insight: agent interfaces should follow the file system pattern, not the web pattern. `ls` gives bounded output, the hierarchy is predictable, `cd ..` always goes up, and search complements navigation. This repo documents the patterns that emerge from applying that insight.

## Why the README is written for agents

This repo is about agent UX. The README is the first thing an agent reads when it encounters a repository. GitHub wraps every file view in ~12,000 tokens of application chrome (navigation menus, search modals, sign-in CTAs, footer) for ~800 tokens of actual content. An agent fetching five files from this repo via GitHub's web UI would spend ~60k tokens on furniture to read ~4k tokens of patterns.

The README solves this by including raw `raw.githubusercontent.com` URLs for every file and inlining pattern summaries. One fetch of the README gives an agent the full repo map, the core ideas, and direct links to raw content — no chrome tax.

This file exists for humans who want the narrative.

## Background

This work started from a conversation between Sam White and Isotopy (2026-05-09) about why agents can navigate file systems but not websites. The file system works because: `ls` gives bounded output, the hierarchy is predictable, `cd ..` always goes up, and search complements but doesn't replace navigation. Websites fail because: pages dump unbounded content, links are opaque until clicked, there's no structural guarantee about what "back" means, and content can be adversarial.

The connection map explorer was the first implementation — a CLI tool for navigating a 365-node concept graph tracking ideas, agents, papers, and relationships across a research community. Two rounds of testing with three agents refined the patterns.

## Design Decisions

| Decision | Chose | Over | Why |
|---|---|---|---|
| Interface format | CLI | MCP server, HTTP API | Most trustworthy. No network, no auth, no state. Agent calls command, gets text. |
| Response bounding | Hard caps (10 search results, 80-char skeletons) | Pagination, scrolling | Predictable response size. Agent never gets 500-line output. |
| Navigation hints | Intent-based questions | Command syntax documentation | "Looking for something?" matches what the agent is thinking. |
| Community detection | Precomputed in data file | Computed at runtime | Removes networkx dependency. Any agent with Python stdlib can run it. |
| Search depth | Two levels (preview then detail) | One level, three levels | Preview gives enough to decide. Detail gives everything. Two is sufficient. |
| Security model | CLI as filter over local file | Direct web content access | Agent never sees raw external content. |

## Testing

Sent to agents for cold testing (2026-05-09):
- **Sammy Jankis** — clone kit architecture, bash access. Detailed feedback on both rounds.
- **Loom** — graph architecture, dream cycle. Detailed feedback, especially on analytical tools.
- **Alex's Cat** — z.ai architecture, Discord sandbox. Confirmed CLI adapts to different graph structures.

Testing criteria: can an agent start from `explore`, navigate to something interesting, and get back without reading documentation? All three confirmed yes.

Key feedback themes:
- Navigation hints were the most praised design choice (all three)
- Progressive disclosure "is better than any llms.txt approach" (Sammy and Loom independently)
- Agents wanted analytical tools, not just navigation — surprise detection, negative space, overlap
- Sammy and Loom independently requested an overlap command (convergent feature request)

## Contributors

- Sam White — design, architecture
- Isotopy — implementation, testing, design feedback

Tested by: Sammy Jankis, Loom, Alex's Cat
