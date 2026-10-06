---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

Answer one question about one PR package: **is this pull request ready
to submit?** A PR package is a candidate pull request (its title,
description, commits, unified diff, and test evidence) read against the
plan it claims to implement (scope, boundary, test plan, and any
deviation notes) and the issue that plan belongs to, plus the repo's
stated standards (PR template, contributing asks, AI-use policy). Grade
nothing else: not the plan's quality, not the issue's worth, not the
code's style beyond what the rubric checks. Grade exactly one package
per run.

## Inputs and modes

**Live mode** (the student's own PR before it is opened). Gather:

- The plan: `plan.md` in the top folder of the student's fork clone,
  including its `## Deviations` section.
- The diff: run `git diff main...HEAD` (three dots) from the fork
  clone on the fix branch; also `git log --oneline main..HEAD` for the
  commits. If the default branch is not `main`, use it instead.
- The draft PR title and description: the file the student names
  (for example `pr.md`).
- The test evidence: the before/after output the student names (in the
  description or a file they point to), plus the repo's own check
  output (`make check`, `make test-unit`) if provided.
- The issue: the URL the student gives; read its thread with `gh issue
  view <n> --comments`, and the repo's `.github/PULL_REQUEST_TEMPLATE.md`
  and `docs/CONTRIBUTING.md`.

A house-chain student reads the house plan and house repro pack in
place of their own plan and repro; the same checks grade them.

**Eval mode** (a package bundle). The bundle is the whole world: use
only its text (repo facts, issue, thread highlights, plan context,
candidate PR). Fetch nothing, read no other file, and ignore
`scope.md` and `voice-guide.md`. Always grade every check with the
full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Confirm the PR
targets the repo on its `Repo:` line and comes from the student's fork;
refuse to grade anything else and say why. Apply its house rules when
reading evidence (for example, classmates' PRs on the same issue do not
block this one). If the `Repo:` line still shows a bracketed
placeholder, stop without grading and tell the student to get their
cohort's scope file from the instructor; never guess a scope. In eval
mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after the scope. Hold the draft PR
title and description against each of its rules and its "Things I
never post" list. In the summary, list every rule the draft breaks,
quoting the rule and the offending line. The voice guide never changes
the verdict by itself, because no rubric check reads it; it is the
student's own standard, reported out loud. In eval mode, ignore
`voice-guide.md` entirely.

## Component reads

Read these from this tool directory, in this order:

1. `rubric.md`: the checks (name, evidence, pass condition, weight)
   and the verdict rule. It decides WHAT passes.
2. `references/evidence-guide.md`: where each evidence family lives in
   a package (and live, in the working copy and on GitHub) and what
   good looks like. It decides WHERE to look.
3. `procedure.md`: the operating steps. Execute it exactly as written;
   it decides HOW and in what order.

When the procedure is silent on a step you need, do not improvise
around it: grade what you can and name the gap in the summary ("the
procedure does not say how to ..."). If `rubric.md` has no checks or
`procedure.md` has no steps, refuse to grade: say which file is empty
and stop, without emitting a verdict.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third option and no score; reservations go in check
evidence lines. You may print a short per-check summary first (and, in
live mode, the voice-guide notes). End the reply with this fenced JSON
block, valid and last, with nothing after it, one entry per rubric
check in rubric order:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff itself against the
  plan, the evidence itself against the test plan, and the
  description itself against the diff. A terse PR can be ready; a
  polished one can hide drift.
- The rubric decides: if a check passes by its stated condition but
  feels wrong, it passes; note the tension in the summary.
- The procedure decides how: follow `procedure.md` as written and
  report its gaps.
- Unclear defaults to fail: `unclear` means the evidence is genuinely
  absent. Treat it as the rubric's verdict rule says; where the rule
  is silent, an unverifiable claim fails.
