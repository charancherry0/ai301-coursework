# Teaching my issue scout to skip taken work, and the AI issue it found

## Overview

issue-scout is a Claude Code skill I built this term to find a first open-source issue for me. It runs GitHub searches, ranks the results for free, and then pays about $0.20 per issue to grade the top few against a rubric. I wrote the two parts that decide what it does: `search-strategy.md` (the queries and ranking filters) and `rubric.md` (the checks an issue must pass). Its first real scan on 2026-11-11 kept surfacing issues that were already taken and misread how real repositories work. I added a search qualifier that drops issues with a linked pull request and fixed four rubric checks. The improved search found [suitenumerique/conversations#911](https://github.com/suitenumerique/conversations/issues/911), "Add user-agent to MCP and LLM calls", in the French government's open-source AI chat app. My instructor approved it, and on 2026-11-12 I posted a [claim comment](https://github.com/suitenumerique/conversations/issues/911#issuecomment-6812019713) asking the maintainer to confirm the header format. As of today there is no reply, so I have not written code or opened a pull request.

## What happened

### The first scan

My strategy ran two `gh search issues` queries for Python and TypeScript issues mentioning `llm` and labelled `good first issue`. Filters cut 200 results to 123, and the scout graded the top five: 2 accepted, 3 rejected. The rejects were correct, but the scout's own report showed that my strategy had let it down:

```
1. Filter 3 only checks titles and labels. It let a repo named `react-native-…` through to the #1 spot.
2. Sorting by fewest comments favours brand-new issues. All five survivors had 0 comments and most were filed today.
   As a result, the reply-time check sampled only same-day issues and failed every repo.
```

This is one of the checks it got wrong:

```json
{"name": "maintainer-responsive", "grade": "fail", "evidence": "Sampled issues #12-#17 (all opened 2026-11-11) have no OWNER/MEMBER/COLLABORATOR reply yet"}
```

React Native was on my avoid list, and an issue opened that morning can't show whether maintainers reply within 30 days. Both problems came from my own instructions, not from the model.

### Most good first issues were already taken

Next I checked picks from the course's Issue Selection site, first with free `gh` commands and then with the scout. Nine of the eleven I checked by hand already had an open pull request (ArduPilot, taskchampion, Zulip, vllm-ascend). The scout only noticed this at grading time, after I had paid for the grade. It did make one good catch: it rejected taskchampion#427 as claimed because PR #777 was open, which the site's Claim button didn't show.

### What I changed

The biggest fix was moving the "already taken" check from the paid grading step into the free search. Both queries now end with GitHub's `-linked:pr` qualifier:

```
gh search issues llm --label "good first issue" --language python --state open --no-assignee --limit 100 \
  --json title,repository,labels,assignees,commentsCount,updatedAt,url -- -linked:pr
```

I tested both queries afterwards, and each still returns 100 results. I also made the avoid filter check repository names and allowed only one result per repository, because one repo had taken two of my five graded slots.

Then I fixed the rubric wherever a real repo had fooled it. `maintainer-responsive` before, as I wrote it in Unit 1:

> The "maintainer first-response sample" in the repo-facts block.

After:

> …sample only issues opened 30 or more days before today, open or closed; issues younger than 30 days cannot show a 30-day response yet. … If the repo has no issue old enough to sample, grade `unclear`, not `fail`.

`unclaimed` passed cherry-studio#24233 even though its thread said an open PR rewrote the same two functions. That PR just wasn't linked. I added:

> An open pull request that is not linked but that the thread names as touching the same code ("overlaps", "would conflict with", "blocked on #N") counts as an open linked PR and fails this check.

I made two smaller fixes too. A `help wanted` label that the opener's issue template added no longer counts as a maintainer endorsement (cherry-studio#19396, where it appeared two seconds after the issue was opened). And anyone who merges pull requests now counts as a maintainer, because GitHub labels private org members as CONTRIBUTOR.

### What the new search found

With `-linked:pr` in place, a search surfaced conversations#911. It fit my profile closely: it's Python, it touches LLM provider and MCP client code, and the project's CONTRIBUTING.md allows AI-assisted work. The scout still graded it **reject**. Every check passed except settled-approach, because the collaborator who opened it wrote: "Needs more spec'ing with firsthand LLM (Albert) and MCP (datagouv) teams."

I agreed with that verdict, and it's the behaviour I want: a `good first issue` label shouldn't override an open design question. But the blocker is one I could remove by asking, so I took the issue with my instructor's approval. Before writing the claim, I read the code so I could ask a specific question. The Mistral provider in `chat/agents/base.py` sends pydantic-ai's `get_user_agent()`, which names the library (`pydantic-ai/<version>`) rather than the app. The Albert and OpenAI-compatible providers and the data.gouv `MCPToolset` set no User-Agent explicitly. My claim comment proposed:

> a single helper returning something like `conversations/<version>` (optionally with a contact URL), applied to all three LLM providers and the MCP client, plus a test that checks the header is sent.

The result is that a better search led me to an approved, claimed issue in a real generative-AI codebase. I decided not to write code until the format is agreed, because that is exactly the risk the scout flagged.

## Limitations and what's next

- **The fixes are untested.** I didn't rerun the scan after changing the rubric, to save credit, so I haven't confirmed that the four fixes behave as intended. Next, I'll run a cheap canary scan on one or two candidates that the old rubric got wrong.
- **The rubric ignores triage bots.** On Cherry Studio, the project's labelling bot shows up as an outsider, so its triage is ignored. I'd add a rule for known triage bots.
- **The search can't filter by popularity.** The `llm` keyword pulls in tiny repos that are only days old, and `gh search` can't filter by stars. I'd add a star and age check in the ranking step using `gh api`.
- **Some invitations look open-ended.** The rubric can't tell a standing "add one of these" invitation from open-ended work (DrumBeatRepo#270). I've written a patch but haven't applied it.
- **The contribution is waiting.** The maintainer hasn't replied to my claim yet. Next, I'll follow up on the thread. If the spec stays open, I'll offer a draft pull request with a configurable header so the teams can decide on the exact string during review.
