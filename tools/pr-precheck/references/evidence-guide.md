# Evidence guide: where evidence lives in a PR package

Every eval package has the same top-level blocks, in this order:
`## Repo facts`, `## Issue`, `## Thread highlights (N comments
total)`, `## Plan context`, `## Candidate PR`. The candidate PR always
has `### Title`, `### Description`, `### Commits`, `### Diff`, and
`### Test evidence`. The plan context is prose: a `Repro evidence`
paragraph, then `Plan:` text whose scope, files, deferrals, and test
plan appear as inline phrases (`Scope`, `Files:`, `Out of scope`,
`Known limit`, `deferred`, `Test plan:`) or plain sentences. Find each
part by what it says, not by its label, and never grade a check down
because a label is missing.

In live mode the same parts live in: the issue page and thread on
GitHub; the student's `plan.md` (repro evidence it builds on, scope,
files, test plan, deviation notes); the branch's diff from
`git diff main...HEAD` and commits from `git log main..HEAD
--oneline`, both run in the working copy; the draft PR title and
description; the captured test output; and the repo's
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`.

## Plan fidelity (harness category: silent-drift)

Where it lives:

- What was promised: the `## Plan context` block (live: `plan.md`).
  Pull out the files it names, each change it promises (code, tests,
  docs, warnings), and everything it scopes out, defers, or lists as a
  known limit. A deviation note counts as part of the plan whether it
  sits in the plan or is restated in `### Description`.
- What was delivered: `### Diff`, file by file (`+++ b/<path>` lines)
  and hunk by hunk (`@@` lines). Live: `git diff main...HEAD`.
- What is claimed: `### Title` and `### Description`. Look for
  fidelity phrases ("per the plan", "exactly as planned", "implements
  the posted plan exactly", "no changes beyond the plan", "no
  functional changes outside ...") and statements of what was added
  ("docs now document", "tests added", "CHANGELOG entry added").

What good looks like: every hunk implements a promised change, a test
for it, a comment explaining it, or an entry the repo requires, and
every promised change has a hunk or a deferral note that the
description restates. pkg-13 (neovim) is the model honest shortfall:
`%VAR%` expansion is deferred in the plan and restated in the
description, and it is ready as scoped. Drift runs in two directions:

- More than the plan: an unplanned rewrite, rename pass, config
  option, or flag rides along (pkg-03 adds a `resolve_symlinks`
  option the plan scopes out; pkg-06 adds a renaming pass and a new
  setting; pkg-09 adds a `--time-format` flag; calib-03's second hunk
  rewrites `print/property.js`, a file the plan never names).
- Less than the plan: a promised change is missing with no note
  (pkg-17 promises a warning AND a docs update; the diff has only the
  warning).

A description claiming more or less than the diff delivers is silent
drift, not a comms problem: pkg-17 says the docs "now document the
limitation" with no docs hunk; calib-03 says "no changes beyond the
plan" over an unplanned hunk. Always read the diff before believing
the description.

## Test evidence (harness category: not-tested)

Where it lives: `### Test evidence`, plus any before/after blocks in
`### Description`. Read it against the plan context's test plan (its
scenarios and expected-after) and its repro evidence (the steps,
inputs, and observed failure). The repo's test command is in the
`## Repo facts` `pull requests:` line or stated in the plan. Live: the
student's captured test output, read against `plan.md`'s test plan
and the repo's `make` targets / CI jobs named in
`.github/PULL_REQUEST_TEMPLATE.md`.

What good looks like: the reproduced failing scenario is re-run with
the same input, the before shows the original failure, the after
shows the plan's expected result, and a named suite command shows
its outcome (pkg-02: the repro script before/after plus `pytest tests/
-q` passes (1462 passed); pkg-19: byte-identical round-trip with
`diff` plus `go test ./...` passes). If the test plan names several
failing scenarios, each is shown or explicitly deferred.

Not-tested tells:

- Words in place of output: "tested locally, colors work now"
  (pkg-04), "verified working on my machine for a full day" (pkg-10).
- Evidence on the wrong path: only the control that never broke is
  run (pkg-07 runs the single-file case, not the two-file repro);
  a different request is exercised than the one that failed (pkg-14
  runs a GET with header casing, not the single-header POST).
- A named scenario silently skipped (calib-04 re-runs repro 1 but
  never the `--exec` abort the test plan named).
- A suite "passes" with no command named, or the repo's stated suite
  never shown (pkg-10: `yarn test` never shown).

## Diff quality (harness category: unreviewable)

Where it lives: `### Diff` (every added `+` and removed `-` line) and
`### Commits`. Live: `git diff main...HEAD` and `git log
main..HEAD --oneline`.

What good looks like: every hunk serves the change, so a reviewer sees
the fix and nothing else (pkg-13: one hunk in `ui.lua` plus one test;
calib-01: a one-line deletion). Debris tells, any one of which fails:

- debug output left in: `eprintln!("DEBUG ...")`, `print(`,
  `console.log`, including commented-out ones;
- commented-out code or a commented-out first attempt;
- dead code: an unused function, often behind `#[allow(dead_code)]`
  or named `_unused_...` (pkg-12's `_unused_buffer_probe`, pkg-15's
  `count_open_fds`);
- a stray `TODO` on lines the fix does not need;
- churn: re-indents, import reordering or restructuring, or lines
  removed and re-added identically (pkg-18's re-printed lines).

Commit messages like "wip", "fix", "fmt + cleanup", or "misc cleanups
while debugging" (pkg-12, pkg-15) point to debris; confirm it in the
diff. Unplanned behavior is plan fidelity, not debris.

## Standards and comms (harness category: standards-wall)

Where it lives:

- The repo's asks: the `## Repo facts` block. The `pull requests:`
  line lists template sections, checklist items, issue-link format
  (`closes #`, `fixes #`), and required entries (CHANGELOG.md,
  whatsnew). The `contribution policy` line states any AI rule. Live:
  `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue `Closes #`,
  Changes, Testing checklist including CI green and removing the
  seeded bug's `xfail`, Notes for Reviewers) and `docs/CONTRIBUTING.md`.
- Where the PR answers them: `### Description` for sections,
  checklists, links, and disclosure; `### Diff` for required files (a
  changelog or whatsnew entry must be a diff hunk, not just a claim).
- Maintainer direction: `## Thread highlights` lines from OWNER,
  MEMBER, or COLLABORATOR. "(0 comments total)" means none.

What good looks like: every stated section is present with real
content, every required item is met or marked not applicable with a
reason (pkg-08: "cheatsheets regenerated (no binding changes, no
diff)"), and where the policy asks for AI disclosure the description
gives one in the author's words (pkg-11: "this change was AI-assisted
with me reviewing and understanding every line, and this text is in
my own words"). Treat every package as AI-assisted work.

Standards-wall tells: a required checklist ignored (pkg-01: no
`closes #`, no checklist, and no whatsnew hunk where pandas requires
one); a required AI disclosure missing (pkg-20: ghostty requires
disclosing all AI use, and the description has none). A repo with
"no stated AI policy" asks for no disclosure. Whether the
description's claims match the diff belongs to plan fidelity, above.
