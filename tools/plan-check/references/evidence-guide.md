# Evidence guide: where evidence lives in a plan package

Every eval package has the same top-level blocks, in this order:
`## Repo facts`, `## Issue`, `## Thread highlights (N comments
total)`, `## Repro evidence`, `## Candidate plan`, `## Candidate plan
comment`. The plan's own parts are NOT labeled the same way in every
package: some use `###` subheadings (Summary, Background, Problem
statement, Diagnosis, Scope, Approach, Proposed changes, Changes,
Files, Files and areas, Test plan), some use inline labels (`Cause:`,
`Diagnosis:`, `Change:`, `In scope:` / `Not in scope:`, `Out:`,
`Files:`, `Approach:`, `Test:` / `Test plan:`, `Risk:`), and some use
plain prose. Find each part by what it says, not by its label, and
never grade a check down because a label is missing.

In live mode the same parts live in: the issue page (issue body and
thread), the student's posted repro comment on that issue (or the
house repro pack as the drafts quote it), the repo's README,
CONTRIBUTING.md, and issue/PR templates, and the student's draft
`plan.md` and draft plan comment.

## Diagnosis and grounding

Where it lives:

- The cause: the plan's `Cause:` / `Diagnosis:` / `### Diagnosis` /
  `Root cause per the report:` text, or the plan's first sentences
  explaining why the bug happens. The plan comment often restates it
  in one line; if the two disagree, grade the plan and note the
  mismatch under Comms.
- What the cause must explain: the `## Repro evidence` block. Read its
  `Environment:`, `Steps:` (or `Steps and output:`, `Steps and
  timings`), `Expected:`, `Actual:`, and especially any `Control:` /
  `Control runs:` and `Artifact:` lines. Controls are the strongest
  evidence in the package: they show what changes the behavior and
  what does not.
- Live: the cause is in `plan.md`; the behavior is in the student's
  posted repro comment.

What good looks like: every step, timing, artifact, and control in the
repro evidence is something the stated cause predicts, and the cause
names a mechanism, not just a restatement of the symptom. A diagnosis
fails when a control rules it out. Example: in calib-03 the plan blames
pager key bindings, but repro step 3 shows the same 26 s delay with
`--paging=never` (no pager at all) and step 2 shows the same pager
seeking instantly with color off, so the evidence points at
highlighting, not bindings. In calib-01 the cause ("the view's model
is not refreshed after push") explains steps 1 to 4, including why
re-entering the view fixes the color.

## Scope

Where it lives: the plan's `In scope:` / `Not in scope:` / `Out:`
lines, a `### Scope` section, the files listed under `### Files`,
`Files:`, `### Files and areas`, or named inline in `Change:` /
`### Changes` / `### Proposed changes` / `### Approach`. Measure them
against what the `## Issue` asks for and anything the thread
highlights narrow or widen.

What good looks like: one change aimed at the reproduced behavior,
where every file or area listed is needed for that fix and the
not-in-scope line excludes neighbors a builder might drift into (calib-01:
one callback in `sync_controller.go`, with push-status computation and
other views explicitly out). A deferral with a reason ("deferred: the
Windows variant, I cannot test it") is good scope. Bad scope bundles
work the issue did not ask for: rewrites ("rewrite the tokenizer"),
refactors, renames, docs or dependency changes, or extra features. A
plan whose scope is "everything in module X" is unbounded even if it
is short.

## Executability

Where it lives: the plan's change description (`Change:`, `###
Changes`, `### Proposed changes`, `### Approach`, numbered change
steps) and file list. Check the files and functions it names against
the `## Repo facts` block and the repro evidence (eval mode never
fetches the repo, so a path is accepted if nothing in the package
contradicts it); live, check them against the repo itself.

What good looks like: a stranger could open an editor and start: the
plan names where the change goes (a file, function, callback, or code
path) and what changes there (calib-01: "the push completion callback
in `pkg/gui/controllers/sync_controller.go` adds the commits context
to its post-push refresh scope"). Bad: "fix the refresh logic",
"handle this case properly", or a plan that names a file but not what
to do in it, or what to do but not where.

## Test plan

Where it lives: the plan's `Test:` / `Test plan:` / `### Test plan`
text, plus any test file or test case named under the changes. Read
it against the repro evidence's `Steps:`, inputs, `Expected:`, and
`Control:` lines.

What good looks like: at least one check reruns the reproduced steps
or inputs and names the result that must be seen after the fix, which
differs from the `Actual:` in the repro evidence (calib-01: "at step 3
the color must flip without leaving the view"). Strong test plans also
rerun the control to show nothing else changed, or cover the sibling
paths the change touches (calib-01: main commits panel and force
push). Bad: "run the test suite", "verify it works", "add tests", or a
test of something other than the reproduced behavior (calib-03's
binding smoke test checks the bindings, not the 26 s delay).

## Honesty

Where it lives: `Risk:` lines, any "unknown", "assumption", "I have
not confirmed", "open question" text in the plan or comment, and the
confidence of the diagnosis wording ("this is caused by" vs. "likely"
vs. "I suspect"). Read each confident claim against the repro
evidence and against who said it in the thread highlights (the
author association in parentheses: OWNER, MEMBER, COLLABORATOR,
CONTRIBUTOR, NONE). Live: a mid-build deviation is recorded in
`plan.md` itself, not only in the diff.

What good looks like: claims the repro evidence shows are stated
plainly; claims it does not show are marked as assumptions with how
they will be checked; real risks are named with a mitigation (pkg-14:
draining input at attach could eat a keystroke, so the drain is
bounded to OSC patterns and flagged for review). Bad: presenting an
unverified claim as established. calib-03 calls a NONE commenter's
untested theory "identified" and builds on it, while a COLLABORATOR
called the problem known with no current solution and the repro
evidence contradicts the theory.

## Comms

Where it lives: the `## Candidate plan comment`, read against:

- `## Thread highlights`: each maintainer line (OWNER, MEMBER,
  COLLABORATOR) is an ask, decision, or warning the comment must
  respect; also prior attempts and linked PRs. "(0 comments total)"
  means there is nothing to address.
- `## Repo facts`: the `bug reports:` template line and the
  `contribution policy` line, including any AI disclosure requirement
  and any limits on outside PRs.
- The `## Candidate plan`: the comment must describe the same cause
  and change.
- Live: the issue thread, CONTRIBUTING.md, and templates on GitHub;
  `scope.md`'s house rules (no piggybacked plans).

What good looks like: the comment shows it read the room. It answers
or follows every maintainer direction in the thread, meets the stated
contribution policy (calib-01 keeps the change minimal and cites the
review-bandwidth note in CONTRIBUTING), discloses AI use where the
policy asks for it, and gives the maintainer something to decide.
Bad: boilerplate that would fit any issue, ignoring a maintainer's
stated direction or an open PR that already covers the fix, skipping
a required disclosure, contradicting the plan, or "same approach as
above".
