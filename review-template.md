# Review: [scope]

> `ship-review` · [YYYY-MM-DD] · mode: [diff | phase N | audit] · reviewer: Claude Code
> Checklist: `review-checklist.md` · Base: `[base-ref @ short-sha]` → Head: `[branch @ short-sha]`

Verdict: **[SHIP | FIX | DISCUSS]** (Claude). Final verdict is set in *Codex Verification* below.

## Scope
- **Reviewed:** [N files, +A/−D lines] · [commit list for phase mode]
- **Not reviewed:** [generated/vendored/binary files skipped; audit sampling rule if used]
- **Context read:** [spec.md / build-plan.md / test-plan.md / CLAUDE.md / AGENTS.md, or "none found"]

## Baseline
| Check | Command | Result |
|---|---|---|
| [tests] | `[cmd]` | ✅ exit 0 / ❌ exit N / ⏱ timeout / ⏭ none found |

## Deterministic scans
| Rule | Hits (location only) | Confirmed? |
|---|---|---|
| H1 secrets | [path:line, …, or none] | [yes / false positive: why] |
| H2 student/PII | … | … |
| H3 prompt injection | … | … |

## Findings
| ID | Sev | Lens | Location | Finding | Evidence | Fix |
|---|---|---|---|---|---|---|
| C-1 | blocker | L2/H1 | `path:line` | … | `…` (value masked) | … |

**Counts:** blocker [n] · major [n] · minor [n] · nit [n] · needs-decision [n]

## Lens notes
- **L1 Correctness** ([/code-review | checklist fallback]): [one line: what was checked, anything notable]
- **L2 Security + data** ([/security-review | checklist fallback]): …
- **L3 Tests + simplicity** ([/simplify report-only | checklist fallback]): …
- **L4 Skills/prompts** ([applied | n/a]): …
- **L5 Browser QA** ([gstack /qa-only against <local/staging URL> | checklist fallback, not run in a browser | n/a, no UI]): …
- **Audit only, /cso** ([run, report-only | gstack not installed | n/a, not audit]): …

## Verdict
Verdict: **[SHIP | FIX | DISCUSS]**. [One sentence: why, citing the gating finding IDs or the red baseline.]

Next: run `ship-review-verify` in Codex on this file.

## Codex Verification
<!-- Appended by ship-review-verify. Claude: leave empty. Codex: never edit anything above this line. -->

## Decisions
<!-- Written only by `/ship-review discuss` when the final verdict is DISCUSS. Otherwise leave empty. -->
