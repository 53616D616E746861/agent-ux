# Human-to-Agent UX Mapping

What humans get from visual interfaces and what agents need instead.

## Visual → Sequential

| What humans see | What it gives them | Agent equivalent | Status |
|---|---|---|---|
| Landing page with sections | Orientation — what's here, how big, where to start | Framing sentence + stats + suggested commands | Built |
| Navigation bar / sidebar | Always-visible options | Navigation hints on every response | Built |
| Back button / breadcrumbs | Return to previous context | "Back to home?" + contextual breadcrumbs (community ID on node view, seed names on subgraph) | Built |
| Search results with snippets | Preview before committing to click | Two-level results: skeleton + top edges before full detail | Built |
| Hover tooltips | Quick preview without navigating | *(no equivalent yet — closest is search result preview)* | Gap |
| Filtering / faceting | Narrow results by attribute | `--type`, `--origin` flags on search/community | Not built |
| Sorting controls | Reorder by different criteria | `--sort degree\|alpha\|relevance` | Not built |
| "Related items" sidebar | Discovery — what else is relevant | Similar nodes in node detail (cosine similarity) | Built |
| Pagination | Bounded batches of results | Cap at 10 results + "N more" count | Built |
| Visual graph layout | Gestalt — density, clusters, bridges, outliers at a glance | Structural tools: anomaly-report, graph-overview, rankings | Built (KG), not yet in explore tool |
| Color-coded clusters | Group membership visible instantly | Community ID on every node + community command | Built |
| Node size = importance | Relative significance at a glance | Degree shown inline, structural role labels (hub/bridge/leaf/core) | Partial (degree yes, role labels KG only) |
| Edge labels on graph | Relationship type visible on connections | Curated edges with predicates in node detail + search results | Built |
| Click node → detail panel | Progressive disclosure | `node <name>` command | Built |
| Drag to explore neighborhood | Spatial exploration of surroundings | `subgraph <name> --hops N` | Built |
| Path highlighting | How two things connect | `path <from> -- <to>` | Built |
| Dashboard / overview panel | Health + key metrics at a glance | graph-overview, anomaly-report | Built (KG) |

## What Doesn't Map

Some human visual capabilities have no clean sequential equivalent:

- **Simultaneous pattern perception.** A human sees cluster density, outliers, and bridge nodes all at once. Agents get them sequentially. The ambient/overview/focused layering partially compensates.
- **Spatial memory.** Humans remember where things were on screen. Agents don't retain layout between commands. The navigation hints substitute for spatial memory.
- **Peripheral vision.** Humans notice changes in their visual periphery without looking directly. The KG self-poke and bridge patrol are the closest agent equivalent — surfacing things you weren't looking for.

## Design Lessons

1. **Two levels is enough.** Preview (search results with skeleton + edges) → detail (full node view). A human browser has snippet → page. Adding a third level adds complexity without value.
2. **The agent's context window IS the back button.** In a stateless tool, the agent remembers what they searched for. Navigation hints just need to remind them the command exists.
3. **Intent-based hints outperform command documentation.** "Looking for something?" is processed faster than "search \<query\> — find nodes by keyword." The question matches what the agent is already thinking.
4. **Suggested starting points solve the cold start.** An agent encountering a new dataset needs a first move. "Try: search basin key" beats "365 nodes, 8840 edges."
5. **Precompute what you can.** Community detection requires networkx. Embedding it in the data file makes the tool pure stdlib — any agent with Python can run it.
