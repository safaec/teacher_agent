# curriculum-architect-sidecar Memory

This folder stores persistent memory for the **Curriculum Architect** agent.

## Purpose

Maintains structural decisions across modules for curriculum organization.

## Files

- **structure-state.md**: Persistent state tracking chapter/lesson structures for modules
- **instructions.md**: Agent-specific operating instructions and heuristics

## Access Pattern

Agent accesses these files via:
- `{project-root}/_bmad/_memory/curriculum-architect-sidecar/structure-state.md`
- `{project-root}/_bmad/_memory/curriculum-architect-sidecar/instructions.md`

## Data Privacy

This sidecar directory is restricted - the agent can ONLY read/write files within this folder.
