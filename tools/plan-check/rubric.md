# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's stated cause/diagnosis, read against every artifact in the repro-evidence block (including control runs, debug output, and step-by-step observations) | The plan names a cause, and that cause is consistent with ALL repro artifacts. Fails if any repro artifact (a control run that works, a step showing the bad state already exists earlier, a debug trace pointing elsewhere) rules the named cause out, if the plan ignores the repro evidence, or if no cause is stated. A plausible-sounding cause that a control contradicts fails even if the thread proposed it. | required |
| scope-bounded | The plan's scope / in-scope and not-in-scope statements and its files-to-touch list, read against what the issue asks for | The planned change fixes the reported behavior and nothing the issue did not ask for. Fails if the plan bundles in a refactor, rewrite, migration, dependency upgrade, new option/feature, framework, CI matrix, or "while I'm in the area" work beyond the fix. Explicitly deferring related work ("not in scope: X") passes, and an honest scope-down that defers a hard variant with a reason passes. | required |
| executable | The plan's approach and files-to-touch | A stranger could start the work without asking the author anything: the plan names the file(s) or function(s) to change and commits to ONE approach. Fails if the location is "somewhere"/unknown, the approach is still a choice ("upstream or vendored, whichever is easier", "gocui? tcell?"), or the plan is "investigate/profile first" with nothing chosen. | required |
| test-decisive | The plan's test plan, read against the repro evidence's steps and artifacts | The test plan names an observable outcome tied to this fix: re-running the repro (or a named test/fixture) with a specific expected result that differs from the broken one. Fails if the only test is "run the full test suite", "should feel fast/not broken", or anything with no stated expected output for the fix itself. | required |
| unknowns-honest | The plan's risks/unknowns section and the confidence of its claims | Uncertainties the evidence leaves open are stated as unknowns, not asserted as fact. Fails if the plan claims certainty ("guaranteed", "this will definitely fix") about things the repro evidence does not show. | required |
| thread-and-policy | The plan comment, read against maintainer comments in the thread highlights and the repo-facts contribution policy. Treat every package as AI-assisted work. | Both hold: (a) if a maintainer/owner gave explicit direction in the thread (isolated a culprit, chose an approach, asked for testing, pointed at a file or PR), the plan comment engages it (follows it, or says why not); a plan that silently goes a different way fails. (b) If the policy requires disclosing all AI usage (or AI use in issues/comments), the plan comment discloses it; if the policy requires human-written comments, the comment is in the author's own voice. Silence in the policy passes (b). | required |
| prior-art | The plan comment and the thread's linked/open PRs | The plan mentions any open PR or earlier attempt on the issue and how it relates. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept (ready to post and build from) only if every required check
grades `pass`: cause-grounded, scope-bounded, executable, test-decisive,
unknowns-honest, and thread-and-policy. Any required `fail` rejects. An
`unclear` on a required check counts as `fail`: a plan we cannot verify
from the package is not ready to build from. The preferred check
(prior-art) never changes the verdict. Length and polish never earn a
pass; a terse plan that passes every check is ready.
