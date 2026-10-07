# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Nishanth-Raju

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-6030599236

```markdown
## Plan

Following up on my reproduction (Windows 11, Git Bash, Python 3.12.6, pytest 9.1.1, at `2f4e82f`): with `--runxfail` the test fails at `assert 51 > 100`, and the scorer logs `category=minimal ... word_count=51`.

**Diagnosis:** I think the fixture is what's wrong, not the scorer. My control run on plain text gave `51 minimal`, `111 adequate`, `499 adequate`, `500 comprehensive`, which matches the thresholds in `agent/tools/readme_scorer.py`. The test asserts both `word_count > 100` and `word_count_category == "comprehensive"`, so the fixture needs 500+ words, not just over 100.

**Change (one file, `tests/unit/test_readme_scorer.py`):**
1. Extend the README fixture in `test_readme_with_all_quality_signals` to 500+ words of real README sections, keeping the installation, usage, badge, demo-link and tech-stack signals the test checks.
2. Remove the `xfail(strict=True)` marker for #63, as CONTRIBUTING.md asks.

**Not touching:** the scorer's thresholds or regexes, the test's assertions, or any other test.

**Test:** the file should go from `22 passed, 1 xfailed` to `23 passed`, and the scorer's log line for this test should show `category=comprehensive` with 500+ words. `make test-unit` and `make lint` should stay green.

**Open question:** if you'd rather keep the short fixture and assert `adequate` instead, I'll switch to that. I went with the fixture because the test is meant to be a README with every quality signal.

I used Claude Code to help draft this plan; I ran the commands above myself and checked the output.
```

---

## Your branch

**Branch**

fix/63-readme-scorer-fixture-length

**Evidence**

Environment for both runs: Windows 11 (build 10.0.26200), Git Bash, Python 3.12.6,
pytest 9.1.1. These are the Unit 2 reproduction steps 1 and 2.

Before: `main` at `2f4e82f`:

```
$ .venv/Scripts/pytest tests/unit/test_readme_scorer.py -q -rx
x......................                                                  [100%]
=========================== short test summary info ===========================
XFAIL tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - issue #63: README scorer fixture is too short for its own word-count assertion
22 passed, 1 xfailed in 0.55s

$ .venv/Scripts/pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -q --runxfail
>       assert data["word_count"] > 100
E       assert 51 > 100
tests\unit\test_readme_scorer.py:60: AssertionError
---------------------------- Captured stdout call -----------------------------
2026-10-06 23:01:05 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals
1 failed in 0.32s
```

After: `fix/63-readme-scorer-fixture-length` at `ac88acd` (the `xfail` marker is gone,
so step 2 no longer needs `--runxfail`):

```
$ .venv/Scripts/pytest tests/unit/test_readme_scorer.py -q -rx
.......................                                                  [100%]
23 passed in 1.10s

$ .venv/Scripts/pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -q -s
2026-10-06 23:20:58 [info     ] readme_scored                  category=comprehensive score=1.0 word_count=565
.
1 passed in 0.83s
```

Rest of the checks on the branch:

```
$ .venv/Scripts/pytest tests/unit -q
376 passed, 52 xfailed, 5 warnings in 24.51s
$ .venv/Scripts/ruff check .
All checks passed!
$ .venv/Scripts/black --check tests/unit/test_readme_scorer.py
1 file would be left unchanged.
$ .venv/Scripts/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 27 source files
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Calibration smoke run (`--include-calibration --only calib-01,calib-02,calib-03,calib-04`,
   unscored): all four agreed (`calib-01 accept`, `calib-02/03/04 reject`).
2. Full run 1: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.
   Categories: `clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   The one miss was `pkg-14  clear-accept  accept  reject   NO     failed: executable`.
3. Canary run after revising `executable` (`--include-calibration --only pkg-14,pkg-10,pkg-17,pkg-18,calib-02`):
   `agreement: 4/4 scored items`, `categories: clear-accept 1/1  unbuildable 3/3`, and
   calib-02 still rejected.
