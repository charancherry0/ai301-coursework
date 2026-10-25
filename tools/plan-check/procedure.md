# Procedure: how this skill grades a plan package

## Read order

1. Decide the mode. If the input is a single bundle file containing an
   issue-context block, a repro-evidence block, a candidate plan, and a
   candidate plan comment, it is eval mode. If the input is a `plan.md`,
   a draft plan comment, and an issue URL, it is live mode.
2. Live mode only: read `scope.md`. If the `Repo:` line still holds a
   placeholder, stop without grading and tell the student to get their
   cohort's scope file. If the issue URL is not in that repo, stop and
   refuse to grade. Otherwise note the house rules (no piggybacked
   plans, branch named `fix/<issue-number>-<slug>` on the student's
   fork). Eval mode: skip `scope.md` and `voice-guide.md` entirely.
3. Read `rubric.md`. Write down the name and weight of every check, and
   the verdict rule, including what `unclear` does. If the table has no
   filled rows, stop and say the rubric is empty.
4. Read `references/evidence-guide.md` so you know where each evidence
   family lives before you open the package.
5. Read the issue context. Note what the issue says is wrong and what
   it asks for; this is what scope is measured against.
6. Read the repro evidence next, before the plan. Note in your own
   words: the steps and inputs, the observed behavior, the expected
   behavior, and any error output or location it points at. Reading it
   first means you judge the plan against what was reproduced, not
   against what the plan says was reproduced.
7. Read the thread highlights (eval) or the issue thread (live) and the
   repo-facts block (eval) or the repo's CONTRIBUTING, templates, and
   AI-use policy (live). Note every maintainer ask, decision, or prior
   attempt, and every stated contribution requirement.
8. Read the candidate plan in full, then the candidate plan comment in
   full. Do not grade anything until all of steps 5 to 8 are done.

## Evidence gathering

For each check, record the evidence as a short quote or fact with
where it came from (block name in eval mode, URL or file in live
mode). Eval mode: use only text in the bundle and never fetch
anything. Live mode: take the repro evidence from the student's posted
repro comment on the issue (on the house issue, from the house repro
pack as the drafts quote it), and take everything else from the
locations the evidence guide names.

1. diagnosis-grounded: quote the plan's stated cause. Put it next to
   your notes from Read order step 6. List each reproduced behavior and
   mark whether the cause explains it, contradicts it, or ignores it.
   If the drafts quote no repro evidence, record "no repro evidence".
2. targets-cause: quote the plan's proposed change. Record where the
   change acts (file, function, or behavior) and where the diagnosed
   cause lives. Record whether the change removes the cause or only
   hides its effect.
3. scope-bounded: quote the scope statement and the not-in-scope line,
   if present, and list every file or area the plan names. Next to each
   one, record whether fixing the reproduced behavior needs it.
4. executable: list the files, functions, or areas the plan says it
   will change. Check each against the repo-facts block (eval) or the
   repo (live) and record whether it exists. Record what change the
   plan says it makes at each place.
5. test-plan-decisive: quote each test or check in the test plan. For
   each one, record whether it uses the reproduced steps or inputs,
   what result it expects, and whether that result would differ before
   and after the fix.
6. unknowns-honest: list each factual claim in the plan about the cause
   or behavior. Mark each one as shown by the repro evidence, marked by
   the plan as an assumption or unknown, or stated as fact with no
   support.
7. comment-thread-aware: list each maintainer ask from the thread and
   each contribution requirement from the repo facts. For each, quote
   the part of the plan comment that meets it, or record "not
   addressed". Record whether the comment's diagnosis and change match
   the plan's, and whether it only points at another plan.
8. comment-invites-decision: quote the comment's closing lines.

## Check execution

1. Grade the checks in the rubric's table order, top to bottom.
2. For each check, apply its pass condition to the evidence you
   recorded for it. Do not re-read the package unless that evidence is
   missing something the pass condition asks about; then re-read only
   the part the evidence guide names for that check.
3. Grade `pass` if the evidence meets every part of the pass condition.
4. Grade `fail` if the evidence breaks any part of the pass condition,
   or if the evidence the check needs is missing from the package (for
   example, no test plan, no repro evidence quoted, no scope given).
5. Grade `unclear` only when the evidence is present but supports both
   readings, and say in one line what makes it ambiguous. Do not use
   `unclear` because you skipped a step.
6. Write one line of evidence per check: the quote or fact that decided
   the grade.
7. Grade each check on its own. A fail in one check does not change
   the grade of another, even if the same sentence is involved.
8. If this procedure does not tell you how to handle something, grade
   the check by the rubric alone and note the gap in the summary. Do
   not invent a step.

## Verdict assembly

1. Collect the grades of the `required` checks.
2. If any `required` check is `fail`, the verdict is `reject`.
3. If any `required` check is `unclear`, count it as `fail`; the
   verdict is `reject`.
4. If every `required` check is `pass`, the verdict is `accept`.
5. Ignore `preferred` checks for the verdict whatever their grade; list
   them in the summary as advice.
6. Live mode only: check the plan comment against each rule in
   `voice-guide.md` and list any broken rule in the summary, quoting
   the rule. This never changes the verdict.
7. Write the summary: one line per check with its grade, and for a
   `reject`, name the first failing (or unclear) required check in
   table order and quote the evidence that decided it.
8. End with the JSON block from SKILL.md, using the check names from
   the rubric exactly, and nothing after it.
