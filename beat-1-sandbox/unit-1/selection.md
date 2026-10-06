# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58

**Verdict output**

Live-mode run: `claude "issue-select: grade these candidate first issues: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55 https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70"`. The chosen issue, #58, is ranked first with verdict `accept`.

````
I graded all three against your rubric, and **all three are accepted**. Every required check passes on each one. They differ only on fit and the preferred checks.

**Repo facts (shared by all three):** the repo isn't archived. The last default-branch commit and the last push were both on 2026-09-16, 19 days ago. `docs/CONTRIBUTING.md` and the PR template say nothing about AI, so there's no ban.

### Ranked (all accepted)

1. **#58 Bias detector patterns are too narrow.** This is the best fit. It's pure Python regex work in `safety/bias_detector.py`, which is the "regex-heavy code" you said you're comfortable reading. It reproduces with a one-line `python -c` call plus 9 xfail tests in `tests/unit/test_bias_detector.py`, and doesn't need Docker or an LLM key. A maintainer (COLLABORATOR) filed it. One risk: "natural phrasings" is open-ended. Aim at the 9 named tests and don't try to catch every phrasing.
2. **#55 Skill extractor misses JS/TS.** Also Python, regex and pytest (5 xfail tests). It ranks second because the fix touches three functions (`_detect_languages`, `_detect_tools`, `_detect_databases`). That's still one bounded problem, but wider than #58. It's also the most crowded issue: three classmates have posted claims and repro reports. The house rule says that doesn't block you, but your PR will compete with theirs on the same problem.
3. **#70 README parser fixture is indented.** This passes cleanly and has no claims at all. It ranks last only because the fix is mostly removing indentation in `tests/conftest.py` and dropping an xfail marker. That's very little practice at reading unfamiliar code or writing a fix with tests, which is what you said you want.

