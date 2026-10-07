# Plan: issue #63, README scorer test fixture is too short for its own word-count assertion

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63

## Diagnosis

The scorer is correct; the test fixture is wrong. From my reproduction at
`2f4e82f` (Windows 11, Git Bash, Python 3.12.6, pytest 9.1.1):

- With `--runxfail`, `test_readme_with_all_quality_signals` fails at
  `assert data["word_count"] > 100` with `assert 51 > 100`, and the scorer
  logs `category=minimal ... word_count=51`.
- Control: the scorer on plain text at the boundaries returns
  `51 minimal`, `111 adequate`, `499 adequate`, `500 comprehensive`. So it
  counts words correctly and follows its thresholds in
  `agent/tools/readme_scorer.py` (`< 100` minimal, `< 500` adequate, else
  comprehensive).

The fixture is a 51-word README, but the test asserts both
`word_count > 100` and `word_count_category == "comprehensive"`. The second
assertion needs 500+ words, so a fixture padded just past 100 words would
still fail.

## Scope

One bounded change, in one file: `tests/unit/test_readme_scorer.py`.

In scope:
- Extend the README fixture in `test_readme_with_all_quality_signals` to at
  least 500 words of realistic README prose (more description, installation,
  usage, features, tech stack, and demo text), keeping every quality signal
  the test asserts on (installation, usage, badges, demo link, tech stack).
- Remove the `@pytest.mark.xfail(strict=True, reason="issue #63: ...")`
  marker, as `docs/CONTRIBUTING.md` requires once the test passes.

Not in scope:
- Any change to `agent/tools/readme_scorer.py`: its thresholds, regexes, or
  scoring weights. The control run shows the scorer behaves as written.
- Changing the test's assertions (for example, asserting `adequate`
  instead of `comprehensive`). The test's name and docstring say it is a
  README with all quality signals, so `comprehensive` is the intended
  expectation.
- Any other test in the file, and any lint or type cleanup.

## Approach

1. Replace the triple-quoted `readme` string in
   `test_readme_with_all_quality_signals` with a longer README of 500+
   words. Write it as real README sections rather than repeated filler
   words, so the fixture still reads like the README it describes.
2. Delete the `xfail` decorator on that test.
3. Leave all ten assertions unchanged.

## Test plan

- Re-run my reproduction steps on the branch:
  - `.venv/Scripts/pytest tests/unit/test_readme_scorer.py -q -rx` should
    report `23 passed` with no `xfailed` line (it currently reports
    `22 passed, 1 xfailed`).
  - The single test, `pytest "...::test_readme_with_all_quality_signals" -q`,
    should pass, and the scorer's log line should show
    `category=comprehensive` with `word_count` of 500 or more (it currently
    shows `category=minimal ... word_count=51`).
- `make test-unit`, `make lint`, and `make typecheck` stay green.

## Risks and unknowns

- Extra words could change other signals. For example, words like "demo",
  "install" or "stack" are already required, but new prose might push
  `overall_score` in a direction I don't expect. I'll check the logged
  score after the change against the `overall_score > 0.7` assertion.
- A maintainer might prefer the assertion changed instead of the fixture.
  I'm choosing the fixture because the test's intent is "all quality
  signals", but I'll switch if review asks for that.

## Deviations

Nothing changed; the plan held. The build touched only
`tests/unit/test_readme_scorer.py`, extended the fixture, removed the
`xfail` marker, and left all ten assertions and the scorer alone.

Two small notes, neither a change of approach:

- I built it in two commits (fixture first, then the marker), so the
  first commit on its own shows `XPASS(strict)` for this test. That is the
  failure CONTRIBUTING.md describes, and the second commit clears it.
- The risk I named about extra prose shifting `overall_score` did not
  happen in a bad direction: the scorer now logs
  `category=comprehensive score=1.0 word_count=565` (it was
  `score=0.8717...` at 51 words), well above the `> 0.7` assertion.
