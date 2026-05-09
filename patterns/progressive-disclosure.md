# Pattern: Progressive Disclosure

## Problem

Agents receiving all available information at once get overwhelmed. Large responses dilute attention, waste tokens, and make it harder to identify what matters.

## Solution

Structure responses in layers. Show a summary first, let the agent choose what to explore deeper.

## Layers

1. **Overview** — stats, categories, suggested starting points. The "what's here" view.
2. **List/Search** — bounded set of results with enough preview (skeleton, key edges) to decide which to explore.
3. **Detail** — full information on one item. All connections, full text, source URLs.

## Implementation

Each layer is a separate command. Responses include navigation hints pointing to both deeper layers (go in) and shallower layers (go back/home).

## Example: Connection Map Explorer

```
explore           → overview (365 nodes, 5 communities, top nodes)
  community 3     → list (67 nodes with type, degree, skeleton)
    node "Loom"   → detail (full summary, all edges, similar nodes, URLs)
```

```
search "basin key" → list (10 results with skeleton + top 5 edges each)
  node "basin key" → detail (full view)
```

## Key Decisions

- **Two levels for search, not three.** Search results show skeleton + edges (enough to decide). Node detail shows everything. A middle tier adds complexity without value.
- **Cap list results.** 10 for search, all for community (bounded by cluster size). The cap prevents unbounded responses while the "N more results" count tells the agent they haven't seen everything.
- **Truncate at the right level.** Skeletons are compressed summaries already — show them in full. Only truncate when displaying many items side by side (community listing).
