# Procedure: how this tool grades a PR package

## Read order

1. Decide the mode. A single bundle file with `## Repo facts`,
   `## Issue`, `## Thread highlights`, `## Plan context`, and
   `## Candidate PR` blocks is eval mode. A `plan.md`, a branch in a
   working copy, a draft PR title and description, test evidence, and
   an issue URL is live mode.
2. Live mode only: read `scope.md`. If the `Repo:` line still holds a
   placeholder, stop without grading and tell the student to fill it
   with their section's Path Review repo. If the issue or PR target is
   not in that repo, stop and refuse to grade. Otherwise note the house
   rules (branch `fix/<issue-number>-<slug>` on the student's fork, one
   PR per issue, the PR template always used). Eval mode: skip
   `scope.md` and `voice-guide.md` entirely.
3. Read `rubric.md`. Write down every check's name and weight, and the
   verdict rule, including what `unclear` does. If the table has no
   filled rows, stop and say the rubric is empty.
4. Read `references/evidence-guide.md` so you know where each evidence
   family lives before you open the package.
5. Read the issue (eval: `## Issue`; live: the issue page). Note the
   reported behavior and what the issue asks for.
6. Read the plan next, before any part of the PR (eval:
   `## Plan context`; live: `plan.md` with its deviation notes). Write
   down four lists: (a) the files and areas the plan names; (b) every
   change the plan promises, including tests and docs; (c) everything
   the plan scopes out, defers, or records as a deviation; (d) the test
   plan's scenarios, each with its steps/inputs and expected-after,
   plus the repro's observed failure. Reading the plan first means the
   diff is judged against what was promised, not against what the
   description says was promised.
7. Read the repo's stated asks (eval: the `pull requests:` and
   `contribution policy` lines of `## Repo facts`; live: the repo's PR
   template and `docs/CONTRIBUTING.md`). List every required template
   section, checklist item, issue-link format, required entry
   (changelog, whatsnew), stated test command, and AI-disclosure rule.
8. Read the thread (eval: `## Thread highlights`; live: the issue
   thread). List every maintainer (OWNER, MEMBER, COLLABORATOR)
   direction or question about the fix.
9. Read the diff (eval: `### Diff` and `### Commits`; live:
   `git diff main...HEAD` run in the working copy, and `git log
   main..HEAD --oneline`). Read it before the description, so the
   description's claims are checked against what you saw, not the
   other way round.
10. Read the description and title last (eval: `### Title`,
    `### Description`, `### Test evidence`; live: the draft PR title,
    description, and captured test output). Do not grade anything until
    steps 5 to 10 are done.

## Evidence gathering

Record every piece of evidence as a short quote or fact with where it
came from (block name in eval mode; file, command, or URL in live
mode). Eval mode uses only bundle text and never fetches anything.

1. diff-matches-plan: list every changed file and hunk from the diff.
   Next to each, write the plan item from list 6(a)/(b) it implements,
   or "not in plan" (and whether list 6(c) scopes it out). Then walk
   list 6(b) and next to each promised change write the hunk that
   delivers it, or "missing" and whether a deviation note covers it.
   Tests for the fix, comments explaining it, and repo-required
   entries map to the plan item they support.
2. description-matches-diff: quote each claim in the title and
   description about what changed, what was added, or what was left
   alone (including fidelity claims like "exactly as planned"). Next
   to each, write the hunk that makes it true, or "not in diff" /
   "contradicted by <hunk>". Note whether any "missing" item from step
   1 is stated in the description.
3. evidence-reruns-repro: for each test-plan scenario in list 6(d),
   quote the part of the test evidence that re-runs it, and record (i)
   whether it uses the repro's failing steps or inputs (not a control
   and not an unchanged path), (ii) the before and after results
   shown, and (iii) whether the after matches the plan's expected-after.
   Write "not re-run" for any scenario with no matching evidence, and
   whether a deferral note covers it.
4. repo-checks-run: quote the suite or check run in the evidence
   (command or named target) and its stated outcome, next to the
   repo's stated test command from step 7. Write "none named" if the
   evidence only says tests pass.
5. diff-clean: scan every added line and every hunk for debris: debug
   prints or logs, commented-out code, dead or `allow(dead_code)`
   functions, stray TODOs, and formatting / re-indent / import-reorder
   / identical-line churn that the fix does not need. Quote each one
   found with its file. Note the commit messages.
6. template-asks-met: for each required item from step 7, quote where
   the description or diff meets it (a changelog entry must be a diff
   hunk), or write "missing". An item marked not applicable with a
   reason counts as met.
7. ai-disclosure-met: quote the policy's AI rule from step 7 (or "no
   stated AI policy"). Quote the description's disclosure, or write
   "none".
8. thread-direction-engaged: for each maintainer direction from step
   8, quote what in the description or diff follows or answers it, or
   write "not addressed".

## Check execution

1. Grade the checks in the rubric's table order, top to bottom.
2. Apply each check's pass condition to the evidence recorded for it
   in Evidence gathering. Re-read the package only when that evidence
   is missing something the pass condition asks about, and then
   re-read only the part the evidence guide names for that check.
3. Grade `pass` if the evidence meets every part of the pass condition.
4. Grade `fail` if the evidence breaks any part of the pass condition,
   or if the evidence the check needs is absent from the package (no
   test evidence, no plan context, no diff hunk for a claimed change).
5. Grade `unclear` only when the evidence is present but supports both
   readings, and say in one line what makes it ambiguous. Never use
   `unclear` for a step you skipped.
6. Grade each check on its own. The same hunk can fail one check and
   pass another (an unplanned rename fails `diff-matches-plan`; a
   debug print fails `diff-clean`); do not let one grade change
   another.
7. Grade the substance, not the polish: a terse description that is
   true of the diff passes; a confident, well-formatted one that the
   diff contradicts fails.
8. Write one evidence line per check: the quote or fact that decided
   the grade.
9. If this procedure does not say how to handle something, grade the
   check by the rubric alone and note the gap in the summary. Do not
   invent a step.

## Verdict assembly

1. Collect the grades of the `required` checks.
2. If any `required` check is `fail`, the verdict is `reject`.
3. If any `required` check is `unclear`, count it as `fail`; the
   verdict is `reject`.
4. If every `required` check is `pass`, the verdict is `accept`.
5. Ignore `preferred` checks for the verdict whatever their grade;
   list them in the summary as advice.
6. The deciding check for a `reject` is the first `required` check in
   rubric table order that graded `fail` or `unclear`. Name it in the
   summary and quote its evidence line.
7. Live mode only: hold the draft PR title and description against
   each rule in `voice-guide.md` and list any broken rule in the
   summary, quoting the rule. This never changes the verdict.
8. Write the summary: one line per check with its grade, then the
   deciding check (for a reject), then any voice notes (live).
9. End with the JSON block from SKILL.md, using the rubric's check
   names exactly, every check in table order, and nothing after it.
