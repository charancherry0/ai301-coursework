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
**Pull Request:** https://github.com/suitenumerique/conversations/pull/962

---

## Toolkit

Every story section below ends with a Toolkit block: which tool did that phase's work, what it
produced, what fell short, and what I changed because of it. The weekly entries below are the
toolkit's own timeline.

**Unit 5: Grew my issue-select skill into issue-scout, a scout that searches GitHub for me**

I installed issue-scout and wrote a search strategy with two `gh search issues` queries. Its first
scan showed my search kept surfacing issues that were already taken, so I added `-linked:pr`, and
real repos exposed four rubric misreads, which I patched. The details are in Why I Chose This Issue.
I didn't rescan to confirm the patches.

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

My issue-scout scan ran two `gh search issues` queries for Python and TypeScript `llm` issues
labelled `good first issue`, ranked 123 surviving results by fewest comments, and graded the top
five (2 accepted, 3 rejected). I then checked 15 picks from the Issue Selection site. None worked
out: ten already had an open pull request or a recent claim, and the other five were open-ended,
had competing fixes, or overlapped someone else's open PR.

A search with `-linked:pr` added found suitenumerique/conversations#911, "Add user-agent to MCP
and LLM calls", and my instructor approved it. The scout's verdict was **reject**. It passed every
check except settled-approach, because the collaborator who opened it wrote "Needs more spec'ing
with firsthand LLM (Albert) and MCP (datagouv) teams". I chose it anyway because I could remove
that blocker by asking. It also fits my profile: it's Python, it touches the LLM provider and MCP
client code of a generative-AI chat app, it's small enough for my five hours, and CONTRIBUTING.md
allows AI-assisted work.

Claim comment: https://github.com/suitenumerique/conversations/issues/911#issuecomment-6812019713

The scout's verdicts were mostly right, but real repos exposed gaps the course eval set never did.
Popular good-first-issues get several pull requests within days, and the scout only noticed at
grading time. It also misread real repos in four ways:

