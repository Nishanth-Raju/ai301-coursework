# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````text
I have all the evidence needed. Let me now compile the full analysis.

---

## Summary

**Repo facts (all issues share these):**
- `archived`: false; last push 2026-09-16 (13 days ago); 5 recent commits all by Andrew Burke (Aburke225, human, COLLABORATOR); no releases; CONTRIBUTING.md exists but mentions no AI policy.

---

### Issue #63 — "README scorer test fixture is too short for its own word-count assertion"

- **commits-recent**: PASS — most recent commit 2026-09-16 by Andrew Burke (human, COLLABORATOR), 13 days before today.
- **repo-in-use**: PASS — not archived; last push 2026-09-16, within 90 days.
- **scope-bounded**: PASS — single bounded task: extend the fixture or fix the assertion; no design debate; opened by COLLABORATOR; no closed/unmerged PRs.
- **not-claimed**: PASS — no assignees, no open PR; two student "I'll investigate" comments (NONE association); Path Review house rule: student claim comments do not block.
- **ai-policy-allows**: PASS — CONTRIBUTING.md present, no AI policy mentioned; silence passes.
- **maintainer-responsive** (preferred): FAIL — 0 of 4 sampled issue threads (61, 63, 47, 36) show any owner/member/collaborator comment after the issue was opened.
- **newcomer-label** (preferred): PASS — labeled "good first issue".
- **maintainer-engaged** (preferred): PASS — opened by COLLABORATOR Aburke225.
- **clear-done-criteria** (preferred): PASS — body gives exact reproducing command and failing assertion (`assert 51 > 100`).

**Verdict: ACCEPT** (all 5 required checks pass; 3 preferred passes).

---

### Issue #47 — "API docs don't include example curl commands"

- **commits-recent**: PASS — same repo evidence.
- **repo-in-use**: PASS — same repo evidence.
- **scope-bounded**: PASS — single bounded docs task (add curl examples to `docs/API.md`); no design debate; opened by COLLABORATOR; no closed/unmerged PRs.
- **not-claimed**: PASS — no assignees, no open PR; jeff-sp's claim and repro comment (NONE association) do not block per house rule.
- **ai-policy-allows**: PASS — silence.
- **maintainer-responsive** (preferred): FAIL — same sample, 0 maintainer responses observed.
- **newcomer-label** (preferred): PASS — labeled "good first issue".
- **maintainer-engaged** (preferred): PASS — opened by COLLABORATOR Aburke225.
- **clear-done-criteria** (preferred): PASS — "add curl examples for each endpoint in docs/API.md"; jeff-sp's repro even lists all 9 worked examples.

**Verdict: ACCEPT** (all 5 required checks pass; 3 preferred passes).

---

### Issue #36 — "POST /reviews endpoint has no test for when the profile has no ingested documents"

- **commits-recent**: PASS — same repo evidence.
- **repo-in-use**: PASS — same repo evidence.
- **scope-bounded**: **FAIL** — trihiennguye-ux's repro (2026-09-26) shows the endpoint succeeds (`status=complete, score=0.81`) for a zero-ingested-document profile instead of erroring. They explicitly asked "@Aburke225, which behavior should the new test assert?" (option 1: upfront 4xx, or option 2: mark review `failed` in background). No owner/member/collaborator has answered. The approach is still being debated with no maintainer resolution.
- **not-claimed**: PASS — no assignees, no open PR; student claims per house rule don't block.
- **ai-policy-allows**: PASS — silence.

**Verdict: REJECT** — `scope-bounded` fails (design debate unresolved, no maintainer has stated the chosen approach).

---

### Ranking of accepted issues

