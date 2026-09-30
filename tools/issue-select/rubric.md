# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

All day counts are measured against the bundle's capture date in eval
mode, and against today in live mode.

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| commits-recent | Repo facts: "last 5 default-branch commits" (dates and authors) | At least one of the listed commits is dated within 90 days of the capture date AND is human activity: authored by a non-bot account, or a bot commit that merges a human's pull request | required |
| repo-in-use | Repo facts: the repo line ("archived:"), "latest release", "last push to any branch" | archived is "no" AND (the latest release is dated within 365 days of the capture date OR the last push to any branch is within 90 days of the capture date). archived "yes" is always fail | required |
| scope-bounded | Issue body plus the Comments section (author_association on each comment), plus "linked PRs" under Repo facts | Fail if ANY of these: (a) the issue is explicitly an umbrella, tracking, or meta issue listing sub-items to split out; (b) the thread shows the approach is still being debated and no owner/member/collaborator has stated the chosen approach; (c) an owner/member/collaborator says the fix requires changes to core internals or a large refactor; (d) it is a usage or support question, not a request for a code or docs change; (e) 2 or more closed, unmerged PRs have already attempted it (linked or mentioned in the thread); (f) it asks for a new feature or behavior change and nothing shows a maintainer has accepted it: not opened by an owner/member/collaborator, no owner/member/collaborator comment in the thread, AND no labels at all. Otherwise pass. A short body or missing repro steps is NOT a fail by itself | required |
| not-claimed | Repo facts: "this issue: assignees" and "linked PRs"; the Comments section (PR mentions and claim comments with their dates) | Fail if ANY of these: (a) the issue has an assignee, unless the thread shows the assignee withdrew or a maintainer unassigned them; (b) an OPEN PR addresses it, linked or mentioned in the thread (if the sidebar and thread disagree, believe the thread); (c) a comment like "I'll take this" or "working on this" is dated within 30 days before the capture date and nobody said they dropped it. Older claims with no PR are stale and do not block. Otherwise pass | required |
| ai-policy-allows | Repo facts: the "contribution policy" line (CONTRIBUTING.md, AI policy files, templates) | Fail only if the policy bans AI-assisted contributions outright. Conditions pass: disclosure, the contributor must understand and test every change, human review, or a ban limited to "fully AI-generated" work while assistive use is allowed. No policy line, or a policy that says nothing about AI, passes (silence is not a restriction, so this check never grades unclear for missing policy) | required |
| maintainer-responsive | Repo facts: "maintainer first-response sample" (5 recently updated issues) | At least 2 of the sampled issues got a first owner/member/collaborator comment within 14 days. Entries marked "no maintainer comment in thread" count as misses | preferred |
| newcomer-label | Issue header: the "labels:" list | Labels include "good first issue", "good-first-issue", "beginner", "easy", "starter", or "help wanted" | preferred |
| maintainer-engaged | Issue header ("opened by ... (ASSOCIATION)") and the Comments section's author_association values | The issue was opened by an OWNER/MEMBER/COLLABORATOR, OR at least one comment in the thread is from an OWNER/MEMBER/COLLABORATOR | preferred |
| clear-done-criteria | Issue body | The body has concrete reproduction steps, an expected-vs-actual description, or an acceptance-criteria list saying what "done" looks like | preferred |

## Verdict rule

Accept if and only if every `required` check grades `pass`. A single
`fail` on any required check rejects the issue.

`unclear` on a required check counts as `fail`: a first issue whose
maintainer life, usage, scope, or claim status cannot be verified is not
one to take. Maintainer life is gated by commits-recent (human commits in
the last 90 days); the response-latency sample is only a ranking signal,
because a 5-issue sample is too noisy to reject on. (ai-policy-allows is
the exception by construction: a missing policy passes rather than
grading unclear.)

Preferred checks never change the verdict. Report their grades; among
accepted issues, more preferred passes ranks an issue higher. An
`unclear` preferred check simply earns no ranking credit.
