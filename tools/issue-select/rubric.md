# Rubric: is this a good first issue?

All date thresholds below are measured against the bundle's stated capture
date in eval mode, and against today's date in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` flag on the `repo:` line of the repo-facts block (live: the archive banner across the top of the repo front page). | Passes when `archived: no`. An archived repository is read-only and cannot accept a pull request, so `archived: yes` fails. | required |
| maintainer-committing | The dates on the "last 5 default-branch commits" list in the repo-facts block (live: the commit list on the repo front page). | Passes when the newest of those commits is dated within 180 days of the capture date. Commits by a bot count only when the commit merges a human's pull request. | required |
| repo-still-shipping | The "last push to any branch" line, and the "latest release" line, in the repo-facts block (live: the front-page commit date and the Releases box). | Passes when the last push to any branch is within 365 days of the capture date. A repo with no published release still passes on the push date alone: many small and actively maintained projects never cut releases. | required |
| bounded-single-change | The issue title and body. | Passes when the body names one change with a completion condition someone could recognise: a specific defect or behaviour to correct, or named files, pages, or components to add or edit. Fails when the issue is framed as a tracking issue, umbrella, meta-issue, or "megaissue" whose body is a list of other issues; when the ask is an open-ended programme of work applied across the codebase with no single point at which it is done; or when the issue is a usage or support question rather than a request for a change. Several named files touched by one coherent change are still one change; a list of independent sub-tasks meant to become separate pull requests is not. | required |
| settled-approach | The comment thread (author_association on each comment) and the "linked PRs:" field with each PR's state. | Passes when no maintainer (OWNER, MEMBER, or COLLABORATOR) has left the approach open in the thread — no unanswered design question, no competing proposals without a maintainer choosing between them — and the issue has at most one closed-unmerged linked pull request. Two or more closed-unmerged attempts are evidence that the work is harder than the issue text admits. | required |
| maintainer-endorsed | The issue's `opened by` line with its author_association, the `labels:` list, and the comment thread. | Applies only when the issue asks for new behaviour or a new feature; bug reports and documentation tasks are exempt and pass automatically. For a feature ask, passes when a maintainer (OWNER, MEMBER, or COLLABORATOR) has endorsed it: opened it themselves, applied a maintainer-applied label such as `good first issue` or `help wanted`, or commented in support. A feature request nobody on the project has agreed to is a product decision that has not been made yet, not a first issue. | required |
| unclaimed | The "this issue: assignees:" and "linked PRs:" fields, plus the comment thread. | Passes when all three hold: assignees is `none`; no linked pull request is in the `open` state; and no claim comment ("I'll take this", "working on this", "can I pick this up") was posted within 180 days of the capture date. A claim older than 180 days with no follow-up is stale and does not block, and a closed-unmerged linked PR is an abandoned attempt rather than an active claim. | required |
| ai-contributions-allowed | The "contribution policy" line in the repo-facts block (live: `CONTRIBUTING.md` in the repo root or `.github/`, the contributor docs it links to, and any `AI_POLICY.md` or PR-template disclosure box). | Passes when the policy does not ban AI-assisted contributions. Conditions are not bans: disclosure, personal understanding, testing, and human-review requirements all pass, and are terms to follow. A policy stating no AI-generated code or documentation is accepted fails. Silence passes: a repo with no stated policy has not restricted anything. | required |
| maintainer-responsive | The "maintainer first-response sample" in the repo-facts block. | Passes when at least one sampled issue drew a first owner/member/collaborator reply within 30 days. Many healthy repos triage on the pull request rather than on the issue tracker, so this ranks accepted issues; it never rejects one. | preferred |
| shipping-releases | The "latest release" line in the repo-facts block. | Passes when a release was published within 365 days of the capture date. Evidence that the work reaches users, not a condition of health. | preferred |
| newcomer-signposted | The `labels:` list on the issue. | Passes when the issue carries `good first issue`, `help wanted`, `documentation`, or an equivalent maintainer-applied label. A maintainer's own statement that the issue is friendly, which is worth having but is not evidence the issue is free. | preferred |

## Verdict rule

The verdict is `accept` when every `required` check grades `pass`, and
`reject` when any `required` check grades `fail` or `unclear`.

A required check graded `unclear` counts as a fail. The evidence these
checks name is present in every bundle and on every live issue page, so
`unclear` means the signal genuinely could not be found — and a first
issue whose safety cannot be verified is not one to take.

`preferred` checks never change the verdict. Report their grades, and on
an accepted issue use them to rank it against the other accepted
candidates.
