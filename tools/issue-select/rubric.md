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

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-not-archived | The `archived:` field on the repo line of Repo facts (live: the "archived" banner on the repo page) | `archived: no`. `archived: yes` fails, no matter what else the issue shows. | required |
| maintainer-alive | "last push to any branch" and the dates on the "last 5 default-branch commits" in Repo facts, measured against the capture date (live: today) | At least one default-branch commit is dated within 90 days of the capture date AND the last push to any branch is within 180 days. A missing release does not fail this check; commits carry liveness. | required |
| unclaimed | "this issue: assignees" and "linked PRs" in Repo facts, plus every comment in the thread that announces work ("I'll take this", "working on it", "I opened a PR", a PR link) | All three hold: (a) assignees is `none`; (b) no linked or thread-mentioned PR is in state `open` (closed or merged PRs do not block, they are history); (c) no non-maintainer comment claiming the work is dated within 60 days of the capture date. A claim older than 60 days with no open PR is stale and does not block. | required |
| bounded-scope | The issue title and body, the labels, the opener's association, and maintainer (OWNER/MEMBER/COLLABORATOR) comments in the thread | The issue asks for work that fits one PR on one problem. A maintainer-filed issue passes by default, even if terse or listing several related items, causes, or files for that one problem. An issue whose body is a written spec naming the specific files or pages to add/update for one feature or doc topic also passes, however long the spec: detail is a sign of scope, not of size. Fails only if ANY of: (1) the title, body, or labels describe it as an umbrella, tracking, meta, or "megaissue", or it asks for changes across "the codebase" meant to land as many separate PRs; (2) the thread holds unresolved design debate (competing designs) with no maintainer settling it, or 2+ closed unmerged PRs show earlier attempts failed; (3) a non-maintainer opened a feature wish with no spec, where a product/design decision (e.g. an asset or behavior marked "TBD") is still open and no maintainer has weighed in; (4) it is a usage/support question. | required |
| ai-policy-allows | The "contribution policy" line in Repo facts (live: CONTRIBUTING.md, linked contributor docs, AI_POLICY.md) | Passes unless the policy outright bans AI-generated or AI-assisted code or documentation (e.g. "we do not accept AI-generated code"). Conditions (disclose, review, understand, test, human review) pass. "No stated policy" passes. | required |
| maintainer-engaged | Maintainer-badged comments in this thread, the issue opener's association, and the "maintainer first-response sample" | A maintainer opened the issue, commented in it, or the response sample shows at least one maintainer reply within 30 days. | preferred |
| newcomer-label | The issue's labels | Has a `good first issue` / `help wanted` / similar newcomer label. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept only if every required check (repo-not-archived, maintainer-alive,
unclaimed, bounded-scope, ai-policy-allows) grades `pass`. Any required
`fail` rejects. An `unclear` on a required check counts as `fail`, except
for ai-policy-allows, where an absent or silent policy is a `pass` (silence
is not a ban). Preferred checks (maintainer-engaged, newcomer-label) never
change the verdict; they only rank accepted issues. A newcomer label never
rescues a required fail.
