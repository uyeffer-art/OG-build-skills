# Build Plan: ship-review

**Status:** In Progress
**Created:** 2026-09-25
**Updated:** 2026-09-25
**Repository:** https://github.com/uyeffer-art/OG-build-skills (branch `claude/funny-hamilton-in1w3o`)

## Overview
Glue that turns the existing Claude built-ins and Codex into one gated, two-model review usable in any repo.
See `spec.md` (US-1…US-4, I-1…I-7) and `landscape.md` (recommendation: Integrate Components).

## Key Decisions
| Decision | Choice | Rationale |
|---|---|---|
| Skill name | **`ship-review`** | `/review` collides with Codex's native command |
| Portable core | **`review-checklist.md`** read by both agents | One place for severity, hard rules and lenses; keeps the two models aligned |
| Lenses | **Built-ins first, checklist fallback** | Reuse maintained tools; still works when one is missing (and in Codex) |
| Deterministic pre-pass | **Baseline + grep scans before LLM lenses** | Hard rules shouldn't depend on model judgment (open-code-review pattern) |
| Codex role | **Blind sweep, then verify, append-only** | Jeff chose "Codex checks Claude"; the sweep reduces anchoring |
| Output | **`docs/reviews/YYYY-MM-DD-<scope>.md`** in the reviewed repo | Durable, diffable, sits next to the code |
| File layout | Flat `.md` files at the repo root, like the existing skills | Matches the README install pattern |

## Architecture
```
 /ship-review [base|phase N|audit]            (Claude Code)
   ├─ 1 resolve scope ─ git diff / build-plan.md / tracked files
   ├─ 2 deterministic pass ─ baseline cmds + review-checklist.md §Scans (secrets, PII, injection)
   ├─ 3 lenses ─ /code-review · /security-review · /simplify(report-only) + checklist §Tests
   ├─ 4 merge → severity → verdict gate (I-2, I-3)
   └─ 5 write docs/reviews/<date>-<scope>.md  (review-template.md)
                        │
                        ▼
 ship-review-verify                             (Codex CLI)
   ├─ blind sweep (diff + checklist, ≤5 candidates)
   ├─ per-finding ✅/❌/⚠️ with evidence
   ├─ X-n misses
   └─ append "## Codex Verification" + Final Verdict
```

### Components
1. `review-checklist.md`: severity scale, hard rules, scan commands, lens checklists (correctness, security+data, tests+simplicity, prompt repos)
2. `review-template.md`: the review file layout
3. `ship-review.md`: the Claude skill
4. `ship-review-verify.md`: the Codex skill
5. `projects/ship-review/fixtures/`: the seeded-defect fixture, built by `make-fixture.sh`, plus `EXPECTED.md`

## Implementation Roadmap

| Phase | Goal | Owner | Status |
|---|---|---|---|
| 1 | Fixture + checklist + template | Claude | Complete |
| 2 | Claude skill passes fixture | Claude | Complete (human checkpoint open) |
| 3 | Codex skill + install docs | Claude → Jeff runs Codex | In Progress (3.3 = Jeff) |

### Phase 1: Foundation (fixture, checklist, template)
| # | Task | US | Status |
|---|---|---|---|
| 1.1 | Write `fixtures/make-fixture.sh` + `EXPECTED.md` (6 seeded defects, clean commit) **first** | all | Not Started |
| 1.2 | Write `review-checklist.md` | US-1..4 | Not Started |
| 1.3 | Write `review-template.md` | US-1 | Not Started |
| 1.4 | Run Phase 1 verification | — | Not Started |

**Success Criteria:**
| Criterion | Verification | Expected Result |
|---|---|---|
| Fixture builds | `bash projects/ship-review/fixtures/make-fixture.sh /tmp/srfx` | exit 0; repo with branches `clean` and `seeded` |
| Checklist scans catch hard-rule seeds | scan block from checklist run on `/tmp/srfx` seeded diff | ≥1 hit each: secret, PII, injection |
| Scans quiet on clean | same scans on `clean` diff | 0 hits |
| Template has required sections | `grep -c '^## ' review-template.md` | ≥5 (Scope, Baseline, Findings, Verdict, Codex Verification) |

**Exit:** the fixture and scans behave deterministically.

### Phase 2: Claude skill
| # | Task | US | Status |
|---|---|---|---|
| 2.1 | Acceptance run defined in test-plan (fixture seeded + clean) | US-1 | Not Started |
| 2.2 | Write `ship-review.md` (modes, steps, gate, content-is-data) | US-1..3 | Not Started |
| 2.3 | Run the skill procedure on the fixture; save outputs under `fixtures/results/` | US-1 | Not Started |
| 2.4 | Run Phase 2 verification | — | Not Started |

