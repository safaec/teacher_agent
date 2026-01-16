# instructional-designer-sidecar Memory

This folder stores persistent memory for the **Instructional Designer** agent.

## Purpose

Maintains pedagogy decisions across sessions for teaching methodology selection and module-specific project design.

## Files

- **pedagogy-state.md**: Persistent state tracking overall methodology, module-specific teaching approaches, and project designs
- **instructions.md**: Agent-specific operating instructions and behavioral guidelines

## Access Pattern

Agent accesses these files via:
- `{project-root}/_bmad/_memory/instructional-designer-sidecar/pedagogy-state.md`
- `{project-root}/_bmad/_memory/instructional-designer-sidecar/instructions.md`

## Data Privacy

This sidecar directory is restricted - the agent can ONLY read/write files within this folder.
