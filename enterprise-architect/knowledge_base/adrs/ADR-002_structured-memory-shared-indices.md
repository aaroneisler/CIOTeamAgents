# ADR-002: Structured Memory & Shared Indices

**Status:** Approved
**Date:** 2026-01-20
**Approved:** 2026-01-20
**Author:** Enterprise Architect
**Deciders:** {YOUR_NAME}, Archie (System Architect)

---

## Context

Currently, agent memory is stored as unstructured markdown in `memory/semantic_memory.md` and `memory/episodic_memory.md`. This creates several problems:

1. **Poor retrieval**: Finding specific information requires reading entire files
2. **No cross-agent awareness**: Agents cannot see relevant context from other domains
3. **No compounding**: Knowledge captured in one session doesn't surface in future sessions
4. **Human taxonomy burden**: {YOUR_NAME} must choose which agent to talk to, acting as the classifier

### Observed Pattern

The "Second Brain" architecture (community builds, Jan 2026) demonstrated that AI-driven classification and structured storage creates compounding value over time. Key insight:

> *"Routing over organising. Let AI classify; humans hate taxonomy work."*

---

## Decision

**Implement Structured Memory & Shared Indices across all agents.**

Each agent:
1. Classifies conversation content into structured categories
2. Stores to its own structured memory (YAML format)
3. Decides what is cross-domain relevant
4. Writes cross-domain items to shared indices

### 1. Agent Memory Structure (Enhanced)

Each agent's `memory/` folder gains structured files:

```
{agent}/
└── memory/
    ├── semantic_memory.md      # Existing (facts, preferences)
    ├── episodic_memory.md      # Existing (session summaries)
    ├── projects.yaml           # NEW: Structured project tracking
    ├── people.yaml             # NEW: People mentioned in this domain
    └── ideas.yaml              # NEW: Ideas captured
```

### 2. Shared Indices

Cross-domain items aggregate in `shared_resources/indices/`:

```
shared_resources/
└── indices/
    ├── people.yaml             # All people across domains
    ├── projects.yaml           # Cross-domain projects
    └── ideas.yaml              # Cross-domain ideas
```

### 3. Classification Categories

| Category | Fields | Stored In |
|----------|--------|-----------|
| **Project** | title, status, next_action, context, domain, agent | Agent `projects.yaml` + shared if cross-domain |
| **Person** | name, context, relationship, last_contact, follow_ups | Agent `people.yaml` + shared if cross-domain |
| **Idea** | title, one_liner, notes, tags | Agent `ideas.yaml` + shared if cross-domain |
| **Task** | title, due_date, status, context | Agent `projects.yaml` (as task within project) |
| **Fact** | (unstructured) | Agent `semantic_memory.md` (existing) |

### 4. Agent Classification Protocol

Added to each agent's prompt:

```markdown
## Memory Management

At natural breakpoints or session end:

1. **Classify** what was discussed:
   - Projects (status, next action)
   - People (context, follow-ups)
   - Ideas (one-liner summary)
   - Facts (preferences, decisions)

2. **Update own memory** using structured YAML format

3. **Share decision**: If cross-domain relevant:
   - Write to `shared_resources/indices/{category}.yaml`
   - Include source attribution (domain, agent, date)

4. **Ask before writing**: "Want me to save this to memory?"
```

### 5. YAML Schema

**projects.yaml:**
```yaml
projects:
  - id: {unique-id}
    title: "Project Title"
    status: pending|active|waiting|blocked|done
    next_action: "Specific executable action"
    context: "Background and notes"
    domain: system|personal|madeai
    agent: bob|assistant|ceo|etc
    created: YYYY-MM-DD
    updated: YYYY-MM-DD
```

**people.yaml:**
```yaml
people:
  - name: "Person Name"
    contexts:
      - domain: system|personal|madeai
        agent: bob|assistant|ceo|etc
        relationship: "How {YOUR_NAME} knows them"
        notes: "Relevant context"
        last_contact: YYYY-MM-DD
    follow_ups:
      - "Action item"
```

**ideas.yaml:**
```yaml
ideas:
  - id: {unique-id}
    title: "Idea Title"
    one_liner: "Core insight in one sentence"
    notes: "Elaboration"
    tags: [tag1, tag2]
    domain: system|personal|madeai
    agent: source-agent
    created: YYYY-MM-DD
```

---

## Consequences

### Benefits

| Benefit | Description |
|---------|-------------|
| **Structured retrieval** | Query by status, date, person, etc. |
| **Cross-agent awareness** | Agents see relevant context from other domains |
| **Compounding knowledge** | Past work surfaces in future sessions |
| **AI classification** | Agents do taxonomy work, not {YOUR_NAME} |
| **Audit trail** | Source attribution on all shared items |

### Trade-offs

| Trade-off | Mitigation |
|-----------|------------|
| More files to manage | Clear structure, templates provided |
| YAML learning curve | Simple format, examples in protocol |
| Merge conflicts possible | Each agent owns its files; shared indices append-only |

---

## Compliance

| SYSTEM.md Section | Status |
|-------------------|--------|
| Section 2 (Agent Structure) | Extended, not violated—adds optional files |
| Section 3 (Memory Protocol) | Compliant—still requires approval before storage |
| Section 6 (Shared Resources) | Compliant—indices in `shared_resources/` |

---

## Implementation

| Item | Owner | Status |
|------|-------|--------|
| Create `shared_resources/indices/` | Archie | This commit |
| Create index template files | Archie | This commit |
| Create memory management protocol | Archie | This commit |
| Update rules.json | Archie | This commit |
| Update individual agent prompts | Per agent, incremental | Future sessions |

---

## References

- ADR-001: Agent-Maintainable Builds
- Source: Second Brain architecture patterns (community builds, Jan 2026)
- SYSTEM.md v1.4.0: Memory Protocol, Shared Resources

---

## Approval Record

| Date | Action | By |
|------|--------|-----|
| 2026-01-20 | Designed | Enterprise Architect |
| 2026-01-20 | Approved | {YOUR_NAME} |
| 2026-01-20 | Implemented | Archie |
