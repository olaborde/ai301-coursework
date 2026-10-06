# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

- Where it lives: eval: the "Plan context" block (scope, files,
  promised deliverables, test plan, deviation/deferral notes) vs. the
  candidate PR's "Diff" and the fidelity claims in its "Description".
  Live: plan.md (with `## Deviations`) vs. `git diff main...HEAD` and
  the draft description.
- What good looks like: every changed file and hunk is something the
  plan or a deviation note calls for; every promised deliverable is in
  the diff or explicitly deferred; the description's claims ("exactly
  as planned") are true of the diff. Drift in either direction: extra
  work (a new flag, a renaming pass, a rewrite of a file the plan never
  names) or missing work (docs promised, absent) with no note, or a
  description that claims more or less than the diff.

## Test evidence (harness category: not-tested)

- Where it lives: eval: the candidate PR's "Test evidence" section vs.
  the plan's test plan and the repro evidence. Live: the captured
  before/after output and `make check` / `make test-unit` output.
- What good looks like: the plan's repro command(s) re-run on the
  failing path, with before and after output, the after matching the
  plan's expected result, for every failure mode the plan names, plus
  the repo's own test command with its result. "Tested locally", "works
  on my machine", "cargo test passes", or a run of only the control /
  unchanged path is not evidence of this fix.

## Diff quality (harness category: unreviewable)

- Where it lives: eval: the candidate PR's "Diff" and "Commits". Live:
  `git diff main...HEAD` and `git log --oneline main..HEAD`.
- What good looks like: only the fix and its tests. Debris tells:
  debug prints/eprintln/console.log, commented-out code or first
  attempts, dead functions (often behind allow(dead_code)), TODOs,
  re-indent or formatting churn, import reshuffles, re-printed
  identical lines, and "wip"/"misc cleanups" commits.

## Standards and comms (harness category: standards-wall)

- Where it lives: eval: the repo-facts "pull requests" / template /
  contributing line and the "contribution policy" line vs. the PR
  description and diff; the thread highlights for maintainer
  direction. Live: `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, any AI policy file, and the issue thread.
- What good looks like: every required template section and checklist
  item is filled with real content ("closes #N", changelog/whatsnew
  entry if required, checklist ticked truthfully); if the policy
  requires disclosing AI usage, the description names the tool and how
  it was used. A silent policy requires no disclosure. Path Review's
  template has its own sections; fill each one.
