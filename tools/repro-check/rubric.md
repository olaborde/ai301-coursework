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

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment line(s), read against the issue's stated target (version, OS, platform/driver/shell) and the repo-facts bug-report template asks | The report names at least the tool version and the OS, plus any platform detail the issue says matters (driver, shell, browser, build). If the version or platform differs from what the issue targets, the report says so out loud. No environment record at all fails, even when the artifact looks right. | required |
| steps-rerunnable | The repro report's steps/commands and any input files they use | A stranger with only the posted comment could re-run it: the exact command(s) and input (or a public link to them) are given, from a stated starting state through the trigger. Fails if any needed input lives only in a private/unshared repo or config, or a step is a summary ("set up the project") with no command. | required |
| behavior-matches-issue | The artifacts in the repro report (output excerpts, logs, exit codes, error text) read against the behavior the issue and maintainer thread describe | The shown artifact is the issue's behavior, triggered by the issue's trigger (same error type/message, same wrong output, same crash vs. not-crash), OR the report is an honest cannot-reproduce (see outcome-honest). Fails if the input or command was changed so a different error appears, if the artifact shows only that the tool runs, if there is no artifact at all, or if the run used a version the issue does not target without saying so. | required |
| outcome-honest | The report's claims ("reproduced", "confirmed", "root cause is", "guaranteed") read against what its artifacts actually show | Every claim is backed by a shown artifact: the report states what happened, no more. A cannot-reproduce passes when it shows the attempt's artifacts and names what differed from the reporter's setup. Fails if it claims reproduction, a root cause, or certainty that the shown artifacts do not show, or generalizes beyond what was tested. | required |
| claim-specific | The candidate claim comment, read against the issue title/body/thread | The claim names this issue's specifics (the behavior, file, command, or maintainer pointer) and states a concrete next step about investigating. Fails if it is interchangeable boilerplate ("assign me", "+1", "I can fix this"), or promises a fix, a guaranteed outcome, or a date/deadline. | required |
| ai-disclosure | The "contribution policy" line in repo facts, read against the claim comment and repro report text. Treat every package as AI-assisted work. | If the policy requires disclosing AI use in issues/comments or "all AI usage in any form", at least one of the two comments discloses AI assistance (tool and extent). If the policy only asks for disclosure in pull requests, has conditions without a comment-disclosure duty, or states nothing, this check passes. | required |
| control-run | The repro report's artifacts | The report includes a control (the same command without the trigger, or the expected output) that shows the difference. | preferred |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->

Accept (ready to post) only if every required check grades `pass`:
env-recorded, steps-rerunnable, behavior-matches-issue, outcome-honest,
claim-specific, and ai-disclosure. Any required `fail` holds the package
(reject). An `unclear` on a required check counts as `fail`, because proof
we cannot verify is not ready to post. The only exception is a claim-only
live draft, where checks marked "not yet applicable: claim-only draft" are
left out. The preferred check (control-run) never changes the verdict.
Polish never earns a pass: a terse report with the artifacts passes, and a
long confident one without them fails.
