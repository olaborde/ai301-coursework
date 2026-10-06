# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the repo-facts block (live: PR template, CONTRIBUTING.md, AI
   policy). Write down every required PR-template section/checklist
   item and the AI-disclosure rule.
2. Read the issue and thread highlights. Write down any explicit
   maintainer direction.
3. Read the plan context BEFORE the PR, so the PR's description cannot
   frame it. Write down: the files and functions the plan names, what
   it says is in and out of scope, every deliverable it promises (code,
   docs, tests, warnings), its test plan (each failure mode and the
   expected-after), and any deviation/deferral notes.
4. Read the diff next, before the description. List every changed file
   and every hunk with a one-line summary of what it does.
5. Read the commits.
6. Read the test evidence.
7. Read the title and description last, and list every claim they make
   about fidelity, testing, and scope.
The plan comes before the diff, and the diff before the description,
because the core checks compare them side by side; reading the
description first lets its claims replace what the diff shows.

## Evidence gathering

1. plan-fidelity: make a two-column table. Left: each diff hunk. Right:
   the plan item or deviation note that calls for it, or "unplanned".
   Then list each plan deliverable and mark "in diff", "deferred with
   note", or "missing". Then list each description claim and mark
   "matches diff" or "contradicted by diff", quoting the hunk.
2. test-decisive: list each failure mode from the plan's test plan.
   For each, find the before and after output in the test evidence and
   check that the command exercises the failing path (not a control or
   an unchanged path). Mark "shown before+after", "after only",
   "control only", or "absent".
3. diff-clean: scan every added line for debug output (print, eprintln,
   console.log, DEBUG), commented-out code, dead or unused functions,
   TODO/FIXME noise, and every hunk for whitespace/re-indent/import
   churn on lines the fix does not need. Quote each hit.
4. standards-met: put each template/contributing ask from Read order
   step 1 next to the description and diff; mark "met" or "missing".
   Mark disclosure "present", "absent", or "not required".
5. repo-checks-run: note whether the evidence shows the repo's own
   test/check command and its result.
In live mode, the same facts come from plan.md, `git diff main...HEAD`,
`git log --oneline main..HEAD`, the draft description file, the
captured test output, and the repo's PR template and CONTRIBUTING.md.

## Check execution

1. Run checks in rubric order: plan-fidelity, test-decisive,
   diff-clean, standards-met, then repo-checks-run.
2. Grade each check only from its gathered notes, applying the rubric's
   pass condition literally, and write one evidence line quoting the
   deciding fact.
3. plan-fidelity fails on ONE "unplanned" hunk, ONE "missing" deliverable
   without a note, or ONE contradicted description claim. A deviation
   or deferral that is recorded in the plan note or the description
   counts as planned.
4. test-decisive fails if ANY failure mode the test plan names is not
   "shown before+after" on the failing path.
5. diff-clean fails on ONE debris hit, even when the fix is correct.
6. If evidence for a check is genuinely absent from the package (for
   example, no test-evidence section at all), grade `unclear` and say
   what is missing.
7. Grade every check even after one fails.

## Verdict assembly

1. If all four required checks are `pass`, the verdict is `accept`.
2. If any required check is `fail` or `unclear`, the verdict is
   `reject`; `unclear` counts as `fail`.
3. repo-checks-run never affects the verdict.
4. For a reject, the deciding check is the first failing required check
   in rubric order; quote its evidence line in the summary, then list
   any other failing checks.
5. Emit the JSON block last: one entry per check in rubric order.
