# Evidence guide: where proof lives in a reproduction package

The skill uses this guide as its map. For every kind of proof a rubric
check names, it says WHERE to find it (in an eval bundle and in live
mode) and WHAT GOOD LOOKS LIKE there.

Package sections, in eval bundles: **Repo facts** (repo line, latest
release, "bug reports" template line, "contribution policy" line),
**Issue** (title, opener, body excerpt), **Thread highlights**,
**Candidate claim comment**, **Candidate repro report**. In live mode
the same roles are played by the repo's README/CONTRIBUTING/issue
templates, the issue body and thread on GitHub, and the student's draft
files (`claim.md`, `repro.md`).

## Environment

Where it lives:
- Eval: the repro report's opening lines or an "Environment" line/section;
  sometimes a version banner inside an output excerpt. The issue's target
  environment is in the Issue body ("Version:", "Operating system:",
  "Environment:") and the "bug reports" template line under Repo facts.
- Live: the draft `repro.md`; the issue body and its template fields on
  GitHub; the repo's setup docs for which runtime versions matter.

What good looks like:
- The OS and the version or code state (release number, commit SHA, or
  branch) of the software actually run are both stated.
- Any factor the issue or thread says changes the behavior (debug vs.
  release build, driver, shell, platform, install method) is named with
  the value used. If the issue's failure mode differs by that factor, an
  environment record without it is not sufficient.
- When the tested version or platform differs from the issue's, the
  report says so in words ("filed on 4.53.2; I tested 4.53.3").
- Not good: no environment at all, even under a strong artifact. A
  stranger cannot place the attempt.

## Steps

Where it lives:
- Eval: the repro report's command blocks (`$ ...` lines), numbered
  steps, and any input file shown inline (`cat input.x`).
- Live: the draft `repro.md`; inputs must be inline in the draft or at a
  public link, since readers of the thread see only the comment.

What good looks like:
- Starting state is obtainable by a stranger: a public release, a public
  commit, a fresh `git init`/temp directory, or an inline input file.
- Every step is an exact command or a named UI action. Short is fine;
  four exact lines beat twenty vague ones.
- Inputs do not have to be pasted again if the issue already shows them:
  "ran the issue's script verbatim" is followable, because the stranger
  has the issue open. A precise one-line description of a tiny input
  ("an env.yml with a valid dependencies: list plus a category: key")
  is also followable. What matters is whether a stranger could produce
  the same input in one try.
- Not good: steps that summarize ("set up the project", "reproduce the
  normal way"), or that depend on a private repo, private data, or a
  config file the report does not include.

## Behavior shown

Where it lives:
- Eval: the output excerpts, logs, exit codes, and screenshot
  descriptions inside the repro report. Read them against the Issue
  body's stated symptom and trigger, and against owner/maintainer notes
  in Thread highlights (which often narrow what the trigger really is).
- Live: the draft `repro.md` artifacts against the issue body and thread
  on GitHub.

What good looks like:
- Read the artifact first, then the narration. The artifact must show
  the same kind of failure as the issue (crash/panic vs. graceful error,
  same error class or message, same wrong output), produced by the
  issue's input or command.
- Compare the input character by character with the issue's trigger. A
  changed operator, a missing flag, a different range syntax, or an
  unbound variable produces an adjacent failure, not the reported one.
- An old version whose behavior is known to differ is only evidence
  about that version, unless the report says why it is still relevant.
- An artifact that only shows the software running (a version banner, a
  session list, a normal screen) shows nothing about the bug.
- Cannot-reproduce: good looks like the issue's trigger actually run,
  with the actual output shown, so a maintainer can see what happened
  instead.

## Honesty

Where it lives: every sentence in the claim comment and repro report
that asserts something ("reproduced", "confirmed", "verified",
"deterministic", "happens every time", "the cause is ..."), set next to
the artifacts in the same package.

What good looks like:
- Every assertion points at something shown: "reproduced" at an artifact
  showing the issue's behavior; "every time" at repeated runs shown or
  counted; a root cause at code, a trace, or an experiment shown.
  Hypotheses are labeled as hypotheses ("my guess is ...").
- A report whose confidence exceeds its artifacts fails honesty even when
  it is polished: "rigorous", "guaranteed", "I verified this race" with
  nothing behind them are the tells.
- An honest cannot-reproduce says what was tried, shows what happened,
  and names the differences from the reporter's environment that might
  matter. That is a complete, honest outcome.

## Comms

Where it lives:
- Eval: the Candidate claim comment read against the Issue; both
  comments read against the "contribution policy" line under Repo facts
  (AI-use rules, disclosure requirements, review expectations) and the
  "bug reports" template line.
- Live: the draft `claim.md` and `repro.md` against the repo's
  CONTRIBUTING.md, any AI policy file, the issue templates, and the
  scope's house rules.

What good looks like:
- The claim is specific to this issue: it names the symptom, trigger, or
  file, and states intent as investigation or a concrete next step
  ("I'll look at where X decides Y"). It promises no fix, PR, or date.
- Not good: +1 / me-too, "assign me please" boilerplate that fits any
  issue, demands ("fix this soon", "top priority"), or guaranteed
  timelines.
- AI policy: course work is AI-assisted, so treat every package that
  way. If the repo requires AI-use disclosure, a comment must disclose
  it (one plain sentence is enough). If the repo forbids AI-written
  comments, the comments must read as the author's own specific account.
  No AI rule, or a permissive rule without a disclosure requirement,
  asks nothing extra.
