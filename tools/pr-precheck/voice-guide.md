# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making my first contributions, comfortable in Python
but new to this codebase. When I comment I'm telling you what I ran,
what I saw, and what I'll look at next. Expect facts I can show and
plans I control, not guesses dressed up as findings.

## Rules I write by

### Rule: Promise the investigation, not the fix

I only commit to the next step I control (reading code, running a
test, reporting back). No fix, no timeline, no "I'll have a PR up".

- Wrong: "I'll have this fixed and a PR up by the weekend."
- Right: "I'll trace where the parser handles a top-level list and report back here with what I find."

### Rule: Every "confirmed" points at output

If I say I reproduced something, the output that shows it is in the
same comment. If I don't have the output, I say what I did instead.

- Wrong: "Confirmed, this crashes for me too."
- Right: "Reproduced on Python 3.12 / commit abc1234: the traceback below ends in `AttributeError: 'list' object has no attribute 'items'`."

### Rule: Name this issue, not "this issue"

A claim names the thing only this issue has: the symptom, the file,
or the function. If my comment could be pasted onto any other issue,
I rewrite it.

- Wrong: "Hi, I'd like to work on this issue, please assign it to me."
- Right: "I'd like to take the top-level JSON array crash in `rag/generator/output_parser.py` as my first contribution here."

### Rule: Causes are hypotheses until I've shown them

I don't present a root cause as fact unless I've shown the line,
the test, or the output that proves it.

- Wrong: "The bug is definitely that `parse()` assumes a dict."
- Right: "My guess is that `parse()` assumes a dict, since it calls `.items()` on the result; I haven't confirmed that yet."

### Rule: Say what differed

When my environment, version, or result differs from the issue's, I
say so in one plain sentence instead of letting the reader assume it
matched.

- Wrong: "Same behavior here."
- Right: "The issue was reported on Python 3.14; I ran it on 3.12 and got the same traceback."

## Things I never post

- A deadline or a guarantee ("within 2 days", "guaranteed fix").
- "Please assign this to me" / "keep this reserved for me" in place of describing work.
- "Same as above, can confirm" or any +1 with no output of my own.
- Certainty words standing in for evidence: "100%", "definitely", "verified" with nothing shown.
- Flattery or filler ("Great project, sir!") and hype emojis.
- Text an AI wrote that I haven't read, run, and rewritten in my own words.
