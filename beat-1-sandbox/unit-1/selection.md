# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69

**Verdict output**

Live-mode run over three candidates (#69, #57, #60), pasted verbatim. The chosen issue,
#69, is ranked first among the accepted candidates and is the first object in the JSON
block, with `"verdict": "accept"`.

````
Evidence gathered live from the GitHub API. Repo facts common to all three: `archived: false`, last default-branch commit `2026-09-16` by `Aburke225` (COLLABORATOR, human), last push `2026-09-16`, **no published releases**, contribution policy at `docs/CONTRIBUTING.md` with no AI restriction, and a maintainer first-response sample (#72, #63, #62, #70, #68) showing only `NONE`-association student replies.

## Ranked read-out

**Accepted**

**1. #69 — Output parser crashes on a top-level JSON array fallback** — best fit. It sits directly on the LLM generation path (`rag/generator/output_parser.py` parses what the model returns), which is the generative-AI work you want; it's Python, confined to two named files, and its stated 2–4 hour estimate fits inside your five-hour budget. A classmate has already posted a full verified reproduction in the thread, which saves you setup time. Carries `good first issue`.

**2. #57 — Tech detector counts vendored and build-output files** — also clean and slightly smaller (add `node_modules/`/`build/` path exclusion, two files, zero comments so no contention at all), but the defect is path filtering inside an agent tool rather than anything LLM-facing, so it ranks below #69 on your AI-work preference. Carries `good first issue`.

**Rejected**

**#60 — Faithfulness checker crashes on `text: None`** — sank on `unclaimed`: PR #74 by `nianiiier`, opened 2026-09-21 and still `open`, says "Closes #60". The Path Review house rule waives *claim comments*, not an open linked pull request, so this one is genuinely taken.

A note on the tension, not a change to any grade: all three issues fail the preferred `maintainer-responsive` and `shipping-releases` checks, because this is a two-week-old classroom repo with no releases and staff who triage on the PR rather than the issue tracker. The rubric correctly treats those as ranking signals only, so they sank nothing — but they also gave no separation between #69 and #57, which is why the fit profile did all the ranking work here.

Per the house rule, comment to claim #69 anyway even though `jacho15` posted there on 2026-09-19 — credit attaches to the PR you open.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner."},
      {"name": "maintainer-committing", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR, human), 6 days before today."},
      {"name": "repo-still-shipping", "grade": "pass", "evidence": "Last push to any branch 2026-09-16, within 365 days; no release, which the rubric allows."},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "One defect with a recognisable completion condition: handle a top-level JSON array in rag/generator/output_parser.py and drop the xfail in tests/unit/test_output_parser.py."},
      {"name": "settled-approach", "grade": "pass", "evidence": "Both comments are from jacho15 (author_association NONE); no maintainer question left open and zero linked PRs, closed-unmerged or otherwise."},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Bug report (labels include 'bug'), exempt from the feature-endorsement requirement; also opened by Aburke225, a COLLABORATOR."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none, no linked PR in any state; jacho15's 2026-09-19 claim comment is a classmate's and is waived by the Path Review house rule."},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction of any kind; silence passes."},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "Sample of #72, #63, #62, #70, #68 shows every first reply came from a NONE-association student; no owner/member/collaborator reply."},
      {"name": "shipping-releases", "grade": "fail", "evidence": "Releases API returns an empty list; no release ever published."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue', applied by Aburke225 (COLLABORATOR) at issue creation."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/57",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner."},
      {"name": "maintainer-committing", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR, human), 6 days before today."},
      {"name": "repo-still-shipping", "grade": "pass", "evidence": "Last push to any branch 2026-09-16, within 365 days; no release, which the rubric allows."},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "One defect with a repro script and expected output: exclude node_modules/ and build/ paths in agent/tools/tech_detector.py, covered by two named tests."},
      {"name": "settled-approach", "grade": "pass", "evidence": "Zero comments, so no maintainer has left the approach open; no linked PRs at all."},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Bug report (labels include 'bug'), exempt from the feature-endorsement requirement; also opened by Aburke225, a COLLABORATOR."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none, no linked PR in any state, and an empty comment thread so no claim comment exists."},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction of any kind; silence passes."},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "Sample of #72, #63, #62, #70, #68 shows every first reply came from a NONE-association student; no owner/member/collaborator reply."},
      {"name": "shipping-releases", "grade": "fail", "evidence": "Releases API returns an empty list; no release ever published."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue', applied by Aburke225 (COLLABORATOR) at issue creation."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo API reports archived: false; no archive banner."},
      {"name": "maintainer-committing", "grade": "pass", "evidence": "Newest default-branch commit 2026-09-16 by Aburke225 (COLLABORATOR, human), 6 days before today."},
      {"name": "repo-still-shipping", "grade": "pass", "evidence": "Last push to any branch 2026-09-16, within 365 days; no release, which the rubric allows."},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "One defect with a repro: chunk.get('text', '') returns None in rag/evaluator/faithfulness_checker.py, covered by test_none_context_chunk_text."},
      {"name": "settled-approach", "grade": "pass", "evidence": "Zero comments so no maintainer question is open, and the single linked PR (#74) is open rather than closed-unmerged."},
      {"name": "maintainer-endorsed", "grade": "pass", "evidence": "Bug report (labels include 'bug'), exempt from the feature-endorsement requirement; also opened by Aburke225, a COLLABORATOR."},
      {"name": "unclaimed", "grade": "fail", "evidence": "PR #74 by nianiiier, opened 2026-09-21, is in the open state and its body reads 'Closes #60'; the house rule waives claim comments, not an open linked PR."},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md state no AI restriction of any kind; silence passes."},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "Sample of #72, #63, #62, #70, #68 shows every first reply came from a NONE-association student; no owner/member/collaborator reply."},
      {"name": "shipping-releases", "grade": "fail", "evidence": "Releases API returns an empty list; no release ever published."},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "Labels include 'good first issue', applied by Aburke225 (COLLABORATOR) at issue creation."}
    ],
    "verdict": "reject"
  }
]
```
````

---

## Eval iterations

**Run history**

Two runs, in order:

1. A partial canary run with `--only issue-01,issue-05,issue-09,issue-12,issue-20`,
   which scored `agreement: 5/5 scored items`. I picked those five before spending
   anything on a full run because each one attacks a different check I was unsure of:
   `issue-01` is an accept in a repo whose maintainer-response sample is almost all
   silence, `issue-05` is an umbrella wearing a `good first issue` label, `issue-09` is
   an accept carrying a four-year-old claim comment, `issue-12` is the only issue in the
   `policy` category, and `issue-20` is a tidily written feature request with no
   maintainer behind it. A partial run never counts as the submitted run, so this cost
   about $1 and told me whether the $4 full run was worth doing.

2. The full 20-issue run, which scored `agreement: 19/20 scored items  (bar: 18/20:
   PASS)` with `categories: claimed 4/4  clear-accept 7/8  dead-repo 3/3  policy 1/1
   scope 4/4`. That is the run saved in `eval-run.txt`, and I did not revise the rubric
   afterwards.

**Issue analysis**

`issue-19` (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the
UI"). My rubric decided `reject`; the gold label is `accept`. It is the one issue in the
run I disagreed on, and the `note` column names the check that did it:
`failed: bounded-single-change`.

My rubric read the issue body as a list of separate jobs rather than one. The body says
"There are two potential causes which should be fixed: 1. The matchers are slow for
certain rewrites (quadratic instead of linear) 2. UI update is waiting for the matching
thread to finish", and then adds three more items under "Additional suggestions",
including "We should use multi-processing to use all the cores to match rewrites in
parallel". My `bounded-single-change` check fails an issue that is "a list of
independent sub-tasks meant to become separate pull requests", and five numbered items
across two headed lists is exactly the shape that clause was written to catch — it is
the shape that sinks `issue-05` and `issue-10`, which my rubric got right.

The gold label reads the same text the other way, and I now think it is the better
reading: there is one observable defect here, the UI freezing on a large selection, and
the numbered lists are a maintainer's diagnosis of why it freezes plus optional
suggestions, not five pieces of work someone is expected to deliver. The distinction my
check cannot currently see is between a list of *causes of one symptom* and a list of
*separate deliverables*. A COLLABORATOR filed it with a `Type: bug` label and a
`Priority: High` label, and those are signals that the unit of work is the bug.

**Check rationale**

From `rubric.md` as uploaded to `tools/issue-select/`, the `repo-still-shipping` check,
quoted in full:

> | repo-still-shipping | The "last push to any branch" line, and the "latest release"
> line, in the repo-facts block (live: the front-page commit date and the Releases box).
> | Passes when the last push to any branch is within 365 days of the capture date. A
> repo with no published release still passes on the push date alone: many small and
> actively maintained projects never cut releases. | required |

The second sentence of the pass condition is the part I had to write deliberately. My
first instinct for "is the repo in use?" was release recency, because a project that has
shipped in the last year is obviously reaching users. But `issue-06`
(Itqan-community/quran-apps-directory) has `latest release: none published` and is a
gold `accept`, and `issue-07` (wting/autojump) also has `latest release: none
published` and is a gold `reject`. Release recency cannot separate those two, so as a
required check it would have rejected `issue-06` for a property that has nothing to do
with whether the repo is alive. Commits and pushes do separate them: `issue-06` was
pushed 2026-07-31 against a 2026-08-05 capture, and `issue-07` was pushed 2025-02-27.
So the required check reads the push date, and release recency survives as a separate
`preferred` check (`shipping-releases`) where it can rank accepted issues without ever
rejecting one.

**Trade-offs**

What `repo-still-shipping` gives up is any ability to notice a repo that is being
committed to but is no longer being released — a project where someone still merges
small pull requests but has not cut a version in years, so a merged change never reaches
a user. At a 365-day threshold on the push date, that repo passes my required check and
only loses the `shipping-releases` preferred point.

I accept that miss, because the eval set shows the opposite error is the expensive one:
making release recency required would have cost me `issue-06`, a genuine accept, and
would not have bought me `issue-02` or `issue-07`, which my two liveness checks already
reject on dates. The evidence that nothing else moved is the run itself — `dead-repo
3/3` in the categories line, with `issue-02`, `issue-07` and `issue-17` all rejected,
and the one disagreement in the whole run (`issue-19`) failing on
`bounded-single-change`, not on either liveness check.

---

## Selection rationale

**Selection rationale**

*1. The issue's fit to my interests and to the time available.*

I picked #69 because it is the one candidate that sits on the generative-AI path itself.
The bug is in `rag/generator/output_parser.py`, the code that reads what the model
returned: when the LLM answers with a top-level JSON array instead of an object, the
parser calls `.items()` on a list and raises `AttributeError: 'list' object has no
attribute 'items'`. Getting better at generative AI work is the thing I most want out of
this course, and handling the shapes a model can actually return is squarely that, in a
way that #57 — path filtering inside an agent tool — is not. It is also Python, which I
already write, it names its two files (`rag/generator/output_parser.py` and
`tests/unit/test_output_parser.py`), and the issue states an estimated effort of 2–4
hours, which leaves room inside the five hours I have for setup, reproduction and the
pull request.

*2. What the verdict identified correctly, and what I weighed that the rubric could not.*

The verdict got the claim picture right, and that was the decision that mattered across
the three candidates. It rejected #60 on `unclaimed` because PR #74 is open and says
"Closes #60" — a real claim — while passing #69, where the only claim signal is a
classmate's comment that the Path Review house rule waives. That is precisely the
distinction I wanted the check to make, and it made it without my help.

Two things I weighed that the rubric could not. First, `jacho15` posted a full verified
reproduction on #69 on 2026-09-19 — three separate reproductions, on Python 3.14.7,
down to line 68 of the parser. My rubric can only read that comment as a claim signal to
be waived; it cannot see that it is also free setup work that shortens my own Unit 2
reproduction. Second, the rubric ranked #69 over #57 only because my fit profile broke
the tie: both issues passed all eight required checks identically, and both failed the
same two preferred checks, so the preferred checks gave no separation at all here. In a
two-week-old classroom repo with no releases and staff who answer on pull requests rather
than in issue threads, `maintainer-responsive` and `shipping-releases` measure nothing.
The choice between the two accepted issues was mine, not the tool's.

*3. The anticipated difficulty in claiming it.*

The claim itself should be straightforward — no assignee, no linked PR, and the house
rule says a classmate's comment does not block me, so I claim anyway because credit
attaches to the pull request I open rather than to whether it merges. I am not
commenting yet: Unit 2 teaches the claim comment and the voice guide that goes with it,
and choosing is not claiming.

The real risk is the one I watched happen to #60. That issue looked clean when I started
and was taken by an open pull request filed on 2026-09-21, two days before I ran my
skill. `jacho15` has already reproduced #69 and may well be partway to a PR, so the
window on this issue is not open indefinitely. The other difficulty I expect is on the
work rather than the claim: the fix has to decide what a top-level array should actually
parse *into*, not just stop the crash, and removing the `@pytest.mark.xfail` marker
referencing manifest id H-02 means the covering test has to genuinely pass rather than
merely stop failing.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
