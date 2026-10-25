# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against the repro evidence (the observed vs. expected behavior, error output, and steps in the repro-evidence block or the student's posted repro comment). | Pass if the stated cause accounts for every behavior the repro evidence shows and contradicts none of it. Fail if the cause would predict something different from what was reproduced, ignores a reproduced behavior, or no repro evidence is quoted at all. | required |
| targets-cause | The plan's proposed change, read against its own diagnosis and the location the repro evidence points at. | Pass if the change acts where the diagnosed cause lives. Fail if it only hides the symptom while the cause stays (catching or swallowing the error, special-casing the reproduced input, suppressing the output, changing the test instead of the behavior). | required |
| scope-bounded | The plan's scope statement, not-in-scope line, and the files/areas it names, read against what the issue asks for. | Pass if it is one change that fixes this issue, and every named file or area is needed to fix the reproduced behavior. Fail if it bundles unrelated work (refactors, renames, new features, dependency bumps, style sweeps) the issue or thread did not ask for. | required |
| executable | The plan's approach and order of work, read against the repo-facts block (or the live repo) for the files/functions it names. | Pass if someone who never saw the thread could make the first edit without asking the author anything: the plan says where the change goes (a file, function, or area that exists in the repo) and what changes there. Fail if they would have to ask either of those. | required |
| test-plan-decisive | The plan's test plan, read against the repro evidence's steps, inputs, and observed output. | Pass if it names at least one check that uses the reproduced steps or inputs, fails on the current code, and passes after the fix, with the expected result stated so anyone can observe it. Fail on "run the tests", "verify it works", or checks unrelated to the reproduced behavior. | required |
| unknowns-honest | The plan's claims and its risks/unknowns, read against what the repro evidence actually established. | Pass if every claim the repro evidence does not support is marked as an assumption or open question, ideally with how it will be resolved. Fail if the plan states as fact something the evidence does not show, or claims certainty the evidence contradicts. | required |
| comment-thread-aware | The plan comment, read against the thread highlights (maintainer asks, decisions, prior attempts) and the repo-facts block's templates, contributing asks, and contribution policy (including AI-use disclosure). | Pass if the comment follows or directly addresses every maintainer direction in the thread, meets every stated contribution requirement (AI-use disclosure included where the policy asks for it), and describes the same diagnosis and change as the plan. Fail if it ignores or contradicts any of these, or piggybacks another plan ("same as above"). | required |
| comment-invites-decision | The plan comment's closing, read against the thread. | Pass if it gives the maintainer a clear decision point (asks whether the approach is acceptable before the build starts, or names the one open question). | preferred |

## Verdict rule

Accept (ready) only if every `required` check grades pass. Any
`required` check that grades fail means reject (hold).

A `?` (unclear) on a `required` check counts as a fail, so the package
is held: if the package does not let a grader confirm the check, a
maintainer reading the posted comment can't confirm it either. Grade
`unclear` only when the evidence is in the package but ambiguous; when
the evidence the check needs is missing entirely, grade fail.

`preferred` checks never change the verdict, whether they pass, fail,
or come back unclear. Report them in the summary as advice only.
