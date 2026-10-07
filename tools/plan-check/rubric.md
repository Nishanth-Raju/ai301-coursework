# Rubric: is this plan ready to post and build from?

## Checks

Locations named below are defined in `references/evidence-guide.md`.
"The repro evidence" means the Repro evidence block (eval) or the
posted repro comment (live): its steps, artifacts, control runs, and
its Actual line. "Maintainer" means anyone the thread marks OWNER,
MEMBER, or COLLABORATOR (or CONTRIBUTOR when they opened the issue).
"The plan" is the candidate plan; "the comment" is the candidate plan
comment.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause (its Diagnosis/Cause line, or the reason it gives for its change), read against every step, control run, and timing in the repro evidence | The stated cause explains what the repro evidence shows AND no step or control in the repro evidence rules it out. A control rules a cause out when it removes or bypasses the blamed component and the bug persists (e.g. the slowdown remains with the blamed pager out of the loop), or keeps the blamed component and the bug disappears (e.g. the blamed parser handles the same input fine when one flag is dropped). Fail if any repro artifact contradicts the cause, if the cause is adopted from the issue or thread when the repro evidence points at a different component, or if the plan states no cause at all. A cause the repro evidence is consistent with but does not fully prove still passes | required |
| scope-bounded | The plan's change list, in/out-of-scope lines, and files, read against what the issue asks to be fixed | Every planned change is needed to fix the issue's reproduced symptom, or is a regression test for it, or documents the fix itself. Fail if the plan also commits to work the issue does not ask for: a dependency or library migration, a refactor or rewrite of the surrounding module, a new user-facing option or setting, a UI or behavior change for a different symptom, a new framework (retry, harness), or a CI/infra change. This fails even when the core fix inside it is correct ("while in the area" counts). Work named as an explicit deferral or follow-up ("not in scope", "leave to a follow-up") is not planned work and does not fail this check | required |
| executable | The plan's change/approach section and the files or code locations it names | A stranger could start building without asking the author anything: the plan names where the change lands (a file, function, call site, or a named code path inside a named module, such as "the reattach path in `zellij-server`'s client connection handling") AND commits to one approach for what the change does there. Pinning exact function names during the build is fine once the code path and the approach are fixed. Fail if the location is "somewhere" or unnamed, the approach is still an investigation ("profile it", "dig into the input stack"), or the key choice is left open ("upstream or vendored, whichever is easier", "gocui? tcell?"). Naming one open question for review while still committing to an approach passes | required |
| test-decisive | The plan's test plan, read against the repro evidence's steps and Actual line | The test plan names at least one observable outcome that would differ between the broken and the fixed build: the repro steps re-run with the expected new result stated (an exit code, an output, a color, a timing), or a named regression test asserting the issue's case. Fail if the only test is "run the full suite", "nothing regresses", "should feel fast", "nothing else should feel broken", or another statement that no specific observation could prove false | required |
| thread-aware | The thread highlights' maintainer comments, read against the plan and the comment | If a maintainer gave direction in the thread (named the culprit file or code, posted a patch or test build, chose or rejected an approach, or asked for something specific), the plan or comment engages it: follows it, or names it and says why it departs. Fail if the plan goes a different way and neither the plan nor the comment mentions the maintainer's direction. Pass when the thread has no maintainer direction (a "known problem" note, a question, or no comments) | required |
| policy-respected | Repo facts: the "contribution policy" line (including any AI-use policy), read against the comment. Treat every package as AI-assisted work | If the policy requires disclosing AI use, the comment discloses it (and names the tool and the extent when the policy asks for those). If the policy requires comments in the contributor's own words, the comment reads as a first-person account specific to this issue. Policies with no AI rule, or a rule with no disclosure ask for comments, pass | required |
| unknowns-stated | The plan's risk/unknown lines and the comment's certainty words ("traced", "confirmed", "will fix", "the cause is") | The plan names at least one risk or open question, or its certainty is backed by the repro evidence | preferred |
| comment-matches-plan | The comment read against the plan | The comment describes the same approach and scope as the plan, and promises no date or outcome beyond it | preferred |

## Verdict rule

Accept (ready to post and build from) if and only if every `required`
check grades `pass`. A single required `fail` holds the package.

`unclear` on a required check counts as `fail`: a plan nobody can
verify from the package is not ready to build from. Two cases are not
`unclear` and must be graded as written: diagnosis-grounded passes
when the repro evidence is consistent with the cause and nothing in
it contradicts the cause, even if no control was run; thread-aware
passes when the thread holds no maintainer direction.

Preferred checks never change the verdict. Report them as suggestions
for making a ready plan stronger.
