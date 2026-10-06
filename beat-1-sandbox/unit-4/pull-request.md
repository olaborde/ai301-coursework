# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/97

**Branch**

fix/58-bias-detector-patterns

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1 (saved as `eval-run.txt`): **19/20**, categories clear-accept 6/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, unreviewable 3/3. PASS. This was my only full run, so it is also the final one.

**Package analysis**

**pkg-05** (nushell keybinding merge, clear-accept). Gold label: `accept`. My rubric: `reject`, failed on `test-decisive`.

The plan's test plan names three things: re-run the issue's script and expect both `atuin` rows; re-run with two same-name, same-key bindings and expect one row plus a warning; and `cargo test` on the config crate. The PR's evidence shows the first one decisively, as a before table with one row and an after table with two. For the second, it only says "Same-key redefine prints the one-time warning", with no command or output. My `test-decisive` check fails if "a failure mode the test plan names is not re-run" with before and after shown, so the grader held it on that sentence. The gold label reads the main repro's before/after plus the named `cargo test` result as decisive enough, because the second case is a guard, not the reported bug. I kept the strict reading, because it is the clause written to catch pkg-07 and pkg-14 (evidence that runs a control or the unchanged path) and calib-04 (one of two named failure modes never re-run). The cost is this one clear-accept.

**Check rationale**

Quoted from `tools/pr-precheck/rubric.md`:

> | test-decisive | The test-evidence section, read against the plan's test plan and the repro's steps | The evidence re-runs the plan's repro (every failure mode the test plan names) on the changed code path and shows before AND after output, with the after matching the plan's expected result. Fails if the evidence is only "tested locally", "works on my machine", "tests pass" with no named output, if it exercises only a control or unchanged path instead of the failing one, or if a failure mode the test plan names is not re-run. | required |

Why it reads this way: the not-tested packages each fail in a different way. pkg-04 and pkg-10 say "tested locally" or "works on my machine" with no output. pkg-07 runs only the single-file control, which never breaks. pkg-14 runs a GET when the bug is a POST. A check that only asks "is there test evidence?" passes three of the four. So the condition names each escape route: no named output, control or unchanged path only, and a named failure mode not re-run. It also requires before AND after, because an after on its own can't show the fix changed anything.

**Trade-offs**

It gives up pkg-05, a gold accept, as described in Package analysis: a PR that shows the main repro decisively but only asserts a secondary case named in the test plan is held. I accept that miss rather than loosen the "every failure mode" clause, because loosening it would also let calib-04 through (that plan names two failure modes and the evidence re-runs only one), and calib-04 is exactly the borderline the master rejects on this check. With the strict clause, the run still clears the bar at 19/20 with not-tested 4/4.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
