# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

Nishanth-Raju

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-6030598060

````markdown
Hi, I'd like to investigate this one. My first step is to reproduce the `assert 51 > 100` failure on Windows, then check what word count the test's second assertion (`word_count_category == "comprehensive"`) actually needs. Looking at the scorer, extending the fixture just past 100 words may not be enough. I'll post what I find here before changing anything.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63#issuecomment-6030598647

````markdown
### Reproduction on Windows

**Environment**
- OS: Windows 11 (build 10.0.26200), commands run in Git Bash as `docs/SETUP.md` says for Windows
- Python 3.12.6, pytest 9.1.1
- Commit `2f4e82f` (`main`), clean working tree
- Setup: `cp .env.example .env`, `python -m venv .venv`, `.venv/Scripts/pip install -e ".[dev]"`, which are the dependency steps of `make setup`. I skipped Docker and the DB migration/seed steps because this unit test doesn't touch the database.

**Steps and output**

1. The issue's command. The test is marked `xfail(strict=True)` for #63, so it shows as `x` rather than a failure:

```
$ .venv/Scripts/pytest tests/unit/test_readme_scorer.py -q -rx
x......................                                                  [100%]
XFAIL tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals - issue #63: README scorer fixture is too short for its own word-count assertion
22 passed, 1 xfailed in 0.31s
```

2. The same test with the marker ignored, to see the real assertion:

```
$ .venv/Scripts/pytest "tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals" -q --runxfail
    [... fixture source lines trimmed ...]
>       assert data["word_count"] > 100
E       assert 51 > 100

tests\unit\test_readme_scorer.py:60: AssertionError
---------------------------- Captured stdout call -----------------------------
2026-09-29 23:39:46 [info     ] readme_scored                  category=minimal score=0.8717142857142858 word_count=51
FAILED tests/unit/test_readme_scorer.py::TestReadmeScorer::test_readme_with_all_quality_signals
1 failed in 0.30s
```

3. Control: the scorer on plain text at the category boundaries, to check whether the scorer or the fixture is off:

```
$ .venv/Scripts/python -c "
from agent.tools.readme_scorer import ReadmeScorer
s = ReadmeScorer()
for n in (51, 111, 499, 500):
    d = s.execute({'readme_content': 'word ' * n}).data
    print(n, d['word_count'], d['word_count_category'])
"
51 51 minimal
111 111 adequate
499 499 adequate
500 500 comprehensive
```

**Expected:** the test passes, because its fixture is meant to be a README with every quality signal.

**Actual:** the fixture is 51 words, so it fails at `word_count > 100`. That matches the issue.

**What I noticed:** the scorer counts correctly and follows its thresholds in `agent/tools/readme_scorer.py` lines 68-73 (`< 100` minimal, `< 500` adequate, else comprehensive). So a fixture padded to just over 100 words would pass the first assertion and still fail the next one (`word_count_category == "comprehensive"`), which needs 500+ words. I think the fix is either a fixture of at least 500 words or an assertion changed to match the intended category. I haven't changed anything yet.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Calibration smoke run (`--include-calibration --only calib-01,...,calib-04`, unscored):
   all four agreed (`calib-01 accept`, `calib-02/03/04 reject`).
2. Full run 1: `agreement: 18/20 scored items  (bar: 18/20: PASS)`.
   Categories: `clear-accept 6/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
   The misses were `pkg-05  accept  reject   NO     failed: steps-rerunnable, control-run, template-asks`
   and `pkg-12  accept  reject   NO     failed: steps-rerunnable, control-run, template-asks`.
3. Canary run after revising steps-rerunnable (`--only pkg-05,pkg-12,pkg-18,pkg-06,pkg-19,pkg-20`
   plus `calib-04`): `agreement: 6/6 scored items`, and calib-04 still rejected.
4. Full run 2: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.
   Categories: `clear-accept 8/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
   This is the committed `eval-run.txt`.

**Package analysis**

**pkg-12** (prettier/prettier#19795, range formatting corrupts output when
the range starts on a comment). Gold: **accept**. Run 1: **reject**. Run 2:
**accept**.

The report is strong on the proof families. Its environment line is
`prettier 3.9.6 (npm, fresh npm install prettier@3.9.6), Node 22.23.1,
macOS 14.6 (arm64). The issue was filed against 3.8.4; both shapes still
reproduce on 3.9.6.` Both artifacts show the issue's corruption exactly:
shape A's output ends `};);` ("Re-parsing that output fails: SyntaxError:
Unexpected token"), and shape B's ends `};;`. So env-recorded,
behavior-matches and claims-backed all passed in both runs.

Run 1 rejected it on steps-rerunnable alone. The skill's evidence was:
`"repro.mjs content is never shown inline; 'containing the issue's two
prettier.format calls' is a summary — a stranger must reconstruct the
script from the issue body."` My first pass condition said "inputs shown
inline or linked publicly", so a step that pointed back to the issue's
own script read as a summary. But the report says it `ran the issue's
script verbatim in an empty directory` and names both calls' parameters
(`shape A: rangeStart 19, rangeEnd 32; shape B: rangeStart 18, rangeEnd
31`). Anyone reading the comment has the issue open above it, so they can
re-run it in one try. That's the rubric being too literal, not the package
being unfollowable. After the revision (inputs may be "taken verbatim from
the issue body (saying so is enough)"), run 2 graded it accept.

**Check rationale**

> | steps-rerunnable | The repro report's steps: the commands, inputs, and actions from starting state to trigger | A stranger with only public resources could re-run every step: exact commands or UI actions, and each input either shown inline, linked publicly, taken verbatim from the issue body (saying so is enough), or described precisely enough to write in one try ("a valid dependencies: list plus a category: section"). Fail if any step is a summary that hides the work ("set up the project", "run it the usual way"), or depends on private code, private data, or a config the report does not share | required |

The first version said "inputs shown inline or linked publicly". That
sounds strict in a good way, but it graded how the report was laid out,
not whether a stranger could actually follow it. It failed pkg-05 (whose
`env.yml` is described as `a valid dependencies: list plus a category:
section`) and pkg-12 (which `ran the issue's script verbatim`), and both
are followable in one try.

I thought about dropping the input requirement completely, and rejected
that. The failure this check exists for is the reproduction nobody else
can run: steps in a private monorepo, or a config file that's never
shared. So the revision names exactly two extra ways to supply an input
(verbatim from the issue, or a precise one-line description) and keeps
the fail cases explicit: "a summary that hides the work" and "private
code, private data, or a config the report does not share". The test I
use is "could a stranger produce the same input in one try", and the
evidence guide's Steps section says the same thing.

**Trade-offs**

Loosening steps-rerunnable gives up some strictness. A report that says
"ran the issue's script verbatim" but actually ran an edited copy now
passes this check, because the check takes "verbatim" at its word. I
accept that it will miss that case here, and rely on behavior-matches to
catch it instead: that check compares the report's input "character by
character with the issue's trigger" and reads the artifact against the
issue's symptom, so an edited input that changes the failure still fails
there.

The change flipped pkg-05 and pkg-12 from reject to accept (gold: accept).
Before the full run, I checked for flips in the other direction with a
canary run: `--only pkg-05,pkg-12,pkg-18,pkg-06,pkg-19,pkg-20` plus
`calib-04`. These were pkg-18 (the private-monorepo reproduction, the case
this check most needs to keep failing), pkg-06 and pkg-19 (the other
unfollowable-comms packages), pkg-20 (the one-package disclosure
category), and calib-04. All stayed reject (`agreement: 6/6 scored items`).
The confirming full run 2 then scored 20/20 with every category at full
count, so nothing else changed.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
