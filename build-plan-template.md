# Build Plan: [Feature Name]

> **Save to:** `BrainDrive-Library/projects/active/[project-name]/build-plan.md`

**Status:** [Not Started / In Progress / Complete]
**Created:** [Date]
**Updated:** [Date]
**Repository:** [Link if applicable]

---

## Overview

[2-3 sentence summary of what we're building and why.]

See `spec.md` for detailed requirements and user stories.

---

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| [Decision 1] | **[Choice]** | [Why this choice] |
| [Decision 2] | **[Choice]** | [Why this choice] |
| [Decision 3] | **[Choice]** | [Why this choice] |

---

## Architecture

### Component Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     [System Name]                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ┌──────────────┐     ┌──────────────────────────┐         │
│   │              │     │                          │         │
│   │  Component A │────▶│      Component B         │         │
│   │              │     │                          │         │
│   └──────────────┘     └────────────┬─────────────┘         │
│                                      │                       │
└──────────────────────────────────────┼───────────────────────┘
                                       │
                                       ▼
                          ┌────────────────────────┐
                          │      External API      │
                          └────────────────────────┘
```

### Components

#### 1. [Component Name]
- **Purpose:** [What it does]
- **Location:** `path/to/component/`
- **Key files:** [List main files]

#### 2. [Component Name]
- **Purpose:** [What it does]
- **Location:** `path/to/component/`
- **Key files:** [List main files]

### Data Flow

1. User [action]
2. [Component A] [processes/routes]
3. [Component B] [handles/stores]
4. Response flows back to user

---

## Implementation Roadmap

### Schedule Overview

| Phase | Goal | Owner | Status |
|-------|------|-------|--------|
| 1 | [Goal] | [Name] | Not Started |
| 2 | [Goal] | [Name] | Not Started |
| 3 | [Goal] | [Name] | Not Started |

### Phase 1: [Name]

**Goal:** [One sentence describing what this phase accomplishes]

**Tasks:**
> Tests-first: write tests before implementation. See `test-plan.md` for test details.
> Include US-# references so progress can be traced back to spec requirements.

| # | Task | US | Owner | Estimate | Status |
|---|------|----|-------|----------|--------|
| 1.1 | Write tests for Phase 1 acceptance criteria | — | [Name] | — | Not Started |
| 1.2 | [Implementation task 1] | US-1, US-3 | [Name] | [X days] | Not Started |
| 1.3 | [Implementation task 2] | US-2 | [Name] | [X days] | Not Started |
| 1.4 | Run Phase 1 verification | — | [Name] | — | Not Started |

**Success Criteria:**

| Criterion | Verification | Expected Result |
|-----------|--------------|-----------------|
| Phase 1 tests pass | `[test command from test-plan]` | All green |
| [Feature criterion] | `[command]` | [Expected output] |
| Baseline checks pass | `[always-run verification commands from working repo]` | All green |

**Exit Criteria:** [One sentence: what must be true to move to Phase 2]

### Phase 2: [Name]

**Goal:** [One sentence]

**Tasks:**

| # | Task | US | Owner | Estimate | Status |
|---|------|----|-------|----------|--------|
| 2.1 | Write tests for Phase 2 acceptance criteria | — | [Name] | — | Not Started |
| 2.2 | [Implementation task 1] | US-# | [Name] | [X days] | Not Started |
| 2.3 | [Implementation task 2] | US-# | [Name] | [X days] | Not Started |
| 2.4 | Run Phase 2 verification | — | [Name] | — | Not Started |

**Success Criteria:**

| Criterion | Verification | Expected Result |
|-----------|--------------|-----------------|
| Phase 2 tests pass | `[test command]` | All green |
| [Feature criterion] | `[command]` | [Expected output] |
| Baseline checks pass | `[always-run verification commands from working repo]` | All green |

**Exit Criteria:** [What must be true to move to Phase 3]

### Phase 3: [Name]

**Goal:** [One sentence]

**Tasks:**

| # | Task | US | Owner | Estimate | Status |
|---|------|----|-------|----------|--------|
| 3.1 | Write tests for Phase 3 acceptance criteria | — | [Name] | — | Not Started |
| 3.2 | [Implementation task 1] | US-# | [Name] | [X days] | Not Started |
| 3.3 | Coverage audit — fill gaps identified by `/milestone-check` | — | [Name] | — | Not Started |
| 3.4 | Run final verification (all phases + full baseline) | — | [Name] | — | Not Started |

**Success Criteria:**

| Criterion | Verification | Expected Result |
|-----------|--------------|-----------------|
| All tests pass | `[full test command]` | All green |
| Coverage gate | `[coverage command]` | >= threshold |
| [Feature criterion] | `[command]` | [Expected output] |
| Full baseline passes | `[full verification command set from working repo]` | All green |

**Exit Criteria:** [What must be true to consider feature complete]

---

## Technical Details

### Tech Stack

| Layer | Technology | Notes |
|-------|------------|-------|
| Frontend | [React/Vue/etc.] | [Any notes] |
| Backend | [FastAPI/Node/etc.] | [Any notes] |
| Database | [SQLite/Postgres/etc.] | [Any notes] |

### Environment & Version Constraints

> Front-load version info to prevent AI from drifting to incompatible versions.

| Dependency | Required Version | Notes |
|------------|------------------|-------|
| Node.js | 16+ | [Any constraints] |
| Python | 3.11 | [Any constraints] |
| React | 18.2 / 18.3 | Avoid v19 unless explicit |
| Git | Latest | Required for cloning |
| [Other] | [Version] | [Notes] |

**Why this matters:** Pinning versions prevents AI agents from drifting to incompatible versions during implementation.

### Database Schema (if applicable)

```python
# Example SQLModel schema
class FeatureItem(SQLModel, table=True):
    __tablename__ = "feature_items"

    id: Optional[int] = Field(default=None, primary_key=True)
    name: str = Field(max_length=255)
    created_at: datetime = Field(default_factory=datetime.utcnow)
```

### API Endpoints (if applicable)

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/v1/feature/create` | Create new item |
| GET | `/api/v1/feature/{id}` | Get item by ID |
| PUT | `/api/v1/feature/{id}` | Update item |
| DELETE | `/api/v1/feature/{id}` | Delete item |

### Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| [package] | [version] | [why needed] |

---

## Security Considerations

| Threat | Mitigation |
|--------|------------|
| [Threat 1] | [How we handle it] |
| [Threat 2] | [How we handle it] |

---

## Risks & Mitigations

| Risk | Likelihood | Mitigation |
|------|------------|------------|
| [Risk 1] | Low/Med/High | [How to handle] |
| [Risk 2] | Low/Med/High | [How to handle] |

---

## Open Items

- [ ] [Question or decision needed]
- [ ] [Research needed]

---

## Completion Checklist

### Code
- [ ] All phases complete
- [ ] All tests passing
- [ ] No linting errors
- [ ] Code reviewed

### Documentation
- [ ] CLAUDE.md updated (if structure changed)
- [ ] README updated (if applicable)

### Deployment
- [ ] Database migrations run (if applicable)
- [ ] Environment variables documented
- [ ] Feature tested in staging

---

## Human Checkpoints

### After Phase 1
- [ ] [Review item]

### After Phase 2
- [ ] [Review item]

### After Phase 3
- [ ] Final review before merge

---

## Work Log & Changelog

> Track execution details so any AI looking at this later knows what was built.

### Changelog

| Date | Change | Details |
|------|--------|---------|
| [YYYY-MM-DD] | Initial plan created | [Brief description] |
| [YYYY-MM-DD] | [Phase X completed] | [What was done, key decisions] |
| [YYYY-MM-DD] | [Issue fixed] | [What was wrong, how it was resolved] |

### Work Log

**[YYYY-MM-DD] - [Phase/Task Name]**
- What was attempted:
- What worked:
- What didn't work:
- Decisions made:
- Lessons learned:

---

## Conversation References

> Link back to AI conversations and transcripts that informed this plan

| Date | Source | Topic | Location |
|------|--------|-------|----------|
| [YYYY-MM-DD] | [Claude/ChatGPT/Codex/Call] | [What was discussed] | [Link or path to transcript] |

**Related Transcripts:**
- `BrainDrive-Library/transcripts/YYYY-MM/[filename].md`

**Related AI Conversations:**
- [Description of conversation and where it's stored, if applicable]

---

## Notes

[Any additional context, learnings, or reference links]

---

*Next: Run `/test-plan` to define the testing strategy. Use `/milestone-check [phase]` to verify each phase. Run `/retro` after completion.*
