# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

- Where it lives: eval: the first lines of the "Candidate repro report"
  (often "Environment:"), read against the version/OS in the issue body
  and the "bug reports: template asks for" line in repo facts. Live: the
  top of my repro draft, against the issue body and the repo's
  `.github/ISSUE_TEMPLATE`.
- What good looks like: tool version and OS are named, plus any detail
  the issue or template marks as relevant (driver, shell, browser, build
  profile). A version or platform that differs from the issue's target
  is called out ("filed against 13.0.0; unchanged on 15.2.0"). A report
  with a good log but no environment at all is not placeable.

## Steps

- Where it lives: eval: the "Steps" part of the repro report, its code
  blocks, and any input file contents it quotes. Live: the commands in
  my draft and any snippet/gist they link.
- What good looks like: a stranger can start from nothing and reach the
  trigger: exact commands, the exact input (inline or a public link),
  and the starting state. Private repos, unshared configs, or "set up
  the project" steps fail.

## Behavior shown

- Where it lives: eval: the output excerpts, logs, exit codes, and error
  text inside the repro report, read side by side with the issue body's
  actual/expected and the maintainers' notes in "Thread highlights".
  Live: the pasted output in my draft vs. the issue thread.
- What good looks like: the artifact shows the same failure the issue
  describes (same error type, message, wrong output, or crash), produced
  by the issue's trigger. Watch for the adjacent symptom: a changed
  input that yields a different error (syntax error instead of panic,
  arg-validation instead of overflow), an old version's behavior, or an
  artifact that only proves the tool runs.

## Honesty

- Where it lives: the verbs in the claim comment and report
  ("reproduced", "confirmed", "verified the race", "guaranteed"),
  checked against the artifacts the report actually shows.
- What good looks like: each claim points at an artifact. A
  cannot-reproduce is honest when it shows the attempt's output and names
  what differed (OS, shell, sizes) and what might trigger it. A root
  cause or certainty with no artifact behind it, or a garbled-output
  artifact narrated as a crash, is dishonest even if well written.

## Comms

- Where it lives: the "Candidate claim comment" read against the issue
  title, body, and thread; both comments read against the repo-facts
  "contribution policy" line (live: CONTRIBUTING.md, AI_POLICY.md, and
  the issue/PR templates).
- What good looks like: the claim names this issue's specifics and an
  investigation step, and promises no fix or date. If the policy
  requires disclosing AI use in issues or comments (e.g. "all AI usage
  in any form must be disclosed"), one of the comments says which tool
  was used and how. PR-only disclosure rules, conditions without a
  disclosure duty, and silence do not require a comment disclosure.
