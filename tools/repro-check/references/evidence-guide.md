# Evidence guide: where proof lives in a reproduction package

The rubric names a family for each check; this file says where that
family's proof lives and what good looks like there. In an eval bundle
the sections are: `Repo facts`, `Issue` (title, body, environment the
reporter gave), `Thread highlights`, `Candidate claim comment`, and
`Candidate repro report`. In live mode the same things live on GitHub
(the issue body and comments, the repo's CONTRIBUTING / AI policy /
issue templates) and in the student's draft files.

## Environment

- Where it lives: the environment line(s) of the repro report (usually
  near the top: "Environment: ..."). Compare against the version, OS,
  and settings in the issue body, any version/build a maintainer names
  in the thread highlights, and the repo-facts `latest release` line.
  Live: the issue body and comments, and the student's repro draft.
- What good looks like: the report names the software version it ran
  and the OS/platform, plus any setting the issue says matters (driver,
  browser language, build type, shell). Where it differs from the
  issue's target, the report says so in a sentence ("filed against
  13.0.0; still present on 15.2.0"). An older version than the issue
  targets, or a different build/channel, that goes unmentioned is a
  silent deviation: the artifact may show that version's behavior, not
  the reported bug.

## Steps

- Where it lives: the repro report's numbered steps and/or command
  transcript (`$ ...` lines in code blocks), the files or snippets it
  creates, and the config it shows. Compare against the trigger in the
  issue body and any trigger condition added in the thread highlights
  (e.g. an owner noting a flag is required). Live: the draft's steps
  against the issue's steps and the repo's setup docs.
- What good looks like: someone with only public resources starts from
  nothing and reaches the trigger by copying what is shown: exact
  commands, plus inputs and config either shown or described concretely
  enough to recreate (which sections a file has, which character it
  contains), or an explicit "ran the issue's script verbatim". Every
  required trigger condition is present. A cannot-reproduce attempt is
  re-runnable when the attempt itself can be repeated; it does not
  need to have reached the trigger. Steps that
  live in a private repo, use an unshared config, or say "set it up"
  without showing how are not re-runnable, however plausible they read.

## Behavior shown

- Where it lives: the artifacts in the repro report: output excerpts,
  error text, tracebacks, exit codes (`echo $?`), log lines, rendered
  output, and control runs. Read them against the issue body's
  "actual" behavior (its error message, exit code, wrong output), and
  read the input the report actually ran against the issue's trigger
  input.
- What good looks like: the artifact shows the same thing the issue
  shows (same error class or message, same exit status, same wrong
  output) from the issue's own trigger. A control run (same command
  minus the trigger) that behaves correctly is strong support. Adjacent
  behavior does not count: a graceful argument-validation error (exit
  1) is not a panic (exit 101); a compile error from a changed
  expression is not the reported runtime error; garbled output with
  the process still alive is not a crash; a version banner or "the app
  launched" shows setup, not the bug. If the input differs from the
  issue's (different syntax, operator, variable), the artifact is about
  that input, not the issue.

## Honesty

- Where it lives: the report's "Actual", "Result", and "Conclusion"
  sentences and the claim comment's claims about the reproduction,
  each read against the artifacts above.
- What good looks like: every claim points at something shown.
  "Reproduced" means an artifact shows the issue's behavior. A root
  cause appears only as a hypothesis unless evidence for it is shown. No
  claim reaches past what was tested (another version, channel, or OS).
  An honest cannot-reproduce says so in its first lines, shows the real
  attempt and its output, and names what differed from the issue's
  conditions; that is a complete, postable result. Red flags:
  certainty language standing in for evidence ("100% reproducible",
  "ran it ten times", "I verified the race"), and prose that calls an
  artifact a confirmation when the artifact shows something else.

## Comms

- Where it lives: the candidate claim comment read against the issue;
  the repo-facts `bug reports` line (template asks) and `contribution
  policy` line (including any AI policy) read against both comments.
  Live: the repo's CONTRIBUTING.md, AI_POLICY.md, issue templates, and
  the student's draft comments.
- What good looks like (claim): the comment names something only this
  issue has (its symptom, scenario, a file or function, a pointer from
  the thread) and a concrete next step the writer can control
  ("trace where X is emitted and report back"). It promises
  investigation, not a fix or a date, and it describes work instead of
  asking to be assigned. Boilerplate test: if the same text could be
  pasted onto any other issue, it fails.
- What good looks like (disclosure): first decide whether the policy
  actually requires disclosing AI use in issues or comments. "All AI
  usage in any form must be disclosed" does; "state the tool in the
  pull request" applies to PRs only; "comments must be in your own
  words", "you are responsible for AI output", and "understand what you
  submit" are not disclosure requirements. Where disclosure is
  required, a line in the claim or report stating the tool and what it
  helped with satisfies it (e.g. "I used an AI assistant to organize
  this report; I ran every step myself"). Eval packages are treated as
  AI-assisted, so a required disclosure that is missing fails, however
  good the proof is.
- What good looks like (template): the report covers what the repo's
  bug template asks for (version, OS, steps, expected, actual, named
  command output). This is a preferred signal only.