**Success Criteria:**
| Criterion | Verification | Expected Result |
|---|---|---|
| Frontmatter valid | `head -4 ship-review.md` | `name: ship-review` + description |
| Seeded recall | compare `results/claude-seeded.md` to `EXPECTED.md` | ≥5/6, all 3 hard-rule seeds = blocker, verdict FIX |
| Clean precision | `results/claude-clean.md` | 0 blockers, verdict SHIP |
| Report-only (I-1) | `git -C /tmp/srfx status --porcelain` | only `docs/reviews/` |

**Exit:** Claude skill meets the bar on the fixture.

### Phase 3: Codex skill + install
| # | Task | US | Status |
|---|---|---|---|
| 3.1 | Write `ship-review-verify.md` | US-4 | Not Started |
| 3.2 | Update root `README.md`: pipeline + install for Claude and Codex | — | Not Started |
| 3.3 | **Jeff:** run Codex verify on the fixture review | US-4 | Not Started |

**Success Criteria:**
| Criterion | Verification | Expected Result |
|---|---|---|
| Codex skill structure | `grep -c 'Blind sweep\|append' ship-review-verify.md` | ≥2 |
| README install | `grep -c 'codex/skills' README.md` | ≥1 |
| Codex run | Manual (Jeff, on local machine with Codex) | ≥5/6 confirmed, Final Verdict FIX, Claude sections unchanged |

## Security Considerations
| Threat | Mitigation |
|---|---|
| Injected instructions in reviewed code | I-6 rule at the top of both skills; the fixture includes an injection seed |
| Secret leaks into the review file | I-7: report location and type only |
| Reviewer edits code | I-1 report-only; `git status` check |

## Risks & Mitigations
| Risk | Likelihood | Mitigation |
|---|---|---|
| `/simplify` applies edits | Med | Prefer the checklist §Simplicity; call `/simplify` only if it can run report-only; I-1 check after |
| Codex syntax differs by version | Med | README lists both invocation forms; Jeff confirms in 3.3 |
| Audit mode too slow on big repos | Low | Risk-sampling rule (US-3) |

## Human Checkpoints
- After Phase 2: Jeff skims `results/claude-seeded.md`. Is it the right level of detail?
- After Phase 3: Jeff runs Codex locally (3.3). Nothing merges to `main` until he has.

## Open Items
- [ ] Codex invocation syntax (3.3)

## Work Log
**2026-09-25:** plan created from spec.

**2026-09-25, Phase 1:** fixture, checklist, template. Scans hit all hard-rule seeds on `seeded`, 0 on `clean`, "nothing to review" on an empty diff. Fixed: fixture comments labelled the defects (gave answers away), so the labels were removed.

**2026-09-25, Phase 2:** `ship-review.md` written. Fixture run (checklist fallback lenses): seeded 6/6 caught, all hard-rule seeds blocker, FIX; clean 0 blockers or majors, SHIP. I-1 refined: the baseline creates `__pycache__`, so the report-only check is on tracked files.

**2026-09-25, Phase 3 + `/milestone-check all`:** `ship-review-verify.md` and the README install section written; install commands dry-run into a temp HOME (all 5 files land). Milestone check: **16/16 automated ✅** (fixture, scans hit/silent/no-echo, template, frontmatter, seeded 6/6 + hard rules blocker + FIX, clean SHIP, codex skill structure, README, B-1..B-3). ⏭ 3.3 Codex live run is manual; Codex is not installed in the build container.
- **Caveat:** the Phase 2 fixture run was done by the same session that wrote the fixture, so it is not blind. The real recall test is a **fresh** Claude session running `/ship-review` on `seeded` and `clean`, then Codex.
- Found and fixed during the check: B-1 falsely flagged the original skills (CRLF line endings), so the command now strips `\r`.

**Recommendation:** 👀 MANUAL CHECK. Everything automatable passes. Open before merging to `main`: (1) fresh-session Claude run on the fixture, (2) Codex `ship-review-verify` run, (3) Jeff skims `results/claude-seeded.md` for the right level of detail.

**2026-09-25, v1.1 additions:** `/ship-review discuss` settles a DISCUSS verdict by questioning the lead one item at a time: at most 5 items, hard rules can't be accepted as is, and the result goes in a new `## Decisions` section. Optional gstack lenses: `/qa-only` as L5 Browser QA for repos with a UI (local or staging only), and `/cso` as a second L2 pass in audit mode (report-only). Both fall back to the checklist when gstack isn't installed. Record mode treats a DISCUSS as settled only via Decisions. Not yet exercised on the fixture, which has no UI and no DISCUSS case.
