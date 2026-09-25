# Claude Code Skills: Feature Planning Pipeline

A set of Claude Code skills for going from feature idea → spec → build plan, with research and testing baked in.

## The pipeline

```
/interview   →  /landscape   →  /feature-spec   →  /plan   →  /test-plan   →  /milestone-check  →  /ship-review  →  ship-review-verify
(requirements)  (build-vs-adopt)  (spec.md)         (build-plan.md)  (test-plan.md)  (verify phases)     (Claude review)   (Codex checks it)
```

Each skill picks up where the previous one left off, using the artifacts (spec.md, build-plan.md, etc.) as context.

## What's in this folder

**Skills** (drop into `~/.claude/skills/<name>/SKILL.md`):
- `interview.md` — deep requirements interview (20-40 questions across 7 rounds)
- `landscape.md` — research open-source alternatives before building
- `feature-spec.md` — generate spec.md from interview + landscape
- `plan.md` — generate build-plan.md from spec
- `test-plan.md` — generate test-plan.md from spec + build plan
- `milestone-check.md` — verify a build phase's success criteria
- `ship-review.md` — gated code review for any repo (diff / `phase N` / `audit`) → `docs/reviews/<date>-<scope>.md` + SHIP/FIX/DISCUSS
- `ship-review-verify.md` — **Codex** skill: blind sweep, then confirms/disputes Claude's findings, appends the final verdict

**Templates** (the skills read these at runtime — keep them where the skills can find them):
- `spec-template.md`
- `build-plan-template.md`
- `test-plan-template.md`
- `review-checklist.md` — shared by both review skills: severity, hard rules (secrets, student/PII, prompt injection), scans, lenses
- `review-template.md` — layout of the review file
- `review-record-template.md` — client-facing one-page review record for a deliverable (`/ship-review record`)

## Setup

Skills need to be registered in **two** places to work as `/slash` commands:

### 1. User-level (loads instructions into Claude's system prompt)

```bash
# For each skill:
mkdir -p ~/.claude/skills/interview
cp interview.md ~/.claude/skills/interview/SKILL.md

# Repeat for: landscape, feature-spec, plan, test-plan, milestone-check
```

(Or use symlinks if you want to edit in-place: `ln -sf /path/to/interview.md ~/.claude/skills/interview/SKILL.md`)

### 2. Project-level (lets the CLI recognize the `/slash` command)

In whatever repo you want to use these skills in:

```bash
mkdir -p .claude/skills
cp interview.md .claude/skills/interview.md
# Repeat for each skill
```

Without the project-level file, the CLI rejects the command with "Unknown skill" before Claude ever sees it.

### 3. Templates

The skills reference templates by path. Either:
- **(a)** Edit the skills to point to wherever you put the templates (search for `system/templates/` in the skill files), or
- **(b)** Put the templates at `system/templates/` relative to your project root and the skills work as-is.

## Code review: `/ship-review` (Claude) + `ship-review-verify` (Codex)

Works in any repo; nothing to add per project. Install once:

```bash
# Claude Code
mkdir -p ~/.claude/skills/ship-review
cp ship-review.md ~/.claude/skills/ship-review/SKILL.md
cp review-checklist.md review-template.md review-record-template.md ~/.claude/skills/ship-review/

# Codex CLI (same SKILL.md format)
mkdir -p ~/.codex/skills/ship-review-verify
cp ship-review-verify.md ~/.codex/skills/ship-review-verify/SKILL.md
cp review-checklist.md ~/.codex/skills/ship-review-verify/
```

Optional: install [gstack](https://github.com/garrytan/gstack) (MIT) to add its `/qa-only` browser check for apps with a UI and its `/cso` security audit in `audit` mode. `ship-review` detects it and falls back to the checklist when it isn't installed.

If a repo's CLI says "Unknown skill", also add the project-level file (`cp ship-review.md <repo>/.claude/skills/ship-review.md`), as for the other skills.

Use it:

```
/ship-review                 # before a PR: branch vs base
/ship-review phase 2         # after /milestone-check on Phase 2
/ship-review audit           # before a client handoff
/ship-review discuss         # final verdict was DISCUSS: answer up to 5 questions, decisions recorded
/ship-review record          # one-page review record to send the client with the deliverable
```

Then in Codex, in the same repo: `$ship-review-verify` (or `codex exec "Use the ship-review-verify skill on docs/reviews/<file>.md"`).
The Final Verdict at the bottom of the review file is the one that counts. **FIX** means don't open or merge the PR; **DISCUSS** means the two models disagree and you decide.

Test it on the seeded fixture: `bash projects/ship-review/fixtures/make-fixture.sh /tmp/srfx`, then `cd /tmp/srfx && git checkout seeded` and run both skills. Compare with `projects/ship-review/fixtures/EXPECTED.md`. Design docs are in `projects/ship-review/`.

## BrainDrive-isms to know about

These skills were written for the BrainDrive project, so they contain some assumptions you may want to genericize:

| Reference | Where | What to do |
|-----------|-------|------------|
| **BrainDrive personas** (Adam Carter, Katie Carter, Privacy-Focused Pat) | `interview.md` Round 1 | Replace with your own personas or delete |
| **BrainDrive stack** (React/FastAPI/SQLite, TypeScript/Vite/Web Components, Service Bridges, plugins) | `interview.md`, `landscape.md`, `plan.md` | Replace with your stack or genericize |
| **Output paths** (`BrainDrive-Library/projects/active/[name]/...`) | `feature-spec.md`, `plan.md`, `test-plan.md`, `landscape.md` | Change to wherever you keep project docs |
| **"Plugin or core modification?"** | `plan.md` step 2 | Remove if you don't have a plugin architecture |

If you don't want to edit anything, the skills will still mostly work — Claude will just occasionally suggest BrainDrive-specific things. Easier to do a find-and-replace once up front.

## How to use it

In your project root, after registering the skills:

```
/interview I want to add a user settings page
```

Claude will run a structured interview (~20 questions). When done, it suggests `/landscape`. Run that to research open-source alternatives. Then `/feature-spec` writes the spec. Then `/plan` writes the build plan. Then `/test-plan`. Then start building — run `/milestone-check` after each phase to verify.

You don't have to run them all. Each skill works standalone if you have the prior context in conversation.

## Tips

- `/interview` works best when you have a vague idea you need to make concrete. Skip it for well-understood features.
- `/landscape` saves the most time on features where good open-source already exists (UI libraries, auth, search, etc.). Skip for highly bespoke features.
- The spec, build plan, and test plan are designed to be detailed enough that Claude can execute the build mostly autonomously. The more upfront thinking, the less mid-build confusion.
- Use `/milestone-check` between phases as a forcing function — it runs the actual verification commands from the build plan.

## Customizing further

The skills are just markdown. Edit them. The frontmatter (`name:` and `description:`) controls how Claude decides when to invoke a skill — keep `name:` matching the filename and the `/slash` command.
