# Spec: [Feature Name]

> **Save to:** `BrainDrive-Library/projects/active/[project-name]/spec.md`
> Generated from `/interview` on [Date]

## Overview

### What We're Building
[Clear, concise description of the feature - 2-3 sentences]

### Target User
- **Persona:** [Owner / Builder / Entrepreneur]
- **Technical Level:** [Beginner / Intermediate / Advanced]
- **Context:** [When/where they'll use this feature]

### Problem Statement
[What pain point does this solve? What's the current workaround?]

## User Stories

> **Confirmed/Open convention:** Mark each user story with its status so agents know what's safe to build against.
> - **Confirmed (D##):** Requirement is settled. Reference the decision that confirmed it.
> - **Open:** Requirement is proposed but no decision has been made. May change.
> - **Deferred (D##):** Explicitly pushed to a future version.

### US-1: [Primary Flow Name] — **Confirmed** (D##)

As a [persona], I want to [action] so that [outcome].

<details>
<summary>Details — source, steps, acceptance criteria</summary>

**Source:** [Decision refs]

**Steps:**
1. User [action]
2. System [response]
3. User [action]
4. System [response]

**Acceptance Criteria (Given-When-Then):**

```gherkin
Given [initial context/state]
When [action performed]
Then [expected outcome]
And [additional outcome if applicable]
```

```gherkin
Given [edge case context]
When [action performed]
Then [expected behavior]
```

**Status:** Open

</details>

### US-2: [Secondary Flow Name] — **Open**

As a [persona], I want to [action] so that [outcome].

<details>
<summary>Details — source, steps, acceptance criteria</summary>

**Source:** [Decision refs]

**Acceptance Criteria:**

```gherkin
Given [context]
When [action]
Then [outcome]
```

**Status:** Open

</details>

### US-N: [Additional flows as needed]
[Add more user stories following the same pattern — title + summary exposed, details behind accordion]

## Invariants & Edge Cases

### Properties That Must Always Hold
> These drive property-based tests. State what must be true for ALL inputs, not just specific examples.

- [ ] [e.g., "Encrypting then decrypting any input returns the original"]
- [ ] [e.g., "A user can never access another user's data via this feature"]
- [ ] [e.g., "Saving and loading a configuration always preserves all fields"]

### Edge Cases to Test
- [ ] [Empty input / no data]
- [ ] [Maximum length / volume]
- [ ] [Invalid or malformed input]
- [ ] [Concurrent access / race conditions]
- [ ] [Network failure mid-operation]
- [ ] [Unicode, special characters, emoji]

### Failure Modes
| Scenario | Expected Behavior |
|----------|-------------------|
| [External service unavailable] | [Graceful degradation, retry, or error message] |
| [Invalid user input] | [Validation error with clear message, no data corruption] |
| [Partial operation failure] | [Rollback/cleanup, no orphaned state] |

## Detailed Requirements

### Core Functionality
- [ ] [Requirement 1 - be specific]
- [ ] [Requirement 2]
- [ ] [Requirement 3]

### User Interface
- [ ] [UI element 1 - describe appearance and behavior]
- [ ] [UI element 2]
- [ ] [UI element 3]

### Data & State
- [ ] [What data is stored?]
- [ ] [What persists across sessions?]
- [ ] [What is temporary?]

## Scope

> **Confirmed/Open convention for scope:**
> ```
> **Confirmed (D##):** [Scope item that has been decided]
> **Excluded (D##):** [Scope item explicitly excluded by decision]
> **Open:** [Scope item still under discussion — no decision yet]
> ```

### Feature Type
- [ ] **Prototype** - Proving feasibility, skip polish and edge cases
- [ ] **Production** - Full implementation with error handling and polish

### Implementation Location
- [ ] **Plugin** - Standalone capability via Service Bridges
- [ ] **Core Modification** - Changes to BrainDrive core (justify below)

**Justification for Core (if applicable):**
[Why can't this be a plugin?]

## MVP Scope (v1)

### Included
- [Essential feature 1]
- [Essential feature 2]
- [Minimum UI needed]

### Explicitly Excluded (v1)
- [Nice-to-have 1] → v2
- [Enhancement 2] → v2
- [Future idea] → later

## Future Versions

### v2 (Polish)
- [Feature 1]
- [Feature 2]

### v3 (Extended)
- [Feature 1]
- [Feature 2]

### Future Consideration
- [Long-term ideas]

## Technical Context

### Integration Points
- [ ] **API Bridge** - [What backend calls?]
- [ ] **Events Bridge** - [What events emit/listen?]
- [ ] **Theme Bridge** - [Theme-aware styling?]
- [ ] **Settings Bridge** - [User preferences?]
- [ ] **Page Context Bridge** - [Page metadata?]
- [ ] **Plugin State Bridge** - [Persistent state?]

### Dependencies
- [Existing BrainDrive features this depends on]
- [External APIs or services]
- [New packages needed]

### Constraints
- [Performance requirements]
- [Security considerations]
- [Compatibility requirements]

## Test Strategy

### Test Levels Required
- [ ] **Unit** — Pure logic, utilities, transformations, validators
- [ ] **Integration** — API endpoints, database operations, service interactions
- [ ] **Property-based** — Invariants from above (Hypothesis for Python, fast-check for TypeScript)
- [ ] **E2E** — Critical user flows from User Stories above

### Verification Approach
- **Agent self-verification:** [How will the coding agent verify its own work? Test suite? Browser testing? Staging environment?]
- **Human verification:** [What requires manual review? UX feel, visual design, architectural fit?]
- **Production monitoring:** [What metrics or alerts will tell us this feature is working? What indicates failure?]

### Baseline Impact
> Reference the working repo's actual verification docs, scripts, and CI config.
- **Always-run checks affected:** [Which baseline commands or required checks apply?]
- **Additional checks triggered:** [Which extra checks apply? e.g., migrations, frontend coverage, security scans]
- **Global properties affected:** [Which cross-cutting invariants does this feature touch?]

## Security Considerations

### Risk Level
- [ ] **Low** - No user input, no new APIs, no sensitive data
- [ ] **Medium** - Handles user input, new API endpoints, or stores user data
- [ ] **High** - Executes user code, touches auth/credentials, or exposes new network surfaces

### Threat Assessment
- **User input:** [Does this feature accept user input? How is it validated?]
- **Code execution:** [Does this feature run user-provided code? How is it sandboxed?]
- **Data sensitivity:** [What sensitive data is handled? How is it protected?]
- **Network surface:** [New APIs or external calls? Authentication required?]
- **Blast radius:** [If compromised, what is exposed? Single user or all users?]

### Required Mitigations
- [ ] [Mitigation 1 - e.g., input sanitization, rate limiting]
- [ ] [Mitigation 2 - e.g., sandbox isolation, egress filtering]

### Notes
[N/A for low-risk features. For medium/high, describe approach.]

## Explicit Boundaries

> **For AI Agents:** These boundaries define what is OUT OF SCOPE for this feature. Do not modify, touch, or refactor anything in these areas.

### Do Not Modify
- [ ] [File/folder path that should not be changed]
- [ ] [System component that is off-limits]
- [ ] [Database tables/schemas to leave alone]

### Do Not Introduce
- [ ] [Pattern or library to avoid]
- [ ] [Architectural approach that's not appropriate here]

### Security Boundaries
- [ ] [Never commit secrets or credentials]
- [ ] [Never modify authentication/authorization without explicit approval]
- [ ] [Production configs are read-only]

### Out of Scope (Even if Related)
- [ ] [Related feature that should NOT be implemented as part of this]
- [ ] [Refactoring that seems helpful but is not requested]
- [ ] [Performance optimization unless explicitly required]

## Open Questions

- [ ] [Question 1 - needs answer before proceeding]
- [ ] [Question 2]

## Success Definition

When this feature is complete, users will be able to:
1. [Outcome 1]
2. [Outcome 2]
3. [Outcome 3]

---

## Changelog

> Track how the spec evolved over time. Each row links to the decision or conversation that drove the change.

| Date | Change | Reason | Source | Decision |
|------|--------|--------|--------|----------|
| [YYYY-MM-DD] | Initial spec created | `/interview` + `/feature-spec` | [Interview session] | — |

## Conversation References

> Which conversations shaped this spec? Gives agents (and humans) a trail back to the original discussions.

| Date | Source | Topics Discussed | Link |
|------|--------|-----------------|------|
| [YYYY-MM-DD] | [Interview / Dev call / Chat session] | [Topics] | [transcript link or session ref] |

---

## Approval

- [ ] Reviewed by: _______________
- [ ] Date: _______________
- [ ] Ready for Planning: [ ]

---

*Next: Generate `build-plan.md` using the `/plan` skill*