**Notes:**
- **Unclaimed:** #58 and #55 both have recent classmate claims. Under the Path Review house rule I ignored them. I found no assignees and no open PRs for any of the three. The only open fix PRs in the repo (#88 and #95) are for #53.
- **maintainer-engaged (preferred):** passes for all three, but only because a maintainer opened each issue. In the whole repo there are just 2 maintainer comments, both from 2026-09-16, out of 537 total. Don't count on a fast review.
- **newcomer-label (preferred):** fails for all three. They're labelled `tier-1`/`tier-2`, with no `good first issue` or `help wanted`.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: isArchived=false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Latest main commit 2026-09-16 (19 days before 2026-10-05); last push 2026-09-16"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees none; no linked or mentioned PRs; claims by RichardGabelman/Dannypxp (2026-09-28) are classmate claims, ignored per the Path Review house rule"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR Aburke225; one problem (regex patterns in bias_detector.py) pinned by 9 tests in test_bias_detector.py"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md say nothing about AI; no AI_POLICY file"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opener author_association COLLABORATOR"},
      {"name": "newcomer-label", "grade": "fail", "evidence": "Labels: bug, safety, tier-1; no good-first-issue/help-wanted"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/55",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: isArchived=false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Latest main commit 2026-09-16 (19 days before 2026-10-05); last push 2026-09-16"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees none; no linked or mentioned PRs; claims by ArshAnan, aptitudepi, Nexus-00 (2026-09-28 to 09-30) are classmate claims, ignored per the house rule"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR; one detection problem in skill_extractor.py pinned by 5 xfail tests"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md say nothing about AI; no AI_POLICY file"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opener author_association COLLABORATOR"},
      {"name": "newcomer-label", "grade": "fail", "evidence": "Labels: bug, ingestion, tier-1; no good-first-issue/help-wanted"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/70",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "gh repo view: isArchived=false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Latest main commit 2026-09-16 (19 days before 2026-10-05); last push 2026-09-16"},
      {"name": "unclaimed", "grade": "pass", "evidence": "Assignees none; zero comments; no linked or cross-referenced PRs"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Opened by COLLABORATOR; names 3 files, one fixture fix plus removing xfail H-03, estimated 2-4 hours"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md say nothing about AI; no AI_POLICY file"},
      {"name": "maintainer-engaged", "grade": "pass", "evidence": "Issue opener author_association COLLABORATOR"},
      {"name": "newcomer-label", "grade": "fail", "evidence": "Labels: bug, ingestion, tier-2; no good-first-issue/help-wanted"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run 1: **17/20** (below the bar). Misses: issue-01, issue-04, issue-19, all gold `accept`, all failed on `bounded-scope`.
2. Partial `--only issue-01,issue-04,issue-19,issue-05,issue-10,issue-15,issue-20` after loosening `bounded-scope` for maintainer-filed issues: 6/7 (issue-04 and issue-19 fixed, scope canaries 05/10/15/20 still reject, issue-01 still wrong). Partial run, no bar verdict.
3. Partial `--only issue-01,issue-05,issue-10,issue-20` after adding the "written spec naming files" clause: 4/4. Partial run, no bar verdict.
4. Full run 2 (confirming, saved as `eval-run.txt`): **20/20**, categories claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1, scope 4/4. PASS.

**Issue analysis**

**issue-04** (zxcalc/zxlive#555, "Missing several basic rule previews"). Gold label: `accept`. My rubric's first run said `reject` (failed: `bounded-scope`); my final rubric says `accept`.

Why it rejected at first: the whole body is one line, "Including remove identity, fuse spiders, remove self loops, etc." My first `bounded-scope` check failed issues that list "many sub-items meant for separate PRs", and the grader read "several rule previews ... etc." as an open-ended list. But the opener is a COLLABORATOR, it has a `good first issue` label, and every listed item is the same kind of fix (add a missing preview) in the same proof-mode feature. It is one problem with several instances, not an umbrella. issue-19 failed the same way (a maintainer listing two causes plus suggestions). The fix was to say a maintainer-filed issue passes by default even if terse or listing related items, and to only fail on a self-described umbrella/tracking/megaissue or "the codebase"-wide work. After that, issue-04 passes `bounded-scope`; it already passed liveness (last commit 2026-08-04), unclaimed (no assignee, no linked PR, 0 comments), and policy (no AI statement).

**Check rationale**

Quoted from `tools/issue-select/rubric.md`:

> | unclaimed | "this issue: assignees" and "linked PRs" in Repo facts, plus every comment in the thread that announces work ("I'll take this", "working on it", "I opened a PR", a PR link) | All three hold: (a) assignees is `none`; (b) no linked or thread-mentioned PR is in state `open` (closed or merged PRs do not block, they are history); (c) no non-maintainer comment claiming the work is dated within 60 days of the capture date. A claim older than 60 days with no open PR is stale and does not block. | required |

Why it reads this way: the claimed category (issue-03, 08, 13, 18) is about active work, not history. issue-03 and issue-18 have closed PRs next to open ones, and issue-09 (gold `accept`) has a closed PR from 2022 and a 2022 "I'd like to take a swing" comment. So the check has to tell open PRs from closed ones and fresh claims from stale ones. I chose "open" as the PR test because a closed PR is an abandoned attempt, and 60 days as the staleness cut-off because issue-09's claim is years old with no follow-up and issue-12's 2024 claim also went nowhere, while the claims that matter (issue-18's stack) are recent and come with open PRs. I added "thread-mentioned" PRs because the evidence guide warns that not every PR gets formally linked.

**Trade-offs**

The 60-day staleness window in `unclaimed` will miss a slow but real claim: someone who said "working on it" 70 days ago and is still quietly working with no PR yet would be graded unclaimed, and I'd collide with them. I accept that miss, because without a cut-off issue-09 (gold `accept`, 2022 claim) would be rejected. I checked that it changed nothing elsewhere: every claimed-category issue (03, 08, 13, 18) was still rejected in the final full run (claimed 4/4), because each has an assignee or an open PR, not only an old comment. In Path Review the scope.md house rule overrides it anyway (classmate claims don't block).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit: #58 is plain Python and regex in `safety/bias_detector.py`, and it reproduces with a one-line `python -c` call and 9 xfail tests, with no Docker or API keys. That suits the time I have and what I want to practise: reading unfamiliar code and making a focused fix with tests.
2. What the verdict got right and what it couldn't weigh: it correctly found the repo alive (commit 2026-09-16), no assignee or PR, a maintainer-filed bounded issue, and no AI ban. What it couldn't weigh is how open-ended "natural phrasings" is. Regex coverage of bias language can grow forever, so I have to scope my fix to the 9 named tests and say so. It also doesn't know that maintainers have barely commented in this repo, so reviews may be slow.
3. Difficulty claiming: two classmates (RichardGabelman, Dannypxp) already claimed it and posted repros. Under the house rule that doesn't block me, but my claim and repro have to be my own, specific, and not "same as above". My repro should add what theirs don't: the `--runxfail` assertion failures and a positive control.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
