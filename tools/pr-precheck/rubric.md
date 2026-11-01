# Rubric: is this pull request ready to submit?

Four failure families get bad PRs submitted, and every one has
required checks here: silent drift (`diff-matches-plan`,
`description-matches-diff`), not tested (`evidence-reruns-repro`,
`repo-checks-run`), unreviewable (`diff-clean`), and standards wall
(`template-asks-met`, `ai-disclosure-met`). A PR that honestly
discloses a shortfall can still be ready: delivering less than the
whole issue is not a fail when the plan or a deviation note scoped
it out and the description says so.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | Every changed file and every hunk in the candidate PR's diff, read against the plan context's scope, its named files, its not-in-scope / deferral notes, and any deviation note (in the plan, or restated in the description). | Pass if every hunk does work the plan (plus its deviation notes) asks for, AND every change the plan promises (code, test, docs) appears in the diff or is explicitly deferred with a note. Fail if any hunk adds behavior, a config option, a flag, a rename, or a rewrite the plan does not ask for or scopes out, or if a promised change is missing with no deviation note. Tests for the planned fix, comments explaining it, and entries the repo requires (changelog, i18n strings) count as inside the plan. Debris (debug prints, dead code, churn) is graded by `diff-clean`, not here. | required |
| description-matches-diff | Each factual claim in the PR title and description about what changed, read against the diff. | Pass if every claim about what the PR changes, adds, or leaves alone is true of the diff, and any shortfall versus the plan is stated. Fail if the description claims something the diff does not contain (e.g. "docs now document X" with no docs hunk) or claims fidelity the diff contradicts ("exactly as planned", "no changes beyond the plan", "no functional changes outside X" over a diff that has them). | required |
| evidence-reruns-repro | The candidate PR's test-evidence section (and any before/after in the description), read against the plan's test plan and the repro evidence's steps, inputs, and observed failure. | Pass if the evidence re-runs the reproduced failing scenario (the same steps or inputs that failed in the repro, on the code path the fix changes) and shows an observable after-result that differs from the before and matches the plan's expected-after. If the plan's test plan names more than one failing scenario, each one is re-run or explicitly deferred. Fail on "tested locally", "works now", "verified on my machine", evidence that only exercises a control or an unchanged path, or a named failing scenario silently skipped. | required |
| repo-checks-run | The test-evidence section, read against the repo's stated test command (repo-facts block, contributing asks) and the plan's test plan. | Pass if the evidence names the suite or check that was run (a command or a named test target) and states its outcome. Fail if no suite or check run is shown, or one is claimed with nothing named ("tests pass"). A suite pass never substitutes for `evidence-reruns-repro`. | required |
| diff-clean | The diff's hunks and the commit list. | Pass if every hunk serves the change. Fail if the diff carries any debris: a debug print or log left in, commented-out code, a dead or `allow(dead_code)` function, a stray TODO, or formatting / re-indent / import-reorder / re-printed-identical-lines churn on lines the fix does not need. Commit messages like "wip" or "cleanup while debugging" support a fail but never cause one alone. | required |
| template-asks-met | The repo-facts block's `pull requests:` line (template sections, checklist, required items such as a `closes #` / `fixes #` link, a changelog or whatsnew entry, tests) read against the PR description and the diff. | Pass if every section or item the repo states as asked for or required is present with real content; a required changelog / whatsnew entry must appear in the diff; a checklist item marked not applicable with a reason counts as met. Fail if a required template section, checklist, issue link, or stated required entry is visibly missing. Where the repo has no template and asks only for tests, pass if the diff adds or updates a test. | required |
| ai-disclosure-met | The repo-facts block's `contribution policy` line, read against the PR description. Treat every package as AI-assisted work. | Pass if the policy states no AI rule, or the description contains a disclosure meeting what the policy asks (e.g. the tool used and the extent, or a statement that the text is in the author's own words). Fail if the policy requires AI disclosure and the description has none, or the disclosure omits what the policy names. | required |
| thread-direction-engaged | The thread highlights' maintainer lines (OWNER, MEMBER, COLLABORATOR), read against the description and the diff. | Pass if every explicit maintainer direction or question about the fix is followed or answered. Fail if one is contradicted or ignored. "(0 comments total)" passes. | preferred |

## Verdict rule

Accept (submit) only if every `required` check grades `pass`. Any
`required` check graded `fail` means reject (hold).

`unclear` on a `required` check counts as `fail`, so the package is
held: a PR a grader cannot verify from the package is a PR a reviewer
cannot verify either. Grade `unclear` only when the evidence is
present but supports both readings; when the evidence a check needs
is absent, grade `fail`.

`preferred` checks never change the verdict, whatever their grade;
report them in the summary as advice only.
