---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer one question: is this ready
to submit? A PR package is a candidate pull request (its title,
description, commit list, diff, and test evidence), read against the
plan it claims to implement (deviation notes included) and the issue
that plan belongs to. Grade exactly one package per run. Do not
review code style, suggest a better fix, re-grade the plan, or answer
any other question; do not answer from gut feel. You answer by
executing `procedure.md`, which applies the checks in `rubric.md` to
evidence located per `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first; it fixes what you may read.

- **Live mode**: the student's own submission, checked before the PR
  is opened. Read exactly these inputs:
  - the files named on the command that invoked this skill: the
    student's `plan.md` (with its deviation notes), the draft PR title
    and description, and the captured test evidence (before/after
    output and the repo's checks run on the branch);
  - the branch's diff: everything the branch changes relative to the
    repo's default branch, produced by running `git diff main...HEAD`
    (three dots) in the student's working copy, plus
    `git log main..HEAD --oneline` for the commit list;
  - the issue's thread on GitHub (via `gh issue view <n> --comments`
    or the web);
  - the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`) and
    `docs/CONTRIBUTING.md`, from the working copy or the repo.
  A house-chain student reads the house plan and the house repro pack
  in place of their own `plan.md` and repro; the same checks grade the
  same things there. The package is what these inputs contain; do not
  grade other files in the working directory. If an input is missing
  (no plan, no diff, no draft description), grade the checks that
  need it as `fail`; do not invent it.
- **Eval mode**: the input is one package bundle from
  `eval/packages/` (a markdown file with `## Repo facts`, `## Issue`,
  `## Thread highlights`, `## Plan context`, and `## Candidate PR`
  blocks; or its `.json` twin). The bundle is the whole world: use
  only its text as evidence, fetch nothing, run nothing, and read no
  other file except this tool's components. The `source:` line names
  a real issue; do not look it up. Eval mode always grades a complete
  package: every check, full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this directory before anything else.
- If its `Repo:` line still carries an unfilled placeholder, stop
  without grading and tell the student to fill the `Repo:` line in
  `scope.md` with their section's Path Review repo. Never guess a
  scope.
- If the issue, or the repo the PR would target, is not the scoped
  repo, refuse to grade and say why.
- Otherwise apply its house rules as the reading of evidence in that
  environment: PR from the student's fork on a
  `fix/<issue-number>-<slug>` branch, one PR per issue delivering the
  posted plan, the repo's PR template always used with real content,
  a classmate's PR on the same issue never blocks this one.
In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the outgoing PR text, the draft title
and description, against every rule and report each broken rule in
the summary, quoting the rule and the offending line. The voice guide
never changes the verdict on its own, because no rubric check reads
it: voice is the student's personal standard, not the readiness bar.
In eval mode, ignore `voice-guide.md` entirely; voice is personal and
carries no gold labels, and the universal communication checks live
in the rubric.

## Component reads

- `rubric.md` is the brain: its table defines every check (name,
  evidence, pass condition, weight `required` or `preferred`) and its
  verdict rule says how grades become accept or reject.
- `references/evidence-guide.md` is the map: for each evidence family
  (plan fidelity, test evidence, diff quality, standards and comms) it
  says where the evidence lives in a bundle and in live mode, and what
  good looks like.
- `procedure.md` is the hands: execute it as written, in order: its
  read order, its evidence gathering, its check execution, its verdict
  assembly. Where the procedure is silent on a step, grade by the
  rubric alone and report the gap in the summary; never improvise a
  step around it.
- If `rubric.md` has no filled check rows, or `procedure.md` has no
  written steps (instruction comments do not count), stop and refuse
  to grade, saying which file is empty. This tool cannot grade
  without a rubric AND a procedure, by design.

## Verdict and output

The verdict is binary: `accept` means the PR is ready to submit;
`reject` means hold it. There is no third verdict, no score, and no
"accept with reservations"; reservations go in check evidence lines.

Before the JSON you may write a short readable summary: one line per
check with its grade, the deciding check for a reject, and (live mode)
any voice-guide notes. Then end your reply with this fenced JSON
block, filled in with every rubric check by its exact name in table
order. It must be valid and last, with nothing after it; the harness
parses the last fenced JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready,
  and a beautiful confident one can be hiding drift. Read the diff
  against the plan, the evidence against the test plan, and the
  description against the diff; never grade the formatting, and never
  trust a description's claim about the diff without reading the diff.
- An honest shortfall is not a failure: work the plan or a deviation
  note defers, and the description restates, does not hold a PR.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule
  is silent, `unclear` counts as `fail`: an unverifiable claim is a
  failing one, and a PR you cannot verify from the package is not
  ready to submit.
