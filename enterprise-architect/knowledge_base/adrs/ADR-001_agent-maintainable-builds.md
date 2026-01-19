# ADR-001: Agent-Maintainable Builds

**Status:** Approved
**Date:** 2026-01-20
**Approved:** 2026-01-20
**Author:** Enterprise Architect
**Deciders:** {YOUR_NAME}, Archie (System Architect)

---

## Context

The {YOUR_NAME} Digital Estate uses a three-agent System Admin Team:
- **Archie** (Governance)
- **Enterprise Architect** (Design)
- **Bob** (Build)

Currently, when Bob builds infrastructure (Synology, Raspberry Pi, Home Assistant, etc.), the build context—the reasoning, troubleshooting, and decisions made during implementation—exists only in ephemeral conversation sessions. This creates two problems:

1. **Context loss**: Six months later, neither {YOUR_NAME} nor an agent can recall why a configuration was set a certain way, what alternatives were rejected, or how edge cases were handled.

2. **Maintenance burden**: Future changes require {YOUR_NAME} to re-explain the entire system, or the agent must rediscover context through exploration—both are inefficient.

### Observed Industry Pattern

Recent community builds (ref: "Second Brain" project, Jan 2026) demonstrated a principle:

> *"If the agent builds it, the agent can maintain it."*

When AI assists in construction and the conversation artifacts are preserved, the agent can be reinstantiated with full context. There is no switching cost. The documentation *is* the build process.

---

## Decision

**Adopt an "Agent-Maintainable Build" pattern for all infrastructure work in the {YOUR_NAME} Digital Estate.**

### Implementation

#### 1. Build Log Structure

Create a `build_logs/` folder under `system/admin/` (Bob's domain):

```
system/admin/
├── agent_config/
├── memory/
├── knowledge_base/
└── build_logs/                    # NEW
    └── {project-name}/
        ├── build_context.md       # Decisions, reasoning, rejected alternatives
        ├── implementation_log.md  # Step-by-step record of what was done
        └── maintenance_notes.md   # Known issues, gotchas, future considerations
```

#### 1a. Scope: Non-Bob Builds

Build logs for infrastructure work performed *outside* Bob's scope (e.g., {YOUR_NAME} configuring something directly, or another agent assisting) should be stored in the relevant domain's `knowledge_base/` folder, following the same template structure. Examples:

- Personal infrastructure → `personal/assistant/knowledge_base/build_logs/`
- {YOUR_COMPANY} infrastructure → `madeai/ceo/knowledge_base/build_logs/`

The template and maintenance workflow remain identical.

#### 2. Build Context Template

Every build MUST produce a `build_context.md`:

```markdown
# {Project Name} Build Context

**Date:** YYYY-MM-DD
**Built by:** Bob (with AI assistance)
**Design spec:** Link to EA design doc

## Objective
One sentence: what this build achieves.

## Key Decisions

| Decision | Options Considered | Chosen | Rationale |
|----------|-------------------|--------|-----------|
| ... | ... | ... | ... |

## Configuration Highlights
- Setting X was set to Y because...
- Port Z was used instead of default because...

## Rejected Alternatives
- Option A was rejected because...

## Dependencies
- Requires: [list upstream dependencies]
- Feeds into: [list downstream systems]

## Troubleshooting History
| Issue | Cause | Resolution |
|-------|-------|------------|
| ... | ... | ... |

## Future Maintenance Notes
- If X breaks, check Y first
- When upgrading, be aware of Z
```

#### 3. Conversation Export (When Applicable)

For complex builds, Bob SHOULD request to save a conversation summary to `implementation_log.md`. This captures the "how we got here" that raw configs cannot.

#### 4. Maintenance Workflow

When future maintenance is needed:

1. Load the relevant `build_logs/{project}/` files
2. Agent has immediate context—no re-explanation required
3. Update `maintenance_notes.md` with new learnings
4. Commit changes

---

## Consequences

### Benefits

| Benefit | Description |
|---------|-------------|
| **Zero context switching** | Agent can resume maintenance with full history |
| **Living documentation** | Build process IS the documentation |
| **Reduced {YOUR_NAME} burden** | No need to re-explain systems |
| **Pattern capture** | Decisions and rationale preserved for future reference |
| **Audit trail** | Clear record of what was built and why |

### Trade-offs

| Trade-off | Mitigation |
|-----------|------------|
| **Additional overhead** | Keep templates lightweight; Bob can generate most content |
| **Storage growth** | Text files are small; archive old projects annually |
| **Discipline required** | Archie can audit for compliance; add to build checklist |

### Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Builds proceed without logging | Medium | High | Add to Bob's standing instructions; Archie audit |
| Logs become stale | Low | Medium | Review during maintenance; timestamp entries |

---

## Compliance

This ADR aligns with:
- **SYSTEM.md Section 3** (Memory Protocol): Build logs follow explicit approval pattern
- **SYSTEM.md Section 4** (File Storage): Centralised under system/admin/
- **TOGAF**: Architecture Repository concept—capturing building blocks and decisions

---

## Action Items

If approved:

1. [ ] Create `system/admin/build_logs/` folder structure
2. [ ] Add build log requirement to Bob's `prompt.md`
3. [ ] Update Archie's audit scope to check for build logs
4. [ ] Backfill existing infrastructure projects (prioritise: Synology, Pi, Home Assistant)

---

## References

- Source insight: Community "Second Brain" builds, January 2026
- TOGAF 9.2: Architecture Repository, Building Blocks
- SYSTEM.md v1.4.0: Memory Protocol, File Storage Rules

---

## Approval Record

| Date | Action | By |
|------|--------|-----|
| 2026-01-20 | Drafted | Enterprise Architect |
| 2026-01-20 | Reviewed, refinement requested (non-Bob scope) | Archie |
| 2026-01-20 | Approved with refinement | {YOUR_NAME} |
