# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

Locations named below are defined in `references/evidence-guide.md`.
"The issue's behavior" always means the symptom as the issue context
describes it (error text, exit code, crash vs. graceful error, wrong
output), produced by the issue's trigger (its input, command, or
action).

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record, read against the issue's stated environment and any environment factor the issue or thread says changes the behavior | The report states (1) the OS and (2) the version or code state (release, commit, branch) of the software it ran, AND (3) every factor the issue says changes the failure mode (build profile, driver, shell, platform, install method). If the tested version or platform differs from the issue's, the report says so. Fail if the report has no environment record, or omits a factor the issue says matters | required |
| steps-rerunnable | The repro report's steps: the commands, inputs, and actions from starting state to trigger | A stranger with only public resources could re-run every step: exact commands or UI actions, and each input either shown inline, linked publicly, taken verbatim from the issue body (saying so is enough), or described precisely enough to write in one try ("a valid dependencies: list plus a category: section"). Fail if any step is a summary that hides the work ("set up the project", "run it the usual way"), or depends on private code, private data, or a config the report does not share | required |
| behavior-matches | The repro report's artifacts (output excerpts, logs, exit codes, screenshot descriptions) read against the issue's behavior, and the report's input read against the issue's trigger | EITHER the artifact shows the issue's behavior (same kind of failure and same symptom text or effect) produced by the issue's trigger or an equivalent the report justifies, OR the report is a cannot-reproduce whose artifact shows the issue's trigger actually run and what came out instead. Fail if the artifact shows a different failure (e.g. a syntax or argument-validation error instead of the reported crash), comes from a modified input or an old version that changes the behavior without saying so, or shows only that the software runs | required |
| claims-backed | Every assertion in the claim comment and repro report ("reproduced", "confirmed", "verified", "deterministic", a stated root cause) matched against the artifacts the package actually shows | Each assertion of reproduction, certainty, frequency, or cause is backed by an artifact in the package, or is explicitly labeled a guess or hypothesis. An honest cannot-reproduce passes when it says what was tried and what differed. Fail if any assertion outruns the artifacts: a cause asserted with no evidence, "reproduced" narrated over an artifact that shows something else, or certainty ("every time", "on all my machines", "guaranteed") with nothing shown | required |
| claim-specific | The claim comment, read against the issue | The claim names something only this issue has (its symptom, trigger, file, or a specific next step) AND states its intent as investigation or a concrete next step. Fail if it is a +1 or me-too, is boilerplate that would fit any issue, demands a fix or priority, or promises a fix, a PR, or a date ("fix in 2 days", "PR shortly" with nothing behind it) | required |
| policy-respected | Repo facts: the "contribution policy" line (including any AI-use policy), read against both comments. Treat every package as AI-assisted work | If the policy requires disclosing AI use, at least one of the comments discloses it. If the policy forbids AI-generated comments, the comments read as the author's own first-person account (specific to their run, not generated boilerplate). Policies with no AI rule, or a permissive rule with no disclosure requirement, pass | required |
| control-run | The repro report's artifacts | The report includes a control: the same steps with the trigger removed or changed, showing the behavior disappears | preferred |
| template-asks | Repo facts: the "bug reports" template line, read against the repro report | The report supplies every item the repo's bug-report template asks for, or says why one is missing | preferred |

## Verdict rule

Accept (ready to post) if and only if every `required` check grades
`pass`. A single required `fail` holds the package.

`unclear` on a required check counts as `fail`: proof a stranger cannot
verify is not ready to post. The one exception is the claim-only live
draft that SKILL.md describes: checks marked "not yet applicable:
claim-only draft" are left out of the verdict, and the verdict then
covers only claim-specific and policy-respected.

A cannot-reproduce is not a failure of the package. It passes
behavior-matches and claims-backed when its artifacts show the real
attempt and it says what differed.

Preferred checks never change the verdict. Report them as suggestions
for making a ready package stronger.
