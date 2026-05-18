# Test Plan: [Feature Name]

> **Save to:** `BrainDrive-Library/projects/active/[project-name]/test-plan.md`
> Generated from `/test-plan` on [Date]
> **Spec:** `spec.md` | **Build Plan:** `build-plan.md`

**Status:** [Not Started / In Progress / Complete]

---

## Baseline Regression

> Define the baseline from the real working repo. Use its documented scripts, CI jobs, and verification commands instead of assuming a shared global baseline file.

### Always-Run Baseline

| # | Check | Command | Applies? |
|---|-------|---------|----------|
| B-1 | [Baseline check] | `[exact command from working repo]` | [Yes/No] |
| B-2 | [Baseline check] | `[exact command from working repo]` | [Yes/No] |
| B-3 | [Baseline check] | `[exact command from working repo]` | [Yes/No] |

### Additional Conditional Checks

| # | Check | Command | Applies? | Why |
|---|-------|---------|----------|-----|
| C-1 | [Conditional check] | `[exact command]` | [Yes/No] | [Why it applies] |
| C-2 | [Conditional check] | `[exact command]` | [Yes/No] | [Why it applies] |
| C-3 | [Conditional check] | `[exact command]` | [Yes/No] | [Why it applies] |

---

## Property Definitions

> Properties that must always hold for this feature. These drive property-based tests using Hypothesis (Python) or fast-check (TypeScript).

| ID | Property | Always True | Spec Reference |
|----|----------|-------------|----------------|
| P-1 | [Property name] | [Formal statement: `f(g(x)) == x` for all x] | [Invariant I-N] |
| P-2 | [Property name] | [Formal statement] | [Invariant I-N] |
| P-3 | [Property name] | [Formal statement] | [Invariant I-N] |

### Global Properties Affected

> List any cross-cutting invariants this feature touches. These should come from the project spec or the working repo's real architecture constraints.

| ID | Property | Affected? | How |
|----|----------|-----------|-----|
| G-1 | Auth isolation | [Yes/No] | [How this feature interacts with user boundaries] |
| G-2 | Encryption roundtrip | [Yes/No] | [Does this feature store/retrieve encrypted data?] |
| G-6 | Ownership enforcement | [Yes/No] | [Does this feature add new user-owned resources?] |

---

## Feature Tests by Phase

### Phase 1: [Phase Name from Build Plan]

**Write these tests BEFORE implementing Phase 1:**

| Test | Type | File | Verifies | Spec Reference |
|------|------|------|----------|----------------|
| [Test description] | Unit | `tests/test_[module].py` | [What it proves] | [US-N / AC-N] |
| [Test description] | Integration | `tests/test_[endpoint].py` | [What it proves] | [US-N / AC-N] |
| [Test description] | Property | `tests/test_[module]_properties.py` | [P-N from above] | [Invariant I-N] |

**Phase 1 Verification Commands:**

```bash
# Run phase 1 tests
[exact pytest/jest command]
# Expected: all tests green

# Run baseline
[exact always-run verification commands from the working repo]
```

### Phase 2: [Phase Name from Build Plan]

**Write these tests BEFORE implementing Phase 2:**

| Test | Type | File | Verifies | Spec Reference |
|------|------|------|----------|----------------|
| [Test description] | [Type] | [File] | [What it proves] | [Reference] |

**Phase 2 Verification Commands:**

```bash
[exact commands]
```

### Phase 3: [Phase Name from Build Plan]

**Write these tests BEFORE implementing Phase 3:**

| Test | Type | File | Verifies | Spec Reference |
|------|------|------|----------|----------------|
| [Test description] | [Type] | [File] | [What it proves] | [Reference] |

**Phase 3 Verification Commands:**

```bash
[exact commands]
```

---

## Edge Case Tests

> From the spec's "Edge Cases" section. These are specific scenarios that must be tested.

| # | Edge Case | Test Type | Expected Behavior |
|---|-----------|-----------|-------------------|
| E-1 | [Empty input] | [Unit/Integration] | [What should happen] |
| E-2 | [Maximum length input] | [Unit/Integration] | [What should happen] |
| E-3 | [Concurrent access] | [Integration] | [What should happen] |
| E-4 | [Network failure] | [Integration] | [What should happen] |

---

## Security Tests

> Required for features with Medium or High security risk level (from spec).

| # | Threat | Test | Expected Result |
|---|--------|------|-----------------|
| S-1 | [Threat from spec] | [How to test it] | [Expected secure behavior] |
| S-2 | [Threat from spec] | [How to test it] | [Expected secure behavior] |

---

## Test Architecture Summary

| Layer | Framework | Test Count | Files |
|-------|-----------|------------|-------|
| Unit (backend) | pytest | [N] | [List files] |
| Integration (backend) | pytest + AsyncClient | [N] | [List files] |
| Property (backend) | pytest + Hypothesis | [N] | [List files] |
| Unit (frontend) | Jest | [N] | [List files] |
| Integration (frontend) | Jest + testing-library | [N] | [List files] |
| Property (frontend) | Jest + fast-check | [N] | [List files] |
| E2E | [Playwright/None] | [N] | [List files] |

**Total new test files:** [N]
**Total new test cases:** [N]

---

## Verification Summary for Milestone Check

> Compiled list of all commands `/milestone-check` should run.

### Phase 1 Verification

```bash
# Feature tests
[command]
# Expected: [result]

# Baseline
[baseline commands]

# Additional conditional checks (if applicable)
[conditional commands]
```

### Phase 2 Verification

```bash
[commands]
```

### Phase 3 Verification

```bash
[commands]
```

### Final Verification (All Phases Complete)

```bash
# Full test suite
[exact full test command set]

# Full coverage report
[exact coverage commands]

# Build
[exact build commands]

# Security
[exact security verification commands]
```

---

## Open Items

- [ ] [Any test infrastructure needed but not yet set up]
- [ ] [Dependencies to install: hypothesis, fast-check, pytest-cov, etc.]
- [ ] [Test fixtures or mocks that need discussion]

---

*Next: Build Phase 1. Write the tests listed above FIRST, then implement until they pass. Run `/milestone-check 1` to verify.*
