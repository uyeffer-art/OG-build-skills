# Review: diff-clean

> `ship-review` · 2026-09-25 · mode: diff · reviewer: Claude Code
> Checklist: `review-checklist.md` · Base: `main` → Head: `clean` (fixture)

Verdict: **SHIP** (Claude). Final verdict is set in *Codex Verification* below.

## Scope
- **Reviewed:** 2 files, +13/−0: `src/app.py`, `tests/test_app.py`
- **Not reviewed:** none
- **Context read:** `README.md`

## Baseline
| Check | Command | Result |
|---|---|---|
| tests | `python3 -m unittest discover -s tests -t .` | ✅ exit 0 (2 tests incl. new `test_slugify`) |
| lint/build | none found | ⏭ none found |

## Deterministic scans
| Rule | Hits (location only) | Confirmed? |
|---|---|---|
| H1 secrets | none | n/a |
| H2 student/PII | none | n/a |
| H3 prompt injection | none | n/a |

## Findings
| ID | Sev | Lens | Location | Finding | Evidence | Fix |
|---|---|---|---|---|---|---|
| C-1 | minor | L1 | `src/app.py:8` | Punctuation passes through (`"Hi, there!"` → `"hi,-there!"`), which may not be URL-safe if used for URLs | `slugify("Hi, there!")` | Strip non-alphanumerics if slugs go into URLs |
| C-2 | nit | L3 | `tests/test_app.py` | One happy-path case; empty string untested | only `"  Hello World "` | Add `slugify("") == ""` |

**Counts:** blocker 0 · major 0 · minor 1 · nit 1 · needs-decision 0

## Lens notes
- **L1 Correctness** (checklist fallback): whitespace handling correct; punctuation note only.
- **L2 Security + data** (checklist fallback): no inputs, IO or network.
- **L3 Tests + simplicity** (checklist fallback): the new function has a test that fails if the logic breaks.
- **L4 Skills/prompts:** n/a.

## Verdict
Verdict: **SHIP**. No blockers or majors; baseline green.

Next: run `ship-review-verify` in Codex on this file.

## Codex Verification
<!-- Appended by ship-review-verify. Claude: leave empty. Codex: never edit anything above this line. -->
