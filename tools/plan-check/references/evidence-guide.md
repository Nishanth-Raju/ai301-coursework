# Evidence guide: where evidence lives in a plan package

The skill uses this guide as its map. For every kind of evidence a
rubric check names, it says WHERE to find it (in an eval bundle and in
live mode) and WHAT GOOD LOOKS LIKE there.

Package sections, in eval bundles: **Repo facts** (repo line, latest
release, "bug reports" template line, "contribution policy" line),
**Issue** (title, opener, body excerpt), **Thread highlights** (dated
comments, each with the author's association), **Repro evidence**
(environment, steps, artifacts, control runs, Expected/Actual),
**Candidate plan**, **Candidate plan comment**. In live mode the same
roles are played by the repo's README/CONTRIBUTING/AI policy files, the
issue body and thread on GitHub, the student's posted repro comment
(or the house repro pack quoted in the drafts), and the student's
draft `plan.md` and `comment.md`.

## Diagnosis and grounding

Where it lives:
- The cause: the plan's "Diagnosis" or "Cause" line, or failing that,
  the reason the plan gives for its change ("because X, change Y").
  The comment often restates it ("I traced this to ...").
- What it must explain: the Repro evidence block's steps, every
  "Control" line, any timing table, and the "Actual" line. In live
  mode, the posted repro comment's output blocks and controls.

What good looks like:
- Read the repro evidence before the plan. Write down, for each
  control, which component it removed or kept and whether the bug
  survived. Then hold the plan's cause against that list.
- A grounded cause is one every control agrees with. If a control
  takes the blamed component out and the bug stays (the 26 s seek
  with no pager in the loop), or keeps the blamed component and the
  bug goes away (the same tokens parsing fine without `-v`), the
  cause is ruled out, however confident the plan sounds.
- A cause taken from the issue body or a thread comment is fine when
  the repro evidence agrees with it. "As identified in this thread"
  is not grounding by itself; the evidence decides.
- A cause the evidence is consistent with but does not fully prove
  is still grounded. Absence of a control is not a contradiction.

## Scope

Where it lives: the plan's change list (numbered "Changes" or
"Approach" steps), its "In scope" / "Not in scope" lines, its "Files
and areas" line, and the comment's description of what will be done.
Read them against the issue title and body: what symptom was
reported?

What good looks like:
- One bounded change: every listed step either fixes the reproduced
  symptom, tests it, or documents that fix. A short plan touching one
  file is the normal shape.
- Explicit deferrals ("not in scope: option 1, a bigger rework"; "the
  Windows variant goes to a follow-up") are a good sign, not scope.
- Scope creep looks like a correct fix plus extra fronts: a migration
  to a new library, a new setting, a rewrite or "unification" of the
  surrounding module, a test-harness or CI change, a fix to a
  different symptom "while in the area". Count the fronts: more than
  the fix and its tests means the plan is unbounded, even when the
  core fix is right.

## Executability

Where it lives: the plan's "Change", "Approach", or "Files" section;
named paths (`src/output.rs`), functions (`Terminal.print`), call
sites, or branches.

What good looks like:
- A location a stranger can open (a file, a function, a callback) and
  a single chosen approach for it. Terse is fine: "add the commits
  context to the post-push refresh scope in `sync_controller.go`" is
  a complete plan.
- Not good: "investigate", "profile and optimize", "add recover()
  somewhere", "fix upstream or vendored, whichever is easier",
  undecided layer lists ("gocui? tcell? not sure"). Those defer every
  real decision to build time.
- A named code path inside a named module ("the reattach handshake in
  `zellij-server`'s client connection handling") is a location, even
  when the plan says it will pin the exact function names while
  building. What decides is whether the approach is fixed: "drain the
  pending OSC responses there before input is wired" is; "find out
  what's slow" is not.
- One named open question for review is fine when the plan still
  commits to an approach to start with.

## Test plan

Where it lives: the plan's "Test" or "Test plan" section, read against
the repro evidence's steps and its "Expected" line.

What good looks like:
- The test re-runs the repro (or a regression test encodes it) and
  states what will now be observed: "at step 3 the color must flip",
  "exit 0", "each spelling prints `C:\t\fixture\...`", "sync succeeds
  10 of 10". If the broken build would fail that observation and the
  fixed build would pass it, the test plan is decisive.
- Extra checks (full suite, adjacent views) are welcome on top.
- Not good: a test plan that is only "run the full test suite and
  make sure nothing regresses", "should feel fast", or "nothing else
  feels broken". The broken build already passes those.

## Honesty

Where it lives: the plan's "Risk", "Unknowns", or "Open question"
lines; certainty words in the plan and comment ("confirmed",
"traced", "the cause is", "will fix"); and, after a build, the plan's
`## Deviations` section.

What good looks like:
- Unknowns are named as unknowns ("I have not yet measured the
  per-print cost"), and certainty is backed by the repro evidence.
- A mid-build change of approach is recorded under `## Deviations` in
  the plan with the reason, before any follow-up comment.

## Comms

Where it lives:
- Thread direction: the Thread highlights lines from OWNER, MEMBER, or
  COLLABORATOR authors (and the issue opener when they are a
  CONTRIBUTOR maintainer). Live: the issue thread on GitHub.
- Repo conventions: the "contribution policy" line under Repo facts
  (AI-use rules, disclosure asks, own-words asks). Live: CONTRIBUTING.md
  and any AI_POLICY file.
- Both are read against the Candidate plan comment (and the plan).

What good looks like:
- Maintainer direction is anything that steers the fix: a named culprit
  file or line, a posted patch or test binary, an approach chosen or
  rejected ("too expensive for this hot path"), an explicit ask. A
  thread-aware comment follows that direction or names it and says why
  it departs. A comment that goes another way without mentioning it
  ignores the thread.
- Not direction: a "known problem" note, a clarifying question, a
  non-maintainer's claimed root cause (that is diagnosis evidence, not
  direction).
- AI policy: treat every package as AI-assisted. If the policy requires
  disclosure, one plain sentence naming the tool (and the extent, if
  asked) must be in the comment. If the policy wants comments in the
  contributor's own words, the comment must read as their own specific
  account. No AI rule asks nothing extra.
- Tone issues ("PR shortly", "happy to split") are voice notes, not
  convention walls; the rubric does not fail on them.
