# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue context (title, body, thread highlights) first. Write
   down: the reported behavior, and every explicit maintainer/owner
   direction in the thread (culprit named, approach chosen, test asked
   for, PR pointed at), each with its date and author.
2. Read the repro-evidence block second, before the plan, so the plan
   cannot frame it. Write down: the behavior it shows, and every control
   run or intermediate observation, and what each one rules in or out
   ("same input without flag X parses fine, so the item parser is not
   the cause").
3. Read the repo-facts block. Write down the contribution policy's
   AI-use terms (disclosure required? human-written comments
   required? silent?) and any template/contributing asks.
4. Read the candidate plan. Write down its stated cause, scope /
   not-in-scope, files and approach, test plan, and risks/unknowns.
5. Read the candidate plan comment last, since its checks compare it
   against what steps 1 and 3 recorded.

## Evidence gathering

1. For cause-grounded: list each repro artifact from Read order step 2
   next to the plan's stated cause. For each, mark "consistent" or
   "rules it out", quoting the artifact line.
2. For scope-bounded: list each item the plan says it will change or
   add. Mark each "needed to fix the reported behavior" or "beyond the
   issue" (refactor, migration, upgrade, new option, framework, CI,
   UI rework). Note any explicit not-in-scope/deferred items.
3. For executable: copy the plan's files/functions and its approach
   sentence. Note any hedge words that leave a choice open ("or",
   "maybe", "somewhere", "not sure", "investigate first").
4. For test-decisive: copy the test plan and the expected result it
   names. Note whether it re-runs a repro step or names a test.
5. For unknowns-honest: copy every certainty word ("guaranteed",
   "definitely", "will fix") and its claim, and the plan's stated
   unknowns.
6. For thread-and-policy: put the maintainer directions from Read
   order step 1 next to the plan comment and mark each "engaged" or
   "ignored". Put the policy terms from step 3 next to the comment and
   mark disclosure "present", "absent", or "not required".
7. For prior-art: note any open or closed PRs in the issue context and
   whether the comment mentions them.
In live mode, take the same facts from the live issue thread, the
student's posted repro comment, docs/CONTRIBUTING.md, and the drafts
plan.md and comment.md, as the evidence guide maps.

## Check execution

1. Run the checks in rubric order: cause-grounded, scope-bounded,
   executable, test-decisive, unknowns-honest, thread-and-policy, then
   prior-art.
2. Grade each check only from the notes gathered for it, applying the
   rubric's pass condition literally. Write one line of evidence: the
   quote or fact that decided it.
3. cause-grounded fails as soon as ONE repro artifact rules the cause
   out; a single contradicting control is enough.
4. scope-bounded fails as soon as ONE planned item is "beyond the
   issue" and is not explicitly deferred.
5. If a check's evidence is genuinely absent from the package (for
   example, the plan has no test plan at all), grade it `unclear` and
   say what was missing. Don't grade `unclear` for something you didn't
   look for.
6. Grade every check even after one fails, so the output shows all
   problems.

## Verdict assembly

1. Collect the six required grades. If all six are `pass`, the verdict
   is `accept`.
2. If any required grade is `fail` or `unclear`, the verdict is
   `reject`. `unclear` counts as `fail`.
3. Ignore prior-art for the verdict; report it only.
4. In the summary, name the deciding check(s) for a reject and quote
   the evidence line that decided each one.
5. Emit the JSON block from SKILL.md last, with one entry per check in
   rubric order.