- It sampled same-day issues when checking whether maintainers reply.
- It missed a maintainer whom GitHub listed as CONTRIBUTOR.
- It treated a `help wanted` label added by an issue template as a maintainer endorsement
  (cherry-studio#19396).
- It missed an unlinked open pull request that rewrote the same functions (cherry-studio#24233).

It still ignores project triage bots, and `gh search` can't filter out tiny repos that are only
days old.

**Toolkit**

- Tool: issue-scout (my search strategy and rubric), for one full scan and then direct grading of
  site picks, with free `gh` checks first.
- What it produced: on #911, a reject on settled-approach ("Needs more spec'ing…") with every
  other required check passing.
- Difficulties: 9 of my 11 hand-checked picks were already taken, which the scout found only after
  I'd paid to grade them; plus the four misreads above.
- What changed: `-linked:pr`, a repo-name avoid check and a one-issue-per-repo cap in
  `search-strategy.md`; four patched checks in `rubric.md`, each logged with its reason in
  `scan-notes.md`.

---

## Understanding the Issue

**Problem Description**

Conversations, the French government's open-source AI chat app, calls LLM providers (Albert,
Mistral, and other OpenAI-compatible APIs) and an MCP server (data.gouv.fr) without identifying
itself. The teams who run those services can't tell which traffic comes from Conversations, so
they can't trace its calls, measure its usage, or reach the right people when something goes
wrong.

**Expected Behavior**

Every request Conversations sends to an LLM provider or MCP server carries the same User-Agent
naming the app, for example `conversations/<version>`. The exact format is the open question from
the issue.

**Current Behavior**

No outgoing call names Conversations. Each one sends the name of whichever library made it; the
measured values are in Reproduction Evidence. The code sets a User-Agent in only one place, on the
Mistral provider's HTTP client
([`base.py` line 111](https://github.com/suitenumerique/conversations/blob/da71d811ac7f9c179eda86a4324ee34fdd7f7c4f/src/backend/chat/agents/base.py#L111)),
and even there the Mistral SDK overwrites it.

**Affected Components**

- [`src/backend/chat/agents/base.py`](https://github.com/suitenumerique/conversations/blob/da71d811ac7f9c179eda86a4324ee34fdd7f7c4f/src/backend/chat/agents/base.py#L77):
  `prepare_custom_model()` builds the Mistral, OpenAI-compatible and Albert providers.
- [`src/backend/chat/mcp_servers.py`](https://github.com/suitenumerique/conversations/blob/da71d811ac7f9c179eda86a4324ee34fdd7f7c4f/src/backend/chat/mcp_servers.py#L219):
  `DataGouvConnector.toolsets()` creates the data.gouv `MCPToolset`.
- The libraries underneath: pydantic-ai 2.54.0, mistralai 2.9.4, openai 3.20.0 and mcp 1.30.0
  (over httpx 0.28.1), all pinned in `src/backend/uv.lock`.

**Toolkit**

- Tool: no course tool. I downloaded the repo with `gh`, searched it for `User-Agent`,
  `get_user_agent` and `MCPToolset`, and read the pinned pydantic-ai source.
- What it produced: a map of where each outgoing call is built.
- Difficulties: the header passes through two or three libraries, so reading the code couldn't
  tell me what actually goes over the wire.
- What changed: I wrote "not yet confirmed" instead of guessing, and made measuring the real
  headers the first reproduction step. The measurement later proved my Mistral guess wrong.

---

## Reproduction Process

**Environment Setup**

- Ubuntu 22.04.3 LTS on WSL2 (Windows), with uv 0.13.3 installing Python 3.14.8, which the project requires
  (`requires-python = "~=3.14.6"`; my system Python is 3.10).
- Conversations at `main` commit `da71d81` (2026-11-17), cloned from
  https://github.com/suitenumerique/conversations.
- Backend setup: `cd src/backend && uv sync --python 3.14 --all-extras --all-groups`. Tests then
  run with `uv run pytest …` using the repo's `env.d/test` settings.
- Challenges: plain `uv sync` skips the dev group, so `pytest` was missing ("Failed to spawn:
  pytest"); `--all-groups` fixed it. Tests marked `django_db` need Postgres on port 15432, which I
  didn't run. I wrote my reproduction so it needs no database, and I report the database-backed
  tests as not run.

**Steps to Reproduce**

1. Start a local HTTP server on `127.0.0.1` that records every request's `User-Agent` header and
   answers with a minimal chat completion.
2. Build each provider with the app's own `prepare_custom_model()` (kinds `mistral`, `openai`, and
   `openai` with `co2_handling="albert"`), point its `base_url` at the server, and run one agent
   turn. Connect a `MCPToolset` to the server's `/mcp` path.
3. Observed result: every call names a library, none names Conversations.

**Reproduction Evidence**

- **Commit showing reproduction:** reproduced on upstream `main`
  [`da71d81`](https://github.com/suitenumerique/conversations/commit/da71d811ac7f9c179eda86a4324ee34fdd7f7c4f).
  The same check became the wire tests in my fix commit
  [`851a806`](https://github.com/charancherry0/conversations/commit/851a806). Run on `main`, they fail with
  these values.
- **Screenshots/logs:**

  ```
  REPRO mistral  -> User-Agent: ['mistral-client-python/2.9.4']
  REPRO openai   -> User-Agent: ['pydantic-ai/2.54.0']
  REPRO albert   -> User-Agent: ['pydantic-ai/2.54.0']
  REPRO mcp      -> User-Agent: ['python-httpx/0.28.1']
  ```

- **My findings:** the issue is real for all four calls. My claim comment said Mistral sends
  pydantic-ai's string. The wire shows `mistral-client-python/2.9.4`, because the Mistral SDK
  writes its own header on every request, over the one the app puts on its HTTP client.

**Toolkit**

- Tool: a throwaway pytest file with a standard-library HTTP server that records headers,
  written with Claude Code. I didn't run my repro-check skill: like pr-precheck (see Pull
  Request), its scope is the course's Path Review repo.
- What it produced: the four measured headers above.
- Difficulties: the first run failed with "async def functions are not natively supported", because
  the repo runs pytest-asyncio in strict mode.
- What changed: I added `pytestmark = pytest.mark.asyncio`, and switched from `headers.get` to
  `headers.get_all` so a duplicated header couldn't hide.

---

## Solution Approach

**Analysis**

Each layer sets its own User-Agent per request, after anything the app sets as a client default:

- pydantic-ai's OpenAI model calls `extra_headers.setdefault('User-Agent', get_user_agent())` on
  every request.
- pydantic-ai's Mistral model passes `http_headers={'User-Agent': get_user_agent()}`.
- The Mistral SDK first writes `sdk_configuration.user_agent` into the headers
  (`mistralai/client/basesdk.py`).
- The MCP client uses plain httpx, so it sends httpx's default.

So a header set on an HTTP client default never reaches the wire for the LLM calls. It has to be
applied after the SDK builds each request.

**Proposed Solution**

- One helper, `chat/user_agent.py:get_user_agent()`, returns `conversations/<release>`. A new
  `OUTGOING_USER_AGENT` setting replaces it, because the format isn't agreed yet.
- Each LLM provider gets its own `httpx2.AsyncClient` with a `request` event hook that sets the
  header just before sending.
- The data.gouv `MCPToolset` passes `headers={"User-Agent": …}`.
- Not changing: timeouts (still 600 s, 5 s to connect, as Mistral already had), and providers
  resolved by pydantic-ai's built-in registry (non-custom models), which the issue doesn't name.

**Implementation Plan**

1. Add `chat/user_agent.py` and the `OUTGOING_USER_AGENT` setting in `conversations/settings.py`.
2. In `chat/agents/base.py`, add `_http_client()` and pass it to the Mistral, Albert and OpenAI
   providers.
3. In `chat/mcp_servers.py`, pass the header to `MCPToolset`.
4. Add wire-level tests in `chat/tests/test_user_agent.py`, a changelog line, and a row in
   `docs/env.md`.

Deviations:
- The setting wasn't in my claim comment. I added it because the issue says the format still needs
  agreeing with the Albert and data.gouv teams. The PR description states this deviation.
- My first build set the header as the HTTP client default, the way the Mistral code already did.
  Re-running the reproduction showed openai and albert still sending `pydantic-ai/2.54.0` and
  Mistral sending a mangled `mistral-client-python/pydantic-ai/2.54.0`. I switched to the request
  hook (step 2).

**Toolkit**

- Tool: Claude Code, used to read the pinned library source (`pydantic_ai/models/openai.py`,
  `models/mistral.py`, `mistralai/client/basesdk.py`) and to draft the change.
- What it produced: the per-request override chain above, and the request-hook design.
- Difficulties: the first design looked right and passed a code read, but failed on the wire.
- What changed: I now re-run the reproduction after every build, not just at the end.

---

## Testing Strategy

1. **The reproduction, re-run against the fix.** `chat/tests/test_user_agent.py` re-runs the four
   wire checks from Reproduction, asserting the exact header list. Run with
   `uv run pytest chat/tests/test_user_agent.py`:
   - On `main` (fix stashed): `4 failed, 2 passed`, for example
     `assert ['mistral-client-python/2.9.4'] == ['conversations-test/1.0']`.
   - With the fix: `6 passed`, and every call sends
     `conversations/0.0.29` (or the configured value).
2. **The setting.** Two unit tests check the default (`conversations/{settings.RELEASE}`) and the
   override.
3. **Nearby suites.** `uv run pytest chat/tests/agents` gave 108 passed and 8 errors. All 8 need
   Postgres, which I didn't run, so the full `make test` hasn't been run on my machine; the PR's
   CI will run it.
4. **Lint.** `ruff check`, `ruff format --check` and `pylint` on the changed files: clean
   (pylint 10.00/10).

**Toolkit**

- Tool: pytest with the repo's settings, plus `git stash` to run the same tests on the unfixed code.
- What it produced: the before/after results above.
- Difficulties: my first MCP test built the connector with `UserFactory`, which needs the database,
  so it errored ("connection to server at 127.0.0.1, port 15432 failed").
- What changed: the test builds `DataGouvConnector(enabled=True, opted_in=True)` directly, so it
  runs without Postgres and still goes through the app's real `toolsets()` code.

---

## Implementation Notes

**2026-11-11**

My instructor approved #911, and I posted my claim comment asking the maintainer to confirm the
User-Agent format. Decision: no code until the format was agreed, because that's exactly why my
scout rejected the issue.

**2026-11-18**

There was still no reply after six days, so I built the fix with the format configurable. That
keeps the open decision with the maintainers instead of guessing it. Measuring the real headers
showed my Mistral assumption was wrong. My first build (a client default header) failed on the
wire, so I moved the header into a request hook. Committed as
[`851a806`](https://github.com/charancherry0/conversations/commit/851a806), signed off and SSH-signed
as CONTRIBUTING.md requires. I registered my SSH key as a GitHub signing key, and GitHub shows the
commit as verified.

**Toolkit**

- Tool: git and gh: a branch `user-agent-911` on my fork, `git commit -S --signoff`.
- What it produced: one commit, 7 files, +171/−6.
- Difficulties: GitHub didn't recognise my SSH key for signing until I refreshed `gh` with the
  `admin:ssh_signing_key` scope and added the key as a signing key.
- What changed: the key is registered, so future commits verify too.

---

## Pull Request

**PR Link:** https://github.com/suitenumerique/conversations/pull/962

**PR Description:** The description follows the repo's PR template:
- **Purpose:** closes #911, with the measured before/after header table.
- **Proposal:** the helper, the request hook (and why a client default isn't enough), the MCP
  header, the tests, the changelog line and the `docs/env.md` row.
- **Deviation note:** the `OUTGOING_USER_AGENT` setting, and an offer to change the default
  format.
- **Tests run:** with the database-backed tests stated as not run.
- **AI use:** says I used Claude Code and ran the tests myself.
- **Checklist:** every External contributions item ticked.

**Maintainer Feedback:**

- 2026-11-18: none yet

**Status:** Awaiting review

**Toolkit**

- Tool: my pr-precheck skill, to grade the PR before opening it.
- What it produced: a refusal. Its `scope.md` only allows the course's Path Review repo, so it
  wouldn't grade a PR to `suitenumerique/conversations`.
- Difficulties: I tried to widen that scope, but my permission setup blocked edits to the
  staff-written `scope.md`.
- What changed: I checked the PR by hand against the repo's own checklist: signed and signed-off
  commit, gitmoji message, changelog line, tests, and the template sections. Next time, I'd add a
  wild-repo scope to the tool before I need it, with my instructor's OK.

---

## Learnings and Reflections

**Technical Skills Gained**

- How a request's headers get built in layers: pydantic-ai, the provider SDK and httpx each set
  their own User-Agent, and only the last one before sending wins. An httpx `request` event hook
  is the reliable place to override a header.
- Testing what goes over the network with a standard-library HTTP server, instead of trusting
  what the code seems to set.
- Running an unfamiliar Django project's tests with uv on a newer Python than my system has, and
  separating tests that need Postgres from ones that don't.
- Commit signing with an SSH key, and the DCO sign-off.

**Challenges Overcome**

- My first fix was wrong, and only the wire check showed it. Re-running the reproduction after
  the build caught it before a reviewer did.
- My claim comment's guess about Mistral was wrong. I corrected it in the PR instead of keeping
  it.
- The open spec: rather than wait without end or guess the format, I made it a setting and said so
  in the PR.

**What I'd Do Differently Next Time**

- Measure first, then write the claim comment. My comment carried a wrong guess about Mistral
  that ten minutes with a recording server would have caught.
- Prove each tool change with the smallest check. I patched four rubric checks without re-grading
  the issues that exposed them; re-grading just those four would have cost little.
- Check that my course tools' scope covers the repo I'm really contributing to before I depend on
  them at PR time.

---

## Resources Used

- The issue itself, whose body named the open spec question: https://github.com/suitenumerique/conversations/issues/911
- Conversations' CONTRIBUTING.md (sign-off, signed commits, gitmoji, changelog, AI rules): https://github.com/suitenumerique/conversations/blob/main/CONTRIBUTING.md
- pydantic-ai 2.54.0 source, for `get_user_agent()` and where each model sets the header per request: https://github.com/pydantic/pydantic-ai/blob/v2.54.0/pydantic_ai_slim/pydantic_ai/models/__init__.py
- GitHub's search syntax docs, where I found the `-linked:pr` qualifier: https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests
- GitHub's docs on SSH commit signing: https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification
