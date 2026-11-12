# Contribution: Add user-agent to MCP and LLM calls

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Reddy Charan Pulivendula
**GitHub Username:** charancherry0
**Issue:** https://github.com/suitenumerique/conversations/issues/911
**Pull Request:** pending

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Grew my issue-select skill into issue-scout, a scout that searches GitHub for me**

I installed issue-scout over my Unit 1 skill and wrote a search strategy with two `gh search issues` queries for Python and TypeScript `llm` issues labelled `good first issue`, ranked fewest comments first; my first scan graded five issues (2 accepted, 3 rejected) and I then checked 15 more picks by hand or with the scout. The scan showed problems in my strategy: the avoid filter only read titles, so a React Native repo ranked #1, and 9 of my 11 hand-checked picks already had an open PR, so I added `-linked:pr` and a one-issue-per-repo cap to rule those out before grading. Grading showed four ways my rubric misread real repos: maintainer-responsive sampled issues opened that morning; maintainer checks ignored a merger GitHub listed as CONTRIBUTOR; a `help wanted` label the reporter's template added at creation passed as maintainer endorsement (cherry-studio#19396); and `unclaimed` missed an unlinked open PR the thread said rewrites the same functions (cherry-studio#24233). I fixed each of those in `rubric.md` and logged the reason in `scan-notes.md`, though I didn't rescan to confirm the fixes, and I still haven't fixed two gaps: the rubric ignores project triage bots, and `gh search` can't filter out tiny, days-old repos.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I searched two ways. My issue-scout scan ran two `gh search issues` queries for Python and TypeScript `llm` issues labelled `good first issue`, ranked 123 surviving results by fewest comments, and graded the top five (2 accepted, 3 rejected). Then I went to the Issue Selection site and checked 15 more picks, first by hand with `gh` and then with the scout. None of them worked out: ten already had an open pull request or a recent claim (ArduPilot, taskchampion, five Zulip issues, three vllm-ascend tasks), and the other five were open-ended invitations or questions rather than changes, had competing fixes nobody had chosen between, or overlapped someone else's open pull request (DrumBeatRepo, four Cherry Studio issues). A search I added `-linked:pr` to found suitenumerique/conversations#911, "Add user-agent to MCP and LLM calls", and my instructor approved using it. My scout's verdict was **reject**: it passed every check except settled-approach, because the collaborator who opened it wrote "Needs more spec'ing with firsthand LLM (Albert) and MCP (datagouv) teams". I chose it anyway because that is the only blocker, and it is one I can remove by asking. In my claim comment I asked the maintainer to confirm the header format, and I will not start coding until they do. It fits my profile better than anything else I found: it is Python, it touches the LLM provider and MCP client code of a generative-AI chat app, the change is small enough for my five hours, and the repo's CONTRIBUTING.md allows AI-assisted work. I hope to learn how a real app wires its LLM providers and MCP tools through pydantic-ai, and how to agree a spec with maintainers before writing code. I already found that the Mistral provider sends pydantic-ai's default User-Agent while the Albert/OpenAI providers and the datagouv MCP client set none.

Claim comment: https://github.com/suitenumerique/conversations/issues/911#issuecomment-6812019713

Honestly, the scout's verdicts were mostly right, but the wild exposed gaps that the course eval set never did. It kept grading issues that were already taken, because popular good-first-issues get several pull requests within days, so I moved that check into the search with `-linked:pr`. It also misread real repos in four ways, each of which I patched in `rubric.md`: it sampled same-day issues for responsiveness, it missed a maintainer GitHub listed as CONTRIBUTOR, it treated a `help wanted` label added by an issue template as a maintainer endorsement, and it missed an unlinked open pull request that rewrote the same functions. The tool is still weak in two places I have not fixed: it ignores project triage bots, and `gh search` cannot filter out tiny, days-old repos, which flooded my `llm` results.

**Toolkit**

- Tool: issue-scout (my search strategy and my rubric). I used it for one full scan, then for direct grading of my Issue Selection site picks, with free `gh` checks first so I only paid to grade issues that were actually open.
- What it produced: on this issue, a reject on settled-approach ("Needs more spec'ing…") with every other required check passing, including unclaimed, maintainer-endorsed (the collaborator opened it and labelled it `good first issue`), and ai-contributions-allowed (CONTRIBUTING.md allows AI with conditions).
- Difficulties: 9 of my 11 hand-checked site picks were already taken by a pull request, which the scout only discovered at grading time; plus the four rubric misreads above, and the flood of tiny new repos from the `llm` keyword.
- What changed: I added `-linked:pr`, a repo-name check in the avoid filter, and a one-issue-per-repo cap to `search-strategy.md`; patched maintainer-responsive, the maintainer definition, the label-endorsement rule, and unclaimed in `rubric.md`; and logged each change with a dated reason in `scan-notes.md`.

