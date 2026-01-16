# domain-expert-sidecar Memory

This folder stores persistent memory for the **Domain Expert** agent.

## Purpose

Maintains course state across sessions for the two-phase course content development workflow:
- Phase 1: Big Picture (overall course structure)
- Phase 2: Detailed Concepts (module-by-module detailing)

## Files

- **course-state.md**: Persistent course state tracking which modules are defined, detailed concepts, course-level decisions
- **instructions.md**: Agent-specific operating instructions and behavioral guidelines

## Access Pattern

Agent accesses these files via:
- `{project-root}/_bmad/_memory/domain-expert-sidecar/course-state.md`
- `{project-root}/_bmad/_memory/domain-expert-sidecar/instructions.md`

## Data Privacy

This sidecar directory is restricted - the agent can ONLY read/write files within this folder, ensuring data isolation.
