# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/247

**Branch**

fix/69-json-array-fallback

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

One full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
`categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
This is the run saved in `eval-run.txt`.

**Package analysis**

pkg-13 (neovim/neovim#41337). My rubric decided accept and the gold label is accept. It is
the package most likely to be held by mistake, because the PR does not fix everything:
`%VAR%` expansion still breaks. My rubric reads it as ready because `diff-matches-plan`
passes when a promised change is "explicitly deferred with a note", and the plan says
"Known limit, stated in the plan: `%` ... is NOT covered by this change". The description
restates it ("percent-sign environment expansion (`%VAR%`) is NOT handled here"), so
`description-matches-diff` passes too, since the shortfall is stated. The test evidence
re-runs the issue's `&` URL with before (`code = 1`, truncated URL) and after (`code = 0`,
full URL) and names `make functionaltest`, so both test checks pass. The rubric's intro says
"delivering less than the whole issue is not a fail when the plan or a deviation note scoped
it out and the description says so".

**Check rationale**

| evidence-reruns-repro | The candidate PR's test-evidence section (and any before/after in the description), read against the plan's test plan and the repro evidence's steps, inputs, and observed failure. | Pass if the evidence re-runs the reproduced failing scenario (the same steps or inputs that failed in the repro, on the code path the fix changes) and shows an observable after-result that differs from the before and matches the plan's expected-after. If the plan's test plan names more than one failing scenario, each one is re-run or explicitly deferred. Fail on "tested locally", "works now", "verified on my machine", evidence that only exercises a control or an unchanged path, or a named failing scenario silently skipped. | required |

It started from my unit 3 `test-plan-decisive` check, which asked whether a plan's test
"fails on the current code, and passes after the fix". For a PR the question is no longer
whether a test is planned but whether it was actually run on the failing input, so I rewrote
it around the repro. I rejected a looser version ("shows a before and after") because a PR
can show a real before/after on the wrong path: running only the control that never broke,
or a different request than the one that failed, still looks like evidence. "On the code path
the fix changes" and "only exercises a control" are there for that. I also kept
"each one is re-run or explicitly deferred" so a plan with two failure modes cannot pass on one,
while an honest deferral still can. I kept the suite run in a separate check
(`repo-checks-run`) so "tests pass" can never stand in for the repro.

**Trade-offs**

A case I accept `ai-disclosure-met` will miss. It reads only the repo's stated policy:
"Pass if the policy states no AI rule". That is what keeps the 18 scored packages whose policy states no AI rule
from failing for lacking a disclosure nobody asked for, while still holding pkg-20,
where ghostty requires one. The cost showed up in my own live run: Path Review's
`docs/CONTRIBUTING.md` has no AI rule, so the check passed my draft while its AI-use line was
still an unfilled placeholder, even though the course always asks for the disclosure. The
tool noted the placeholder in its evidence line and the voice-guide notes caught it, but the
verdict did not. The disclosure had to be filled in separately before opening the PR, and I accept that the
rubric will not hold a PR for this in a repo with no AI policy.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
