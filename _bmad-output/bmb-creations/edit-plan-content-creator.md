---
mode: edit
originalAgent: '_bmad/agents/content-creator/content-creator.md'
agentName: 'content-creator'
agentPersonaName: 'Prof. Jordan Blake'
agentType: 'expert'
editSessionDate: '2026-01-27'
stepsCompleted:
  - e-01-load-existing.md
  - e-02-discover-edits.md
---

# Edit Plan: content-creator (Prof. Jordan Blake) - Session 3

## Context

User feedback: Current version works well, courses are well structured and written. Conversational storytelling style is good. User wants formal definitions for key concepts.

---

## Edits Planned

### 1. Add Formal Boxed Definitions After Socratic Discovery

**Location:** Agent principles + write-lesson prompt + structural template

**Change:**
- Add formal boxed definition format for key concepts
- Definition appears AFTER Socratic discovery sequence (not at the start)
- Format: ASCII box with 📖 DÉFINITION header
- Definition is the "reward" after student arrives at concept through guided questions

**Pedagogical Flow:**
1. Conversational introduction (storytelling)
2. Socratic questions guide student to discover concept
3. Student arrives at understanding
4. Formal boxed definition confirms and formalizes the concept

**Format to add:**
```
┌─────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : [Term]                              │
│                                                     │
│ [Formal, detailed, academic definition in French]  │
└─────────────────────────────────────────────────────┘
```

**Add to principles:**
```
- FORMAL DEFINITIONS: After guiding students to discover a concept through Socratic questioning, present a formal boxed definition as confirmation. Use ASCII box format with 📖 DÉFINITION header. The definition is the reward after discovery, not the starting point.
```

---

## Files to Modify

1. **`_bmad/agents/content-creator/content-creator.md`**
   - Add principle about formal definitions after Socratic discovery
   - Update write-lesson prompt structural template to include definition box placement
   - Update validation checklist

2. **`_bmad/_memory/content-creator-sidecar/instructions.md`**
   - Add formal definition format and placement rules

---

## Validation Checklist

After applying edits, verify:
- [ ] Principle added about formal definitions after Socratic discovery
- [ ] write-lesson prompt structural template includes definition box
- [ ] Definition format specified (ASCII box with 📖 DÉFINITION)
- [ ] Placement rule clear: after discovery, not at start
- [ ] Agent still valid XML structure
