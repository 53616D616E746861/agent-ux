# Pattern: Navigation Hints

## Problem

Agents don't have spatial memory, browser history, or a back button. Without explicit guidance, they either get stuck (don't know what to do next), get lost (can't return to a previous view), or waste tool calls experimenting with commands.

## Solution

End every response with a navigation section that shows:
1. **Back/Up** — how to return to the previous level or home
2. **Lateral** — how to search or switch direction
3. **Forward** — how to go deeper into something shown in the current response

## Intent-Based Hints

Frame hints as questions the agent is likely thinking:

```
--- NAVIGATION ---
  Follow a connection?          → node Z_Cat
  What's nearby?                → subgraph Alex's Cat --hops 1
  Others in this cluster?       → community 2
  Looking for something else?   → search <query>
  Back to home?                 → explore
```

Not:

```
--- NAVIGATION ---
  node <name>             → full detail on one node
  subgraph <name> --hops N → neighborhood map
  community <id>          → community members
  search <query>          → keyword search
  explore                 → home
```

The first version matches the agent's mental state. The second is API documentation.

## Contextual Breadcrumbs

Even though the tool is stateless, responses can include contextual navigation by computing relevant context:

- **Node detail** shows which community the node belongs to → `community <id>` hint
- **Subgraph** shows seed nodes → `node <seed>` hint for each
- **Path** shows endpoints → `node <start>`, `node <end>` hints
- **Search** shows top result → `node <top result>` hint

The tool doesn't know HOW the agent got here, but it knows WHAT'S RELEVANT from here.

## The "Go Home" Rule

Every response must include a way to return to the top-level overview. This is the equivalent of a browser's home button or a file system's `cd /`. The agent should never be more than one command away from starting over.

## Suggested Starting Points

On the home/overview screen, include a "TRY" section with 2-3 concrete commands. Agents encountering a new dataset need a first move. Without it, they face a blank prompt with no obvious entry point.
