# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The diff's changed files and hunks, read against the plan's scope/boundary/files and its deviation notes; then the description's fidelity claims ("exactly as planned", "no changes beyond", "docs now document X") read against the diff | Every changed hunk implements something the plan (or a recorded deviation note) calls for, AND everything the plan promises is in the diff or is explicitly deferred in a deviation note/description, AND no description claim contradicts the diff. Fails on any unplanned change (new option/flag/setting, renaming pass, rewrite of another function, file the plan never touches) not covered by a deviation note, on any promised deliverable missing without a note, or on a description claiming something the diff does not contain. An honestly disclosed shortfall or deferral passes. | required |
| test-decisive | The test-evidence section, read against the plan's test plan and the repro's steps | The evidence re-runs the plan's repro (every failure mode the test plan names) on the changed code path and shows before AND after output, with the after matching the plan's expected result. Fails if the evidence is only "tested locally", "works on my machine", "tests pass" with no named output, if it exercises only a control or unchanged path instead of the failing one, or if a failure mode the test plan names is not re-run. | required |
| diff-clean | The unified diff and the commit list | The diff contains only the fix, its tests, and changes the plan calls for. Fails on any debris: debug prints/logging left in, commented-out code or earlier attempts, dead/unused functions, TODO noise, formatting or re-indent churn on untouched lines, import reshuffles, or unrelated hunks, even if the fix itself is correct. | required |
| standards-met | The repo-facts block's PR-template asks, contributing asks, and AI policy, read against the description and diff; plus maintainer direction in the thread highlights. Treat every package as AI-assisted work. | Each section or checklist item the stated PR template/contributing guide requires is present with real content (e.g. "closes #N", a changelog/whatsnew entry if required, checklist items). If the stated policy requires disclosing AI usage, the description discloses the tool and extent. A silent policy requires no disclosure. | required |
| repo-checks-run | The test-evidence section | The repo's own test command or suite outcome is visible (named command plus result). | preferred |

## Verdict rule

Accept (ready to submit) only if every required check grades `pass`:
plan-fidelity, test-decisive, diff-clean, and standards-met. Any
required `fail` rejects (hold). An `unclear` on a required check counts
as `fail`: a PR we cannot verify from the package is not ready. The
preferred check (repo-checks-run) never changes the verdict. A PR that
honestly discloses a shortfall or deferral (in the plan's deviation
note or the description) is graded on what it claims, not held for
doing less than everything.