---

## Understanding the Issue

**Problem Description**

Conversations, the French government's open-source AI chat app, calls LLM providers (Albert, Mistral, and other OpenAI-compatible APIs) and an MCP server (data.gouv.fr) without identifying itself. The teams who run those services can't tell which traffic comes from Conversations, so they can't trace its calls, measure its usage, or reach the right people when something goes wrong. The missing piece is a User-Agent header that names the app on every outgoing LLM and MCP call.

**Expected Behavior**

Every request Conversations sends to an LLM provider or MCP server carries the same agreed User-Agent naming the app, for example `conversations/<version>`. The exact format is the open question in my claim comment, which is waiting for the maintainer's answer.

**Current Behavior**

Only the Mistral provider sets a User-Agent explicitly (`chat/agents/base.py`, line 111), and the value it uses is pydantic-ai's `get_user_agent()`, which returns `pydantic-ai/<version>`: it names the library, not Conversations. The Albert and generic OpenAI-compatible providers in the same function pass no HTTP client or headers, so they fall back to whatever pydantic-ai and the OpenAI SDK send by default. The datagouv `MCPToolset` in `chat/mcp_servers.py` sets no headers either. I haven't yet confirmed the exact header each call sends on the wire; that is the first thing my reproduction will check.

**Affected Components**

- `src/backend/chat/agents/base.py`: builds the model for each provider (Mistral with an explicit `httpx2.AsyncClient` and User-Agent; `AlbertOpenAIProvider` and `OpenAIProvider` without one).
- `src/backend/chat/mcp_servers.py`: `DataGouvConnector.toolsets()` creates the `MCPToolset` for the data.gouv.fr MCP server. pydantic-ai 2.54.0's `MCPToolset` takes a `headers=` argument, which looks like the natural place to add it.
- The library side: pydantic-ai 2.54.0 (`get_user_agent()` in `pydantic_ai/models/__init__.py`, `_create_openai_client` in `pydantic_ai/providers/_openai_compatible.py`) and mcp 1.29.1, both pinned in `src/backend/uv.lock`.
- Tests for both files, plus a changelog entry, which CONTRIBUTING.md requires.

**Toolkit**

- Tool: no new tool this phase. I read the code with `gh`: I downloaded a snapshot of the repo, searched it for `User-Agent`, `get_user_agent` and `MCPToolset`, and read the pinned pydantic-ai 2.54.0 source on GitHub to see what its providers send by default.
- What it produced: the map above. Only one of three LLM providers sets a User-Agent, that one names pydantic-ai rather than the app, and the MCP client sets none.
- Difficulties: the default header for the OpenAI-compatible providers passes through two libraries (pydantic-ai's HTTP client and the OpenAI SDK), so reading the code alone can't tell me what goes over the wire.
- What changed: I wrote "not yet confirmed" instead of guessing, and made checking the actual headers the first reproduction step.

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-11**

My instructor approved #911, and I posted my claim comment asking the maintainer to confirm the User-Agent format: https://github.com/suitenumerique/conversations/issues/911#issuecomment-6812019713. Before writing it, I read the code so the question would be specific. Mistral already sends `pydantic-ai/<version>`, while the Albert/OpenAI providers and the datagouv MCP client set nothing. Decision: I won't write code until the format is agreed, because the issue says the spec isn't settled and that is exactly why my scout rejected it. I wasn't sure whether pydantic-ai adds its own default header when no HTTP client is passed, so the comment only says those providers "don't set one explicitly". Checking pydantic-ai 2.54.0 afterwards showed it does build a default client for them, so that careful wording was right.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- The issue itself, whose body named the open spec question: https://github.com/suitenumerique/conversations/issues/911
- Conversations' CONTRIBUTING.md (AI-assistance rules, signed-off commits, changelog entry): https://github.com/suitenumerique/conversations/blob/main/CONTRIBUTING.md
- pydantic-ai 2.54.0 source, for `get_user_agent()` and the OpenAI-compatible provider's default client: https://github.com/pydantic/pydantic-ai/blob/v2.54.0/pydantic_ai_slim/pydantic_ai/models/__init__.py
- GitHub's search syntax docs, where I found the `-linked:pr` qualifier that fixed my scout's search: https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests
