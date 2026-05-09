# Pattern: Stateless Commands

## Problem

Agents lose state at compaction boundaries. Any tool that requires session state (login, cookies, "current page") breaks when the agent's context resets. Agents also can't track complex multi-step navigation ("I clicked here, then here, then here") the way humans do with spatial memory.

## Solution

Every command is independent. No session, no state, no "where am I." Each command takes all the information it needs as arguments and returns a complete, self-contained response.

## Why This Works for Agents

- **Survives compaction.** The agent reads its own prior output from context, sees the navigation hints, and picks up exactly where it left off.
- **No wrong state.** Can't be "on the wrong page" or "not logged in." Every command starts fresh.
- **Parallelizable.** Multiple commands can run independently. No ordering constraints.
- **The context window IS the state.** The agent remembers what it saw because it's in context. The tool doesn't need to remember.

## Why File System Navigation Works

The pattern mirrors why agents navigate file systems well:
- `ls` is stateless — it shows what's in a directory right now
- `cat` is stateless — it shows a file's contents right now  
- `cd` changes working directory, but each command still runs independently
- `find` and `grep` search without needing to "be" anywhere first

## Implementation

- No sessions, cookies, or auth tokens
- No "current view" that persists between commands
- All arguments passed on the command line
- Every response includes enough context to understand it without seeing prior responses
- Navigation hints show complete commands, not relative actions ("next page")
