# Plan: issue #69, output parser crashes on a top-level JSON array

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69
Branch (on my fork): `fix/69-json-array-fallback`

## Repro evidence this plan builds on

From my repro comment on #69 (posted 2026-10-20). Ubuntu 22.04 (WSL2),
Python 3.11.17, pytest 9.1.1, commit `f89c06f` on `main`, installed
with `pip install -e ".[dev]"`.

Raw array, crashes from the raw-JSON branch (line 48):

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps(["First feedback item", "Second feedback item"])))'
  File ".../rag/generator/output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
  File ".../rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'
```

Control, a top-level object, works:

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps({"strengths": "clear README"})))'
[FeedbackSection(section_name='strengths', content='clear README', confidence=0.85, suggestions=[])]
```

Fenced array, same crash from the fenced branch (line 41):

```
$ .venv/bin/python -c 'from rag.generator.output_parser import parse_review_output; print(parse_review_output("```json\n[\"First feedback item\"]\n```"))'
  File ".../rag/generator/output_parser.py", line 41, in parse_review_output
    return _parse_json_output(data)
  File ".../rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'
```

Covering test, forced to run:

```
$ .venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
E       AttributeError: 'list' object has no attribute 'items'
rag/generator/output_parser.py:68: AttributeError
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed, 18 deselected in 0.25s
```

One thing I checked after posting the repro, on the same checkout
(not in the posted comment): other JSON values that aren't objects
crash the same way.

```
"just text" -> AttributeError: 'str' object has no attribute 'items'
42          -> AttributeError: 'int' object has no attribute 'items'
null        -> AttributeError: 'NoneType' object has no attribute 'items'
[]          -> AttributeError: 'list' object has no attribute 'items'
```

## Diagnosis

`parse_review_output()` in `rag/generator/output_parser.py` has two
places that call `json.loads` (the fenced branch at line 40 and the
raw branch at line 47). Both pass the result straight to
`_parse_json_output()`, which is typed `data: dict` and starts with
`for key, value in data.items():` (line 68). Nothing checks that the
parsed value is actually a dict. A top-level array is valid JSON, so
`json.loads` succeeds, the list reaches `.items()`, and it raises
`AttributeError`. Both branches only catch `json.JSONDecodeError`, so
the error is never caught and the plain-text fallback
(`_parse_plaintext_output`) is never reached.

This matches all of the evidence above: the traceback goes through
line 41 or 48 into line 68; an object (the control) works because it
has `.items()`; and the fenced and raw arrays fail the same way
because both branches make the same call with no check.

## Scope

In scope: in `parse_review_output()`, only hand the parsed value to
`_parse_json_output()` when it is a dict. Anything else falls through
to the plain-text fallback, which is the fallback path the issue asks
for. This applies to both the fenced and the raw branch, and the
covering test's xfail marker comes off.

Not in scope:

- Turning array items into one `FeedbackSection` each. The issue asks
  for the fallback path, and array items have no section names to
  map to. See Risks and unknowns.
- `_parse_json_output()` itself, including how it handles objects.
- The seeded `sections = []  # noqa: F841` line at the top of
  `parse_review_output()` and its `var-annotated` mypy override in
  `pyproject.toml`. CONTRIBUTING.md asks that these be left alone; they
  belong to a different seeded defect.
- `rag/generator/review_generator.py`, the only caller. It already
  handles a single plain-text section.
- Dedicated handling or tests for strings, numbers, and `null`. The
  same dict check sends them to the fallback too, so I'll check them by
  hand (Test plan step 4) but won't add separate tests.

## Files

- `rag/generator/output_parser.py`: `parse_review_output()`, lines
  36-48.
- `tests/unit/test_output_parser.py`: remove the `@pytest.mark.xfail`
  on `test_json_array_fallback` (lines 141-144) and add one test for a
  fenced array.

## Approach

1. Create branch `fix/69-json-array-fallback` from `main` on my fork.
2. In the fenced branch of `parse_review_output()`, after
   `data = json.loads(json_str)`, call `_parse_json_output(data)` only
   if `isinstance(data, dict)`. Otherwise log a warning (e.g.
   `json_not_object_in_fence` with the type name) and fall through.
3. Make the same change in the raw branch after `data = json.loads(raw)`
   (e.g. `raw_json_not_object`), so a non-object falls through to
   `_parse_plaintext_output(raw)`.
4. In `tests/unit/test_output_parser.py`, delete the xfail marker on
   `test_json_array_fallback`. Add `test_fenced_json_array_fallback`,
   which passes `"```json\n[\"First feedback item\"]\n```"` and asserts
   that it returns one section named `general_feedback` and doesn't
   raise.
5. Run `make check && make test-unit`, then commit as
   `fix(rag): fall back to plain text for non-object JSON output` with
   `Fixes #69`.

## Test plan

Rerun my posted repro steps on the fixed branch:

1. Raw array call (same command as the repro). Expected after the fix:
   no traceback. The output is
   `[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]`.
   Before the fix, this raises the `AttributeError` above.
2. Fenced array call (same command as the repro). Expected: no
   traceback, and one `general_feedback` section whose content is the
   whole fenced input.
3. Object control (same command). Expected: unchanged,
   `[FeedbackSection(section_name='strengths', content='clear README', confidence=0.85, suggestions=[])]`.
   This shows objects still go through `_parse_json_output()`.
4. Non-object values: rerun the `"just text"`, `42`, `null`, and `[]`
   inputs. Expected: each returns one `general_feedback` section, with
   no `AttributeError`.
5. `.venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback -q`
   with the marker removed. Expected: `1 passed`. Before the fix (with
   `--runxfail`) it was `1 failed`.
6. `.venv/bin/pytest tests/unit/test_output_parser.py -q`. Expected:
   all tests pass, including the new fenced-array test, with 0 xfailed
   and no `XPASS(strict)`.
7. `make check && make test-unit` passes before I push, and CI is
   green on the PR (all five jobs).

## Risks and unknowns

- What an array should become is a design choice I'm making, not
  something I've confirmed with a maintainer. I'm treating it as plain
  text because the issue says "the fallback path should handle array
  responses" and the test only asserts a list comes back. If a
  maintainer would rather have one section per array item, the change
  would be in `_parse_json_output()` instead, and I'd update this plan
  before building.
- After the fix, a fenced array's content is the whole raw text,
  fences included. That's what `_parse_plaintext_output(raw)` already
  does for any text that isn't JSON, and I'm not changing it.
- When a fenced array falls through, the raw branch will also log
  `raw_json_parsing_failed`, because the fenced text isn't valid JSON
  as a whole. That log line is a little noisy but harmless, and it
  already happens today for fenced text with invalid JSON.
- `review_generator.py` gets back a `general_feedback` section with
  confidence 0.7 instead of crashing. I haven't run the full review
  flow end to end (it needs an LLM client), so the claim that it's
  fine downstream comes from reading lines 75-84, not from running it.
- Another student on the thread (jacho15) proposed the same dict check.
  I reached it from my own repro.

## Deviations

<!-- Fill in after the build: what changed from this plan and why. -->