4. Full run 2: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.
   Categories: `clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   This is the committed `eval-run.txt`.

**Package analysis**

**pkg-14** (zellij-org/zellij#5174, OSC color sequences leak into the terminal on
session reattach via SSH). Gold: **accept**. Run 1: **reject**. Run 2: **accept**.

The plan passes every other check easily. Its diagnosis (the reattach path "wires the
client's stdin to the session before the query responses have been consumed") fits
both controls: `zellij 0.44.1: 5 reattach cycles, no leak`, and the cache-clear run
where "the next attach is clean, the one after leaks again". Its scope is one change
with explicit deferrals ("Explicitly deferred, with reasons: the Windows
session-switch variant"). Its test plan is decisive: "5 consecutive SSH reattach
cycles with no rgb strings in any pane".

Run 1 rejected it on `executable` alone. The skill's evidence was: `"exact functions
to be pinned in the PR after tracing the query issuance with debug logs" — specific
location deferred to build-time investigation; a stranger cannot start building
without performing the author's tracing work`. My first pass condition asked for "a
file, function, branch, or call site", so a plan that names a code path but not a
function looked like an unnamed location. But the plan does name where the change
lands: "consuming or draining pending OSC query responses in the client attach path
in `zellij-server`'s client connection handling before pane input is wired". That
is a place and an action. Only the function names are left to the build, and that's
not the same as pkg-18's "recover() 'somewhere'". The rubric was too literal about
the shape of a location. After the revision, run 2 graded it accept.

**Check rationale**

> | executable | The plan's change/approach section and the files or code locations it names | A stranger could start building without asking the author anything: the plan names where the change lands (a file, function, call site, or a named code path inside a named module, such as "the reattach path in `zellij-server`'s client connection handling") AND commits to one approach for what the change does there. Pinning exact function names during the build is fine once the code path and the approach are fixed. Fail if the location is "somewhere" or unnamed, the approach is still an investigation ("profile it", "dig into the input stack"), or the key choice is left open ("upstream or vendored, whichever is easier", "gocui? tcell?"). Naming one open question for review while still committing to an approach passes | required |

The first version said the plan "names where the change lands (a file, function,
branch, or call site) AND commits to one approach". It read the location as a list
of allowed shapes, so pkg-14's "client attach path in `zellij-server`" didn't count
because it wasn't a function name. The question I actually care about is "what would
a stranger do first?", and pkg-14 answers it: open the reattach path in
`zellij-server` and drain the OSC responses before input is wired.

I thought about dropping the location requirement and only checking for a committed
approach, and rejected that. The unbuildable packages fail on location as much as on
approach: pkg-18 is "recover() 'somewhere'", and pkg-10 is "profile-and-optimize
with no files". So the revision adds exactly one more kind of location (a named code
path in a named module) and keeps the fail cases spelled out. It also says pinning
function names during the build "is fine once the code path and the approach are
fixed", so a grader can't read "pinned in the PR" as a missing location again. The
procedure's step 4 got the same test ("if the plan answers that with a place and an
action, pass").

**Trade-offs**

Loosening `executable` gives up some strictness. A plan that names a big module
("the networking code in `core/`") and a vague action could now pass this check,
because "a named code path inside a named module" can be read generously. I accept
that it may miss that case and rely on the second half of the check: the approach
still has to be committed ("profile it" and "dig into the input stack" still fail),
and `test-decisive` still needs an observable outcome.

The change flipped pkg-14 from reject to accept (gold: accept). To check for flips
the other way before paying for a full run, I re-ran with
`--include-calibration --only pkg-14,pkg-10,pkg-17,pkg-18,calib-02`. These are
every package in the `unbuildable` category (the category this check exists for),
plus calib-02, the worksheet's "poke around the editor code this weekend" plan.
All stayed reject (`agreement: 4/4 scored items`, `unbuildable 3/3`). The confirming
full run 2 then scored 20/20 with every category at full count, so nothing else
changed.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
