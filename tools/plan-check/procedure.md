# Procedure: how this skill grades a plan package

Follow these steps in order. Every check and location named here is
defined in `rubric.md` and `references/evidence-guide.md`.

## Read order

1. **Repo facts first.** Note the "contribution policy" line word for
   word: does it require AI disclosure for comments, require comments
   in the contributor's own words, or say nothing about AI?
2. **Issue second.** Note the reported symptom (what goes wrong) and
   the trigger (what input or action causes it), in one line each.
3. **Thread highlights third.** List every maintainer comment (OWNER,
   MEMBER, COLLABORATOR, or a CONTRIBUTOR who opened the issue). For
   each, note whether it is direction (names a culprit file or line,
   posts a patch or test build, chooses or rejects an approach, makes
   a specific ask) or not (a question, a "known problem" note).
   Non-maintainer root-cause claims go in a separate list: they are
   claims to test against the repro evidence, not direction.
4. **Repro evidence fourth, before the plan.** For each step and
   control, note which component it involved or removed and whether
   the bug appeared. Copy the Actual line. This list is what the
   diagnosis gets held against, so build it before reading the plan's
   own story.
5. **Candidate plan fifth.** Note its stated cause, every planned
   change (one line each, numbered), its deferrals, the files or
   locations it names, its test plan, and its risks.
6. **Candidate plan comment last.** Note what approach it describes,
   which maintainer comments or PRs it mentions, and whether it
   discloses AI use.

Live mode only: before step 1, read `scope.md` and stop if the issue
is outside the scoped repo or the repo line is a placeholder. Gather
steps 1-3 from GitHub (CONTRIBUTING.md and any AI policy file; the
issue body; the issue thread), step 4 from the student's posted repro
comment (or the house repro pack quoted in the drafts), and steps 5-6
from the draft `plan.md` and comment file. Then read `voice-guide.md`
and note any rule the draft comment breaks.

## Evidence gathering

Each check reads only the notes listed for it. If a note is missing,
go back to that package section and take it; do not guess.

- **diagnosis-grounded**: the plan's stated cause (step 5) and the
  per-step/per-control list (step 4).
- **scope-bounded**: the numbered list of planned changes and the
  deferrals (step 5), and the issue's symptom (step 2).
- **executable**: the files or locations and the approach wording
  (step 5).
- **test-decisive**: the test plan (step 5) and the repro's Actual and
  Expected lines (step 4).
- **thread-aware**: the maintainer-direction list (step 3), the plan's
  approach (step 5), and what the comment mentions (step 6).
- **policy-respected**: the policy line (step 1) and the comment's
  disclosure or voice (step 6).
- **unknowns-stated** and **comment-matches-plan** (preferred): the
  plan's risks (step 5) and the comment (step 6).

## Check execution

Run the checks in this order: diagnosis-grounded, scope-bounded,
executable, test-decisive, thread-aware, policy-respected, then the
two preferred checks. Grade every check, even after a required check
fails, so the output shows the full picture.

For each check:

1. Apply the rubric's pass condition to the gathered notes, literally.
2. **diagnosis-grounded**: for each control in the step-4 list, ask
   "if the plan's cause were true, would this control have come out
   the way it did?" One "no" is a fail; quote that control. If every
   answer is "yes" or there are no controls, and the cause explains
   the Actual line, pass.
3. **scope-bounded**: for each numbered planned change, label it
   fix / test / docs-of-fix / extra. Any "extra" is a fail; quote it.
   Deferrals are not planned changes.
4. **executable**: pass only when there is both a named location and
   one chosen approach. A named code path in a named module counts as
   a location; "exact functions pinned during the build" does not
   fail it when the approach is fixed. Ask "what would a stranger do
   first?": if the plan answers that with a place and an action, pass.
   Quote the location, or the vague phrase that fails it.
5. **test-decisive**: ask "would the broken build fail this test?" If
   at least one stated observation would, pass. Quote it.
6. **thread-aware**: if the direction list is empty, pass. Otherwise,
   for each direction item, check whether the plan follows it or the
   plan/comment names it. Any direction item that the plan departs
   from without naming it is a fail.
7. **policy-respected**: no AI rule, pass. A disclosure requirement
   with no disclosure sentence in the comment is a fail.
8. Grade `unclear` only when the package is truly silent on what a
   check needs. A plan with no test section at all is a fail, not
   unclear. Write in the evidence line what was missing.

Each evidence line quotes the fact that decided the grade, from the
package, in one line.

## Verdict assembly

1. Take the six required checks. `unclear` counts as `fail`.
2. If all six pass, the verdict is `accept`. If any fails, `reject`.
3. Preferred checks never change the verdict.
4. In the readable summary, name the deciding check(s) for a reject
   and quote their evidence lines. For an accept, say "all required
   checks pass".
5. Emit the JSON block exactly as SKILL.md specifies, with every check
   (required and preferred) listed, and nothing after it.
