# Rubric: is this reproduction package ready to post?

Six required checks, one for each way a bad package actually gets
posted, plus one preferred check. Every pass condition reads the thing
itself against the issue (does this artifact show that behavior? could
a stranger re-run this?), never how the write-up is laid out. The
section names in the Evidence column are the ones
`references/evidence-guide.md` maps.

Claim-only drafts (live mode): `env-recorded`, `steps-rerunnable`,
`behavior-matches`, and `honest-outcome` need the repro report, so they
are reported `unclear` with `not yet applicable: claim-only draft`.
`claim-specific` and `ai-disclosure` are graded on the claim comment
alone.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment line(s), read against the version, OS/platform, and any setting the issue or a maintainer in the thread says matters (driver, browser language, build type, shell). Evidence guide: Environment. | Passes when the report names the version of the software it ran AND the OS/platform it ran on, AND every difference from the issue's target that could change the behavior is either absent or stated in the report. Testing a NEWER release than the issue's and saying so passes; a different OS or install method that is stated passes. Fails when there is no environment record, when the version or platform is missing, or when the report ran an OLDER version than the issue targets (or a different build/channel than the one the issue or maintainers name) without saying so. | required |
| steps-rerunnable | The repro report's steps or command transcript, read from starting state to trigger, against the trigger conditions in the issue body and thread highlights. Evidence guide: Steps. | Passes when a stranger with only public resources could re-run the attempt: the exact commands are shown, and each input file, config, or snippet is either shown or described concretely enough to recreate (e.g. "an env.yml with a valid dependencies list plus a `category:` section"), or the report says it ran the issue's own script/steps verbatim; and every trigger condition the issue or a maintainer names is present in what was run (a required flag, driver, language setting). For a cannot-reproduce report, the steps are the attempt: they pass when the attempt itself is re-runnable, even though it did not produce the trigger. Fails when there are no steps, when a step depends on private or unshared code/config that a stranger cannot obtain or recreate, or when a reproduce claim's steps leave out a trigger condition the issue says is required. | required |
| behavior-matches | The artifacts in the repro report (output excerpts, error text, exit codes, logs, observed state) read against the behavior the issue describes, and the input actually run read against the issue's trigger. Evidence guide: Behavior shown. | Passes when a shown artifact displays the issue's behavior: the same error/panic/message class, exit status, or wrong output, produced by the issue's trigger (not a modified input or a different syntax). For a report that states it could NOT reproduce, passes when an artifact shows the attempt at the issue's scenario was really run and what it produced instead; whether the attempt reached the exact trigger is judged by honest-outcome (does the report say what differed?), not here. Fails when there is no artifact, when the artifact only shows setup (a version banner, the app running), or when it shows a different behavior from the issue's (a graceful validation error vs a crash, a compile error vs the reported runtime error, garbled output with the process still alive vs a crash), whatever the prose says about it. | required |
| honest-outcome | The report's stated result and conclusions (and the claim comment's claims about the reproduction), read against what the report's artifacts actually show. Evidence guide: Honesty. | Judge the report's outcome claims: whether it reproduced, what it says caused the bug, and where it says the bug applies. Passes when each of those is backed by a shown artifact (a secondary supporting remark described in prose, such as a control run, does not fail the check when the main outcome is shown): "reproduced/confirmed" only where an artifact shows the issue's behavior, no root cause stated as verified without shown evidence for it, no generalization to versions/channels/platforms that were not tested, and a cannot-reproduce stated plainly with what was tried and what differed from the issue's conditions. An honest, evidenced cannot-reproduce passes. Fails when the report or claim asserts more than its artifacts show; certainty words ("100%", "guaranteed", "verified the cause", "ran it ten times") are not evidence. | required |
| claim-specific | The candidate claim comment, read against the issue it is posted on. Evidence guide: Comms. | Passes when the comment could only have been written for this issue (it names this issue's symptom, scenario, code location, or a thread pointer) AND states a concrete next step the claimant can actually take (investigate X, test Y, report back). Fails when it is interchangeable boilerplate or a +1 with no intent, when it promises an outcome or a deadline ("fix within 2 days guaranteed"), or when it asks to be assigned or reserved instead of describing work. | required |
| ai-disclosure | The contribution-policy line in the repo-facts block (live: the repo's CONTRIBUTING / AI_POLICY files), read against the claim comment and repro report. Course packages count as AI-assisted work. Evidence guide: Comms. | If the policy requires disclosing AI use in issues or comments (or "all AI usage in any form"), passes only when the claim comment or repro report discloses the AI assistance and its extent. If the policy has no disclosure requirement, asks for disclosure only in pull requests, only asks contributors to understand and take responsibility for the work, or asks that comments be in the contributor's own words (a voice rule, not a disclosure rule), passes when the comments read as the contributor's own specific first-person writing. | required |
| template-asks-covered | The bug-reports line in the repo-facts block (the repo's issue template asks), read against what the repro report supplies. | Passes when the report supplies the items the repo's bug template asks for (version, OS, steps, expected, actual, or the named command output) or says why one does not apply. | preferred |

## Verdict rule

`accept` only if every required check passes. Any required check
graded `fail` or `unclear` makes the verdict `reject`: proof that
cannot be verified is not ready to post. The preferred check
(`template-asks-covered`) is reported but never changes the verdict.
For a live claim-only draft, the four checks marked not yet applicable
are left out, and the verdict is `accept` only if `claim-specific` and
`ai-disclosure` both pass.
