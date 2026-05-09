# Pattern: Analytical Lenses

## Problem

Navigation tools (search, browse, drill down) show agents what's in a graph. But agents also need to understand what the graph *means* — where the unexpected connections are, what's missing, where independent work converges. These questions can't be answered by traversal alone.

## Solution

Provide analytical commands that compute derived views over the same data. Each "lens" transforms the graph into a different perspective:

- **Surprise lens.** What connections exist that the embedding model wouldn't predict? Find curated edges between semantically distant nodes. These are the structurally important connections that simple similarity search would never surface.
- **Negative space lens.** What *isn't* connected? For a given agent or node, show communities, types, and high-degree nodes with no edges. The absence is informative — it reveals blind spots, unexplored territory, or deliberate scope boundaries.
- **Overlap lens.** Where did two agents contribute independently to the same concept space? Not just co-authorship (that's a curated edge) but convergent work — nodes from different origins in the same cluster or with shared neighbors.
- **Temporal lens.** When did concepts enter the graph? A snapshot shows neighbors as simultaneous, but the order matters — each concept built on previous ones. The trajectory, not just the snapshot.

## Why This Works for Agents

- **Complements navigation.** Navigation shows what's here. Analysis shows what it means. An agent navigates to find the question, then uses a lens to investigate it.
- **Drives curiosity.** "You're connected to X but the model wouldn't have predicted it" is a thread to pull. "You have no presence in C1" surfaces a blind spot to investigate. These outputs are invitations, not answers.
- **Stays bounded.** Each lens produces a ranked list with a natural cutoff. Surprise: top 10 by ascending similarity. Negative space: top communities by absence. No unbounded output.
- **Composable with navigation.** A surprise result links to `node <name>`. A negative space gap links to `community <id>`. The agent flows from analysis back into navigation seamlessly.

## Design Principle: Lenses Are Complementary Pairs

Lenses work best when they reveal two sides of the same question:

| Lens A | Lens B | What they reveal together |
|---|---|---|
| Surprise (unexpected presences) | Negative space (unexpected absences) | Full picture of what's anomalous |
| Overlap (convergent work) | Temporal (sequence of appearance) | Whether convergence was parallel or sequential |

An agent running one lens is likely to want the other. Navigation hints should suggest the complement.

## Implementation Notes

- Lenses should use existing data (similarity weights, community assignments, origin fields) rather than requiring new computation.
- Output format should match the navigation tool's style — same header format, same navigation hints at the bottom, same bounded response size.
- Each lens result should offer a way to drill deeper using existing navigation commands.
- The preview gate applies: if a lens would return a large result set, show top N with opt-in for full list.
