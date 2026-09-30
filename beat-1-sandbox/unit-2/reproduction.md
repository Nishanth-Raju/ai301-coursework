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

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

**Reproduction comment**

[Link to the comment where you posted your reproduction. It must record the environment
(OS, relevant versions, code state), steps a stranger could follow, and what you observed.
**Then paste the text of that comment underneath the link** — the pasted text is what this
field is graded on, so copy across what you actually posted.]

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
