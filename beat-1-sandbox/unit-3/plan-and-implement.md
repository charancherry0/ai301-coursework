# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

---

## Posted upstream

**GitHub username**

charancherry0

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69#issuecomment-6400142592

Plan for the top-level JSON array crash in `rag/generator/output_parser.py`, building on my repro above (Python 3.11.17, commit `f89c06f`).

**Diagnosis.** Both `json.loads` calls in `parse_review_output()` (the fenced branch at line 40, the raw branch at line 47) hand whatever they parse straight to `_parse_json_output()`, which calls `data.items()` at line 68. My repro shows this: the array tracebacks run through line 41 (fenced) and line 48 (raw) into line 68, and the object control returns a section normally. Both branches only catch `JSONDecodeError`, so the plain-text fallback is never reached. Since posting the repro, I've also checked that `"just text"`, `42`, and `null` crash the same way (`'str'` / `'int'` / `'NoneType' object has no attribute 'items'`).

**Change.** In both branches, only call `_parse_json_output()` when the parsed value is a dict. Anything else logs a warning and falls through to `_parse_plaintext_output()`, the fallback path the issue describes. Files: `output_parser.py` (`parse_review_output()` only) and `tests/unit/test_output_parser.py` (remove the H-02 `xfail` marker on `test_json_array_fallback` and add a fenced-array test). Out of scope: `_parse_json_output()`, the `# noqa`'d `sections = []` line, and `review_generator.py`. Branch: `fix/69-json-array-fallback` on my fork.

**Test.** Rerun my repro commands. The raw array should return `[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]` instead of the `AttributeError`, the fenced array should return one `general_feedback` section, and the object control should be unchanged. `test_json_array_fallback` should pass without its marker (it failed with `--runxfail` above), and `make check && make test-unit` should be green.

**Open question.** I'm treating an array as plain text, not as one section per item, because the issue asks for the fallback path and array items have no section names. If one section per item is preferred, I'll change the plan before building. This is the same dict check jacho15 suggested above. I reached it from my own repro

---

## Your branch

**Branch**

fix/69-json-array-fallback

**Evidence**

Before: my Unit 2 repro, posted on #69 (commit `f89c06f` on `main`, Python 3.11.17):

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps(["First feedback item", "Second feedback item"])))'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File ".../rag/generator/output_parser.py", line 48, in parse_review_output
    return _parse_json_output(data)
  File ".../rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'

$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps({"strengths": "clear README"})))'
[FeedbackSection(section_name='strengths', content='clear README', confidence=0.85, suggestions=[])]

$ .venv/bin/python -c 'from rag.generator.output_parser import parse_review_output; print(parse_review_output("```json\n[\"First feedback item\"]\n```"))'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File ".../rag/generator/output_parser.py", line 41, in parse_review_output
    return _parse_json_output(data)
  File ".../rag/generator/output_parser.py", line 68, in _parse_json_output
    for key, value in data.items():
AttributeError: 'list' object has no attribute 'items'

$ .venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback -q
18 deselected, 1 xfailed in 0.25s
$ .venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
>       for key, value in data.items():
E       AttributeError: 'list' object has no attribute 'items'
rag/generator/output_parser.py:68: AttributeError
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback
1 failed, 18 deselected in 0.25s
```

After: the same commands on `fix/69-json-array-fallback` (commit `b2457d0`):

```
$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps(["First feedback item", "Second feedback item"])))'
2026-10-25 12:36:26 [warning  ] raw_json_not_object            json_type=list
2026-10-25 12:36:26 [info     ] plaintext_output_parsed        content_length=47
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]

$ .venv/bin/python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps({"strengths": "clear README"})))'
2026-10-25 12:36:26 [info     ] json_output_parsed             section_count=1
[FeedbackSection(section_name='strengths', content='clear README', confidence=0.85, suggestions=[])]

$ .venv/bin/python -c 'from rag.generator.output_parser import parse_review_output; print(parse_review_output("```json\n[\"First feedback item\"]\n```"))'
2026-10-25 12:36:26 [warning  ] json_not_object_in_fence       json_type=list
2026-10-25 12:36:26 [warning  ] raw_json_parsing_failed
2026-10-25 12:36:26 [info     ] plaintext_output_parsed        content_length=35
[FeedbackSection(section_name='general_feedback', content='```json\n["First feedback item"]\n```', confidence=0.7, suggestions=[])]

$ .venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback -q
1 passed, 19 deselected in 0.19s
$ .venv/bin/pytest tests/unit/test_output_parser.py -k test_json_array_fallback --runxfail -q
1 passed, 19 deselected in 0.16s
```

## Eval iterations

**Run history**

1. Calibration check (`--include-calibration --only calib-01,calib-02,calib-03,calib-04`): agreement 0/0 scored items; all 4 calibration packages agreed with the gold label (calib-01 accept, calib-02/03/04 reject). Calibration packages are never scored.
2. Full run (`--save-run eval-run.txt`): agreement: 20/20 scored items (bar: 18/20: PASS).

**Package analysis**

pkg-04 (category `thread-convention`, fzf#4260). My rubric decided reject; the gold label is reject. The diagnosis itself passed: "Cause (fzf keeps reading console input during execute child) matches Actual line and both control runs." It was held because the plan is docs-only while the cause stays in the code ("Not in scope: any change to fzf's input handling code"), its test plan uses the `> /dev/tty` form that already works today and so "cannot fail before the change", and the comment-thread-aware check caught that it "Ignores OWNER's culprit in light_windows.go and patched binary 8916cbc; proposes docs only." The thread is where the owner had already named `src/tui/light_windows.go` and posted a test binary, so a plan that documents a workaround instead reads past the maintainer's direction.

**Check rationale**

> | test-plan-decisive | The plan's test plan, read against the repro evidence's steps, inputs, and observed output. | Pass if it names at least one check that uses the reproduced steps or inputs, fails on the current code, and passes after the fix, with the expected result stated so anyone can observe it. Fail on "run the tests", "verify it works", or checks unrelated to the reproduced behavior. | required |

It reads this way because the failure family is "the test plan proves nothing observable", and the template warned that structure-shaped checks make graders disagree. I rejected a check like "the plan has a test plan section" in favour of an outcome rule: the test has to fail on today's code and pass after the fix. That is what separates a real test from "run the full test suite" (calib-04) or a test that already passes (pkg-04's `/dev/tty` check). I did not revise it between runs; it held on the first full run.

**Trade-offs**

Requiring a check that "fails on the current code" means a plan that legitimately changes no behaviour (a docs-only or test-coverage issue) can never pass test-plan-decisive, so my rubric would hold a good docs plan; I accept that miss because every package in this set is a behaviour bug. Nothing changed elsewhere because I made no revisions: the rubric, procedure, and evidence guide in `eval-run.txt`'s fingerprints are the same files the calibration run graded (4/4 agreed), and the full run was the only scored run.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
