# content-creator-sidecar Memory

This folder stores persistent memory for the **Content Creator** agent.

## Purpose

Maintains lesson writing state and stores written lesson content.

## Files

- **writing-state.md**: Persistent state tracking lessons written, terminology consistency, style guidelines
- **instructions.md**: Agent-specific operating instructions and two-layer intelligence system rules
- **lessons/**: Directory containing written lesson markdown files

## Access Pattern

Agent accesses these files via:
- `{project-root}/_bmad/_memory/content-creator-sidecar/writing-state.md`
- `{project-root}/_bmad/_memory/content-creator-sidecar/instructions.md`
- `{project-root}/_bmad/_memory/content-creator-sidecar/lessons/{lesson-id}.md`

## Data Privacy

This sidecar directory is restricted - the agent can ONLY read/write files within this folder.
