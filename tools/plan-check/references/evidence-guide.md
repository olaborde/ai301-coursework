# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

- Where it lives: eval: the plan's cause/diagnosis section vs. the
  "repro evidence" block (its steps, outputs, control runs, debug
  traces). Live: plan.md's diagnosis vs. my posted repro comment on the
  issue.
- What good looks like: the stated cause explains every artifact,
  including the controls. If a control shows the suspected component
  working (same input passes without the flag; a sibling call prints
  fine in the same build; the data is already wrong before the step the
  plan blames), that cause is ruled out, however confident the plan
  or thread sounds.

## Scope

- Where it lives: the plan's scope / in-scope / not-in-scope lines and
  its files-to-touch list. Live: the same sections of plan.md.
- What good looks like: one change that fixes the reported behavior,
  with related work named as deferred. Red flags: "while I'm here",
  rewrites, migrations, dependency upgrades, new options, new
  frameworks, CI matrices, UI rework.

## Executability

- Where it lives: the plan's files/functions and approach sections.
- What good looks like: named files (and ideally functions) and one
  chosen approach. "Somewhere", "X or Y, whichever is easier", "not
  sure which layer", or "profile first" means a stranger can't start.

## Test plan

- Where it lives: the plan's test plan, mapped onto the repro
  evidence's steps.
- What good looks like: it re-runs the repro (or names a test/fixture)
  and states the expected output after the fix, which differs from the
  broken output shown in the repro. "Run the full suite" or "should
  feel fast" names nothing observable for this fix.

## Honesty

- Where it lives: the plan's risks/unknowns section, certainty words
  anywhere in the plan or comment, and (live, after the build) the
  `## Deviations` section of plan.md.
- What good looks like: open questions are labelled as unknowns, with
  how they'll be checked. Certainty ("guaranteed", "definitely") only
  about things the repro shows. A mid-build change is recorded under
  Deviations, not only in the diff.

## Comms

- Where it lives: the plan comment vs. the thread highlights'
  maintainer/owner comments, and the repo-facts contribution policy
  (live: the issue thread, docs/CONTRIBUTING.md, AI_POLICY files, PR
  template).
- What good looks like: the comment engages explicit maintainer
  direction (follows the named culprit/approach or says why not) and
  any open PRs. If the policy requires disclosing all AI usage, the
  comment says which tool and how it was used. If it requires
  human-written comments, the comment is in the author's own voice.
  A silent policy requires nothing.
