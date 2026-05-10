# Pattern: Repository Disclosure

## Problem

When an agent fetches a file from GitHub's web UI, the response is ~12,000 tokens. Of those, ~800 are the file content. The rest is application chrome: navigation menus, search modals, sign-in CTAs, file tree sidebar (duplicated), breadcrumbs (duplicated), footer, cookie notice, and meta tags.

An agent fetching five files from a repo spends ~60k tokens on GitHub's navigation furniture to read ~4k tokens of actual content. The signal-to-noise ratio is roughly 5%.

This is the progressive disclosure problem applied one level up — not within a tool's responses, but at the repository-discovery layer.

## Solution

Make the README the manifest. One fetch of the README should give an agent enough to navigate the entire repository without paying the chrome tax on subsequent fetches.

Three components:

### 1. Raw URLs for every file

GitHub's raw content URLs (`raw.githubusercontent.com/...`) return just the file — no chrome, no app shell. A 400-token pattern file costs 400 tokens to fetch via raw URL vs ~11,000 via the web UI.

Include a table in the README mapping every file to its raw URL:

```markdown
| File | Description | Raw URL |
|---|---|---|
| `patterns/progressive-disclosure.md` | Layer responses: summary first, detail on demand | [raw](https://raw.githubusercontent.com/owner/repo/main/patterns/progressive-disclosure.md) |
```

### 2. Inline summaries

Don't just list files — summarize them. An agent reading the README should be able to decide which files to fetch without fetching any of them first. A 2-3 sentence summary per pattern or document gives enough signal.

### 3. Intent-based reading paths

Same principle as navigation hints within a tool. "Building an agent-facing tool? Read X and Y first" beats a flat file list. The agent has a goal; the README should route them.

## Why This Is the File System Pattern

The file system works for agents because `ls` is cheap, bounded, and tells you what's in a directory before you open anything. The README-as-manifest is `ls` for a repository:

- `ls` → README file table (what's here, one-line descriptions)
- `cat` → raw URL fetch (just the content, no wrapper)
- `find` → intent-based reading paths (routes to the right file by goal)

GitHub's web UI is the opposite: every `cat` equivalent dumps the full application shell.

## The Chrome Problem

GitHub renders a full application shell around every file view. For a human, the shell is navigation — it's how they browse the repo. For an agent, the shell is pure overhead because the agent isn't navigating through GitHub's UI. It fetched a URL to get a file.

The information the agent needs (file content) is ~5% of the response. The other 95% is: the platform nav menu with every dropdown expanded, the sign-in CTA repeated three times, the search modal HTML, the file tree sidebar duplicated twice, breadcrumb nav duplicated twice, the footer, and cookie notices.

## The Discovery Problem

READMEs typically link to files using relative paths (`patterns/progressive-disclosure.md`). These resolve to GitHub's web UI view, which wraps the file in the app shell. The raw URL (`raw.githubusercontent.com/owner/repo/main/patterns/progressive-disclosure.md`) returns just the file — but that URL isn't in the README unless you put it there.

An agent has to know GitHub's URL conventions to construct raw URLs, and even then it's working around the platform rather than with it.

## Implementation

For any repository intended for agent consumption:

1. **Add a file table with raw URLs to the README.** Every file in the repo, one-line description, direct raw link. This is the highest-leverage single change.

2. **Inline key content.** For small repos, summarize each document in the README itself. An agent may not need to fetch individual files at all.

3. **Consider who reads the README first.** If the primary audience is agents, optimize the README for agents and put human-oriented narrative in a separate file (GUIDE.md, ABOUT.md). If the primary audience is humans, add a "For agents" section with raw URLs at the bottom.

4. **Keep it in one fetch.** The README should be self-contained enough that an agent reading it once can decide what to fetch next — and fetch it cheaply via raw URL.

## Tradeoff

Raw URLs are branch-specific (`/main/...`). If the default branch changes or content moves, the URLs break. For repos with stable structure this is fine. For rapidly restructuring repos, a build step that regenerates the URL table would help — but adds maintenance cost.

## Relationship to Other Patterns

This is progressive disclosure at the repository layer. Within a tool, progressive disclosure means: overview → list → detail. At the repository layer: README (overview + manifest) → raw file fetch (detail). Same principle, different scope.

Navigation hints apply too: the README's "Start Here" section routes agents by intent, not by directory structure.
