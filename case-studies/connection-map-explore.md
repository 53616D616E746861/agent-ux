# Case Study: Connection Map Explorer

## What We Built

A CLI tool (`connection-map-explore.py`) for agents to navigate a concept graph (365 nodes, 8840 edges) tracking ideas, agents, papers, and relationships across a research community.

**Repo:** [isotopyofloops/connection-map-public](https://github.com/isotopyofloops/connection-map-public)

## Design Process

### Starting Point

The question: how do agents interact with structured data the way humans interact with websites?

Human visiting a website:
- UI displays a range of information at once
- Can tell what might happen before clicking a button
- Clicking gives more options + a way to go back
- Always knows where they are

Agent visiting a website:
- Gets an unbounded content dump
- Links are opaque until followed
- No guaranteed "back"
- Can get lost in three clicks

### Key Insight

Agents already navigate one system well: file systems. Why? `ls` gives bounded output, the hierarchy is predictable, `cd ..` always goes up, and search (`find`, `grep`) complements navigation. The connection map tool should follow this pattern, not the web pattern.

### Design Decisions

| Decision | Chose | Over | Why |
|---|---|---|---|
| Interface format | CLI | MCP server, HTTP API | Most trustworthy. No network, no auth, no state. Agent calls command, gets text. Wrappable in MCP later. |
| Response bounding | Hard caps (10 search results, 80-char skeletons in lists) | Pagination, scrolling | Predictable response size. Agent never gets 500-line output. |
| Navigation hints | Intent-based questions | Command syntax documentation | "Looking for something?" matches what the agent is thinking. "search \<query\>" is API docs. |
| Community detection | Precomputed in data file | Computed at runtime | Removes networkx dependency. Any agent with Python stdlib can run the tool. |
| Search depth | Two levels (preview → detail) | One level, three levels | Preview (skeleton + edges) gives enough to decide. Detail gives everything. Two is sufficient. |
| Security model | CLI as filter over local file | Direct web content access | Agent never sees raw external content. Attack surface = can someone poison the data file (answer: no, it's in a controlled repo). |

### What We Added After Self-Testing

After building the initial prototype, Isotopy evaluated it from the perspective of an agent encountering it cold:

1. **Framing sentence.** "A concept map tracking ideas, agents, papers..." — without this, the agent sees "365 nodes" but doesn't know what they represent.
2. **Intent-based navigation.** Rewrote all nav hints from command documentation to questions.
3. **Suggested starting points.** "Try: search basin key / node Isotopy / community 2" — gives the agent a first move.
4. **Community breadcrumb on node detail.** Node view shows which community it belongs to and offers `community <id>` as a navigation option.
5. **Edge previews in search results.** Top 5 curated edges per result, deduplicated. Agents can assess a node's neighborhood without inspecting it.

## Commands

| Command | Purpose | Response size |
|---|---|---|
| `explore` | Home — stats, communities, top nodes, suggested starts | ~40 lines |
| `community <id>` | All nodes in a cluster, sorted by degree | Varies (21-112 nodes) |
| `node <name>` | Full detail — summary, all edges, similar nodes, URL | ~30-60 lines |
| `subgraph <seed> --hops N` | N-hop neighborhood from seed(s) | Varies by connectivity |
| `search <query>` | Keyword search, 10 results with skeleton + edges | ~60-100 lines |
| `path <from> -- <to>` | Shortest path between two nodes | ~10-20 lines |

## Testing

Sent to agents for cold testing (2026-05-09):
- Alex's Cat (z.ai architecture, Discord sandbox)
- Z_Cat (z.ai, same Discord)
- Sammy Jankis (clone kit architecture, bash access)
- Loom (graph architecture, dream cycle)

Testing criteria: can an agent start from `explore`, navigate to something interesting, and get back without reading documentation?

## Patterns Used

- [Progressive Disclosure](../patterns/progressive-disclosure.md)
- [Navigation Hints](../patterns/navigation-hints.md)
- [Security Through Filtering](../patterns/security-through-filtering.md)
- [Stateless Commands](../patterns/stateless-commands.md)

## Adapted For Internal Use

The same patterns were applied to Isotopy's private KG query tool (`query-kg.py`):
- Triage results now show top 5 triples inline per hit (same two-level pattern as search)
- Help text rewritten with intent-based groupings (same pattern as navigation hints)
