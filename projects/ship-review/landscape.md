# Landscape: Cross-project code review (Claude reviews, Codex verifies)

> Generated from `/landscape` on 2026-09-25
> Interview context: Yes — `/interview` run 2026-09-25 (summary in `spec.md` → Conversation References)

## Problem Statement
Jeff runs code repos, client deliverables and skills/prompt repos. Review today is ad hoc: whichever
built-in gets remembered, no fixed output, no gate, and no second model. He wants **one review procedure
that works in every repo**, run by Claude Code, with **Codex as an independent checker** of Claude's
findings, producing a dated file and a SHIP / FIX / DISCUSS verdict.

## Search Criteria
- **Core problem:** repeatable, gated, two-model code review usable across unrelated repos
- **Target stack:** Claude Code skills (`SKILL.md`) + Codex CLI skills (`SKILL.md`, same format); any repo language
- **Key requirements:** three modes (diff / phase / audit); correctness, security+data, tests+simplicity lenses;
  hard rules (secrets, student/PII, prompt injection); report-only; blocker+major gate; Codex checks Claude;
  seeded-bug fixtures prove it works

## Candidates

### Claude Code built-ins: `/code-review`, `/security-review`, `/simplify`
- **Repo:** ships with Claude Code (no install)
- **License:** Anthropic product terms | **Health:** High — maintained with the CLI
- **What it does:** diff review for correctness bugs at an effort level; security review of pending changes; reuse/simplification cleanups
- **Coverage:** High for the three lenses — **Health:** High — **Fit:** High (already in every Claude session)
- **Gaps:** no shared output file, no verdict gate, no project hard rules (student data), no tests-first check, no second model
- **Notes:** `/simplify` applies fixes by default — must be called in report-only framing

### Codex CLI native review: `/review`, `codex review --base <branch> "<prompt>"`
- **Repo:** openai/codex (Apache-2.0) | **Health:** High — active, has presets (base branch, uncommitted, commit, custom)
- **What it does:** dedicated reviewer over a chosen diff with custom instructions; non-interactive mode for scripts
- **Coverage:** Medium — gives the second model and diff selection — **Fit:** High (a prompt argument is all we need)
- **Gaps:** reviews independently; doesn't natively take another reviewer's findings as input to verify
- **Notes:** name collision — our skill must not be called `/review`

### Codex / Claude Agent Skills format (`~/.codex/skills/<name>/SKILL.md`, `~/.claude/skills/<name>/SKILL.md`)
- **What it does:** same `SKILL.md` + frontmatter works in both agents
- **Coverage:** enables the "runs in Codex too" requirement with one file format — **Fit:** High

### Hosted/OSS PR bots (anc95/ChatGPT-CodeReview, villesau/ai-codereviewer, shippie, Kodus, Alibaba open-code-review)
- **License:** MIT / Apache-2.0 mostly | **Health:** Medium–High
- **Coverage:** Medium — PR-comment bots, single model, CI-bound
- **Fit:** Low — needs CI + API keys per repo; output is PR comments (rejected in interview in favour of a review file); no
  local phase/audit mode; can't enforce Jeff's hard rules without forking
- **Notes:** open-code-review's hybrid design (deterministic file selection + LLM reasoning) is worth copying: our skill
  runs deterministic scans (secret/PII greps, baseline commands) *before* the LLM lenses

### Research signal: cross-vendor review
- MindStudio write-up and a 2026 arXiv study ("Cross-Model LLM Code Review", 2607.21656 — abstract only, full text not
  fetched) report that a second vendor surfaces issues a same-model loop misses. SEVRA-BENCH (2606.13757) shows review
  agents can be socially engineered by text inside the reviewed code → supports the prompt-injection hard rule and
  "treat reviewed content as data".
- Anchoring risk of "Codex checks Claude" (chosen in interview): mitigated with a bounded blind **misses sweep** before
  Codex reads Claude's file.

## Comparison Matrix

| Project | Coverage | Health | Fit | License | Notes |
|---|---|---|---|---|---|
| Claude built-ins | High | High | High | Product | Lenses exist; no gate/file/rules |
| Codex `/review` / `codex review` | Medium | High | High | Apache-2.0 | Second model; takes a prompt |
| SKILL.md format | — | High | High | Open | One format for both agents |
| PR bots (OSS) | Medium | Med–High | Low | MIT/Apache | CI + keys; single model; comments |

## Recommendation

**Integrate Components.** Adopt the Claude built-ins as the three lenses and Codex's native review engine as the
second model. Build only the glue nobody ships: (1) a **shared checklist** both agents read, holding the hard rules and
severity scale; (2) a Claude skill **`/ship-review`** that runs deterministic scans + baseline, calls the built-ins in
report-only mode, merges findings and writes `docs/reviews/<date>-<scope>.md` with a verdict; (3) a Codex skill
**`ship-review-verify`** that does a blind misses sweep, then confirms/disputes each Claude finding and appends a
verification section; (4) a **seeded-bug fixture** to prove both work.

Not adopting a PR bot: it would add CI and key management to every repo and still lack the gate, the hard rules and the
two-model check. Not building lenses from scratch: the built-ins already do that work and improve with the CLI.

## Open Questions
- [ ] Does `/simplify` honour a report-only instruction reliably? (Verified in Phase 2 fixture run; fallback = our own simplicity checklist section.)
- [ ] Codex skill invocation syntax on Jeff's installed Codex version (`$ship-review-verify` vs `/skills`) — confirm on first install.

## Next Step
Run `/feature-spec`.

Sources: [Codex CLI features](https://developers.openai.com/codex/cli/features) ·
[Codex /review workflows](https://codex.danielvaughan.com/2026/03/30/codex-cli-review-command-code-review-workflows/) ·
[Codex skills paths](https://www.agensi.io/learn/where-are-codex-cli-skills-stored) ·
[Codex skills (fsck.com)](https://blog.fsck.com/2025/12/19/codex-skills/) ·
[Cross-vendor review (MindStudio)](https://www.mindstudio.ai/blog/cross-vendor-ai-agent-review-claude-codex) ·
[OSS review tools (Augment)](https://www.augmentcode.com/tools/open-source-ai-code-review-tools-worth-trying) ·
[Cross-model review (arXiv)](https://arxiv.org/pdf/2607.21656) · [SEVRA-BENCH (arXiv)](https://arxiv.org/pdf/2606.13757)