Fit profile is blank (not filled in). Both #63 and #47 have identical preferred-check scores (3 passes each). Defaulting to rank by work clarity: **#63 ranks first** — it touches exactly one file (the test fixture or its assertion), the failing line is quoted verbatim, and no environment setup is needed to verify it. **#47 ranks second** — also clear, but requires spinning up the full Docker stack to run and verify the endpoint curl examples.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/63",
    "checks": [
      {"name": "commits-recent", "grade": "pass", "evidence": "Most recent commit 2026-09-16 by Andrew Burke (human, Aburke225), 13 days before today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, within 90 days; no releases (passes via last-push branch)"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single bounded task: extend ~51-word fixture to >100 words or fix the assertion; no design debate; no unmerged PRs"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees, no open PR; two NONE-association 'I'll investigate' comments do not block per Path Review house rule"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI policy; silence passes by rubric construction"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "0 of 4 sampled issues (61, 63, 47, 36) show any OWNER/MEMBER/COLLABORATOR comment in the thread"},
      {"name": "newcomer-label", "grade": "pass", "evidence": "Issue labeled 'good first issue'"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opened by Aburke225 (COLLABORATOR)"},
      {"name": "clear-done-criteria", "grade": "pass", "evidence": "Body gives exact reproducing command and failing assertion: 'assert 51 > 100'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/47",
    "checks": [
      {"name": "commits-recent", "grade": "pass", "evidence": "Most recent commit 2026-09-16 by Andrew Burke (human, Aburke225), 13 days before today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, within 90 days; no releases (passes via last-push branch)"},
      {"name": "scope-bounded", "grade": "pass", "evidence": "Single bounded docs task: add curl examples to docs/API.md; no design debate; no unmerged PRs; opened by COLLABORATOR"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees, no open PR; jeff-sp's claim and repro (NONE association) do not block per Path Review house rule"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI policy; silence passes by rubric construction"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "0 of 4 sampled issues (61, 63, 47, 36) show any OWNER/MEMBER/COLLABORATOR comment in the thread"},
      {"name": "newcomer-label", "grade": "pass", "evidence": "Issue labeled 'good first issue'"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opened by Aburke225 (COLLABORATOR)"},
      {"name": "clear-done-criteria", "grade": "pass", "evidence": "Task is explicit: add curl example per endpoint in docs/API.md; jeff-sp's repro lists all 9 worked commands"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/36",
    "checks": [
      {"name": "commits-recent", "grade": "pass", "evidence": "Most recent commit 2026-09-16 by Andrew Burke (human, Aburke225), 13 days before today"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=false; last push 2026-09-16, within 90 days; no releases (passes via last-push branch)"},
      {"name": "scope-bounded", "grade": "fail", "evidence": "trihiennguye-ux's repro (2026-09-26) shows the endpoint completes successfully (score=0.81) with zero ingested docs; they asked the maintainer which behavior the test should assert (4xx upfront vs. background failure); no OWNER/MEMBER/COLLABORATOR has answered"},
      {"name": "not-claimed", "grade": "pass", "evidence": "No assignees, no open PR; multiple NONE-association claim comments do not block per Path Review house rule"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "CONTRIBUTING.md has no AI policy; silence passes"},
      {"name": "maintainer-responsive", "grade": "fail", "evidence": "0 of 4 sampled issues show any OWNER/MEMBER/COLLABORATOR comment in thread"},
      {"name": "newcomer-label", "grade": "pass", "evidence": "Issue labeled 'good first issue'"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opened by Aburke225 (COLLABORATOR)"},
      {"name": "clear-done-criteria", "grade": "fail", "evidence": "Issue body says 'appropriate error' but repro shows no error occurs; what behavior to assert is actively disputed and unanswered by maintainer"}
    ],
    "verdict": "reject"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Run 1: `agreement: 11/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`.
   Categories: `claimed 4/4  clear-accept 0/8  dead-repo 3/3  policy 1/1  scope 3/4`.
   All 8 gold-accept issues were rejected, each with `failed: maintainer-responsive`,
   and issue-20 was `graded accept` against a gold reject.
2. Run 2: `agreement: 20/20 scored items  (bar: 18/20: PASS)`.
   Categories: `claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.
   Changes between runs: `maintainer-responsive` moved from `required` to
   `preferred`, and `scope-bounded` gained condition (f) for unaccepted feature requests.
   This is the run committed as `eval-run.txt`.

(An earlier attempt at run 1 crashed before grading anything with
`UnicodeEncodeError: 'charmap' codec can't encode characters`. That was a
Windows console encoding issue, fixed by setting `PYTHONUTF8=1`, not a rubric
result, so it is not counted.)

**Issue analysis**

**issue-20** (excalidraw/excalidraw#11811, "Add company logo shape to the toolbar").
Rubric decision: **reject**. Gold label: **reject**. In run 1 the rubric
**accepted** it, the only false accept.

The issue passes every repo-level check. The bundle shows
`2026-08-04 by dwelle: feat(packages/excalidraw): ViewportStatusFrame ...`
(human commits the day before capture), `latest release: v0.18.1 (2026-04-21)`,
`contribution policy (CONTRIBUTING.md): no statement on AI or contribution tooling`,
and `this issue: assignees: none; linked PRs: none`. So commits-recent,
repo-in-use, ai-policy-allows and not-claimed all pass, and in run 1 nothing
stopped it.

The problem is the issue itself. It was `opened by cursor[bot] (NONE) on
2026-08-02, state open, labels: none`, it asks for a new feature, it says
`Logo asset TBD.`, and the thread is `(no comments)`. No maintainer has
agreed this feature should exist, so a newcomer's PR could be closed on
product grounds however good the code is. Run 1's scope-bounded check only
looked for umbrella issues, design debates, core-internals warnings, usage
questions and abandoned PRs, and none of those applies.

Condition (f) added to scope-bounded catches exactly this. On re-grading, the
skill's evidence line was: `"Condition (f): new feature request, opened by
cursor[bot] (NONE), zero owner/member/collaborator comments, no labels — all
three sub-conditions met."` With scope-bounded failing, the verdict rule
("A single `fail` on any required check rejects the issue") produces
reject.

**Check rationale**

> | maintainer-responsive | Repo facts: "maintainer first-response sample" (5 recently updated issues) | At least 2 of the sampled issues got a first owner/member/collaborator comment within 14 days. Entries marked "no maintainer comment in thread" count as misses | preferred |

This check started as `required`, with the idea that a maintainer who never
answers issues will never review your PR. Run 1 showed the sample can't
carry that weight. Every gold-accept issue failed it. conda's sample (issues
01, 09, 16) had one maintainer reply, at `32.9 days`, and four entries of
`no maintainer comment in thread`. lq-ai's sample (issue-14) had one issue
with no maintainer comment at all. Yet those repos had human commits the day
before capture. Five recently updated issues are mostly brand new ones that
nobody has had time to answer yet, so the sample measures timing noise more
than maintainer life.

Maintainer life is already gated by `commits-recent` (a human commit within
90 days), which is what actually fails the dead repos: rupa/z's newest commit
is `2023-12-09` and autojump's is `2025-02-10`. So I kept the 14-day
threshold, because a fast-responding maintainer really is a better first
experience, but moved the check to `preferred`. Now it ranks accepted issues
and never rejects one. The verdict rule records why: "a 5-issue sample is too
noisy to reject on."

**Trade-offs**

Making `maintainer-responsive` preferred gives up the ability to reject a
repo where people commit but never talk to contributors. That's a real way
first PRs die: code lands from an inner circle while outside PRs sit unreviewed.
My rubric now accepts that case as long as someone human pushed in the last 90 days.

Issues whose result it changes: issue-01, 04, 06, 09, 11, 14, 16 and 19 all
went from reject in run 1 to accept in run 2 (gold: accept). No gold-reject
issue flipped to accept because of it. The only other result that changed,
issue-20, flipped the other way (accept → reject), because of the new
scope-bounded condition (f), and I re-ran it alone with `--only issue-20` to
confirm (`agreement: 1/1 scored items`).

A case I accept it will miss is my own target repo. In live mode, Path
Review's sample showed `0 of 4 sampled issues (61, 63, 47, 36) show any
OWNER/MEMBER/COLLABORATOR comment in the thread`. Under the run-1 rubric that
rejected every candidate. Under the current one it's a failed preferred
check, and I rely on the course's own statement that credit attaches to
opening the PR rather than to a quick review.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
