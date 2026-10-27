# Contribution: `convection.cape_ratio` should be able to handle a CubeList

Path: `beat-2-contribution/contribution-story.md`

**Contribution Number:** 1
**Student:** Osse Laborde
**GitHub Username:** olaborde
**Issue:** https://github.com/MetOffice/CSET/issues/925
**Pull Request:** https://github.com/MetOffice/CSET/pull/2548

---

## Toolkit

**Unit 5: issue-scout, a search strategy, and three rubric patches from one wild scan**

I grew my unit-1 issue-select skill into issue-scout: 8 `gh search issues` queries (`good first issue` and `help wanted` across 4 groups of 8 Python orgs), 6 ranking filters, top 5 graded. The first scan returned 56 issues, and all 5 of the top picks were rejected. Three already had linked PRs that my "fewest comments" ranking can't see. `-linked:pr` doesn't work through `gh search`, so I logged that and left it to the `unclaimed` check. beets' AI policy bans AI on good-first issues only, which my `ai-policy-allows` check read as allowed. I changed the check to also fail class-scoped bans, and I added a beets drop rule to the strategy.

**Unit 6: voice-guard and repo memory**

I merged my three voice guides into one canonical `~/.claude/voice-guard/voice-guide.md`, symlinked from repro-check, plan-check and pr-precheck. I added an "Upstream" section (claims, questions, pings). The symlink step broke something: `ln -sf` overwrote two rules ("Say how sure I am about the approach", "The description matches the diff") before I'd folded them in. I restored them from my unit-4 course-repo copy. I wrote `CLAUDE.local.md` in my CSET clone (29 lines, git-excluded), covering CSET's PR checklist, the AI disclosure rule, the `aviation.py` loop pattern, and the macOS environment recipe. The guard hook itself never fired for me. It calls `claude -p`, my nested `claude` login had expired, and I haven't merged the hook into my settings yet. So I checked the claim and the PR body against the guide by hand through repro-check and pr-precheck.

**Unit 7: ship-gate and gate manifest**

I wrote `ship-manifest.md` for CSET. Must-pass checks: `ruff check`, `ruff format --check`, the convection tests, and the operators suite. Exclusions are dated: `test_read::test_cell_methods_computes_time_bounds`, which fails identically on `main` at `37588e03` on my machine; `make test-workflow`, which needs `mo_pack`, and `mo_pack` has no osx-arm64 build; and the docs build and coverage gate, which CI runs. The gate's `install.py` was blocked by my session's safety classifier, so I couldn't install the hook. I ran every manifest command myself instead and saved the output to `evidence/manifest-run.txt`: all checks passed, 9 convection tests, and 830 passing in the operators suite. Writing the exclusions down was the useful part. Without them I'd have read the pre-existing `test_read` failure as mine.

**Unit 8: PR-monitor and playbook**

I wrote my playbook (`~/.claude/pr-monitor/playbook.md`) from CSET's real rhythm. Its last 20 merged PRs closed in 0 to 8 days, with one 27-day outlier, so my nudge threshold is 14 days, with one nudge and @-mentions only for people already on the thread. My first attempt to install `monitor.py` was blocked in my Claude session, so I read the state by hand first with `gh pr view 2548` on 2026-10-27. It showed OPEN, REVIEW_REQUIRED, MERGEABLE, merge state BLOCKED, 0 comments, and 0 checks (CI hasn't started, which looks like the first-time-contributor approval gate). Then I installed the monitor myself and ran the free `--state` canary from my clone. It found PR #2548 and logged "quiet: under a day since the last activity by anyone but you", MERGEABLE/BLOCKED, which matched my hand read. By my playbook that means wait. There's no draft and nothing to post.

---

## Why I Chose This Issue

My wild scan with issue-scout came back empty: 56 issues, and the top 5 were all rejected for linked PRs, fresh claims, a dead default branch, or beets' AI policy. So I switched to the AI301 Issue Selection site. I screened about 180 Python issues there for assignees, open cross-referenced PRs, and claim comments in the last 120 days. That was a quick script, because my ranking filters can't see linked PRs. Most bug picks were already taken (scikit-learn, mahout, isort and packaging had open PRs; dismech closes PRs from forks), and conda#6697 no longer reproduced on conda 25.11.1. I fed three survivors to the scout directly. It accepted MetOffice/CSET#925 as "straightforward" and #926 as "moderate", and rejected power-grid-model-io#384 on scope ("clean generic errors across the repo").

I took #925 because it fits my profile exactly. It's plain Python and numpy, it touches one function, and it reproduces with a few lines and pytest, with no Docker and no keys. A maintainer filed it with `good first issue`, and CSET allows AI help as long as it's disclosed. The repo also already shows the answer's shape: `aviation.aviation_colour_state` loops `zip(iter_maybe(a), iter_maybe(b), strict=True)`. I wanted to learn how a real scientific codebase handles "one cube vs many models" without breaking existing recipes. As for the tool itself: on a real wild search the scout's ranking was blind to linked PRs and claims, which cost me a whole scan. The fix belongs next to the `unclaimed` check (a claim/PR screen before grading), and that's what I'd add.

Claim: https://github.com/MetOffice/CSET/issues/925#issuecomment-6440734536

**Toolkit**

- Tool: issue-scout. I ran a live scan first, then direct grading of three hand-screened URLs from the Issue Selection site.
- What it produced: a scan with 0 of 5 accepted, then a direct run accepting CSET#925 ("straightforward", one function, an existing loop pattern), accepting #926 ("moderate", an Orography pairing edge case), and rejecting power-grid-model-io#384 on `scope-feasible`.
- Difficulties: the ranking can't see linked PRs or claims; `-linked:pr` fails in `gh search`; the AI check missed class-scoped bans; it has no "still reproduces today" check (conda#6697); and the nested `claude` CLI login expired, so I ran the skill in-session.
- What changed: `ai-policy-allows` now fails class-scoped bans, beets good-first results are dropped, and the staff `scope-feasible` row plus a difficulty note replaced my `bounded-scope`. Next I'd add a claim/linked-PR screen and a "still reproduces" check.

---

## Understanding the Issue

**Problem Description**

`convection.cape_ratio(SBCAPE, MUCAPE, MUCIN)` only accepts single iris Cubes. CSET recipes increasingly pass a CubeList with one cube per model being compared (tracking issue #721), and this operator can't take one.

**Expected Behavior**

Given CubeLists of SBCAPE, MUCAPE and MUCIN, it should compute the ratio for each matching set and return a CubeList. A single set of Cubes should still return a single Cube, as before.

**Current Behavior**

It fails immediately with `AttributeError: 'CubeList' object has no attribute 'data'`, because the body reads `.data` and calls `.copy()` directly on its inputs.

**Affected Components**

- `src/CSET/operators/convection.py` (`cape_ratio` only)
- `src/CSET/_common.py` (`iter_maybe`, used but not changed)
- `tests/operators/test_convection.py`
- The recipe `CAPE_ratio_plot.yaml` consumes the output; it isn't changed.

**Toolkit**

- Tool: Claude Code reading the issue, #721, and `convection.py`/`aviation.py`/`_common.py`, plus my `CLAUDE.local.md` repo memory, which I wrote here.
- What it produced: a 29-line repo memory naming the function, the `aviation.py` pattern to copy, and the test fixtures (`ECFlagB.nc` is the known-good output).
- Difficulties: none met. The issue is short but #721 explained the "one cube per model" context.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

- macOS 26.4.1 (Apple Silicon), conda (Anaconda), Python 3.14.10, iris 3.16.2, CSET `main` at `37588e03`.
- Setup: fork MetOffice/CSET to olaborde/CSET, clone it, and add `upstream`. Then `conda env create -n cset-dev -f <environment.yml without mo_pack>`, `conda activate cset-dev`, and `pip install --no-deps -e .`.
- Challenges:
  - CSET's lockfiles are linux-only.
  - `mo_pack` has no osx-arm64 conda-forge build, so my first `conda env create` failed silently and left a broken env. I copied `requirements/environment.yml`, removed `mo_pack`, and rebuilt.
  - The only cost is that `.pp`-file readers and `make test-workflow` can't run locally; CI covers them.

**Steps to Reproduce**

1. In the env, from the repo root, read the three test cubes: `read.read_cube("tests/test_data/convection/{SBCAPE,MUCAPE,MUCIN}.nc")`.
2. Control: `convection.cape_ratio(sb, mu, cin)` gives `single Cube -> Cube (13, 600, 259)`.
3. Wrap each in a two-model `iris.cube.CubeList([c, c.copy()])` and call `cape_ratio` again.
4. Observed: `CubeList -> AttributeError: 'CubeList' object has no attribute 'data'`. Also `isinstance(cube, Iterable)` is `False`, which is why `iter_maybe` is the right wrapper.

**Reproduction Evidence**

- **Commit showing reproduction:** https://github.com/olaborde/CSET/commit/bd21808f89a4c540b7e78e5eb56195316654518f. Its `test_cape_ratio_cubelist` encodes the repro and fails with the AttributeError on `main` at `37588e03`.
- **Screenshots/logs:** before, on `main`: `single Cube -> Cube (13, 600, 259)` / `CubeList -> AttributeError: 'CubeList' object has no attribute 'data'` / `Cube is Iterable: False`. Written up in my repro report (`repro-draft.md`); the same before/after output is in the PR description.
- **My findings:** the math is fine (the control works); only the input handling is single-cube. A Cube isn't `Iterable`, so `iter_maybe` wraps it in a tuple and a loop over `zip(...)` handles both shapes.

**Toolkit**

- Tool: repro-check (live mode, with my unit-6 wild scope), grading `claim-draft.md` and then `repro-draft.md`.
- What it produced: `accept` on the claim (claim-only mode) and `accept` on the full repro package, with no voice-guide rule broken.
- Difficulties: environment setup, not the tool. `mo_pack` and the linux-only lockfiles cost an hour.
- What changed: I recorded the macOS recipe in `CLAUDE.local.md` so a rebuild is one command.

---

## Solution Approach

**Analysis**

`cape_ratio` in `src/CSET/operators/convection.py` treats `SBCAPE`, `MUCAPE` and `MUCIN` as Cubes (`.data`, `.copy()`), so a CubeList never gets past the first attribute access. Other operators converted under #721 (for example `aviation.aviation_colour_state`) solve this with `iter_maybe` from `src/CSET/_common.py`.

**Proposed Solution**

Loop over `zip(iter_maybe(SBCAPE), iter_maybe(MUCAPE), iter_maybe(MUCIN), strict=True)`, run the existing per-cube body unchanged on each set, collect the results into a CubeList, and return the single cube if there's only one. `strict=True` makes mismatched lengths raise `ValueError`. Not changing: the calculation, the `MUCIN_thresh` behavior, `inflow_layer_properties` (#926), recipes, and the environment files.

**Implementation Plan**

1. In `src/CSET/operators/convection.py`, wrap `cape_ratio`'s body in the `iter_maybe`/`zip(strict=True)` loop and return a Cube or CubeList.
2. Update the docstring: types become `Cube | CubeList`, plus a Returns explanation and a Raises `ValueError` section.
3. In `tests/operators/test_convection.py`, add a CubeList test against `cape_ratio_kgo` and a length-mismatch `ValueError` test.

Deviations: nothing changed from the plan. `ruff format` reflowed one assert in my new test. One thing the plan didn't predict: `pytest tests/operators` has one failure, `test_read::test_cell_methods_computes_time_bounds`, but it fails the same way on `main` (828 passed, 1 failed before; 830 passed, 1 failed after), so it's unrelated. I mentioned it in the PR rather than touching it.

**Toolkit**

- Tool: plan-check (live, wild scope) on `plan.md`, which I kept in the clone and git-excluded.
- What it produced: `accept`. Diagnosis, scope/not-scope, files, approach and test plan all traced to the repro.
- Difficulties: none met.
- What changed: nothing in the tool. I kept a Deviations section in the plan so pr-precheck could hold the diff against it.

---

## Testing Strategy

- **Repro re-run against the fix** (`evidence/repro-after.txt`):
  - `single Cube -> Cube (13, 600, 259)` (control, unchanged)
  - `CubeList -> CubeList` (before: `AttributeError: 'CubeList' object has no attribute 'data'`)
- **New tests:** `test_cape_ratio_cubelist` checks that a two-cube CubeList returns a CubeList of length 2, each matching `ECFlagB.nc` within `atol=1e-5` with NaNs equal. `test_cape_ratio_cubelist_length_mismatch` checks for a `ValueError`.
- **Repo checks**, via the ship manifest (`evidence/manifest-run.txt`):
  - `ruff check src tests`: All checks passed!
  - `ruff format --check src tests`: 113 files already formatted
  - `pytest tests/operators/test_convection.py`: 9 passed (7 before)
  - `pytest tests/operators` (with the dated `test_read` exclusion): 830 passed. Without the exclusion it's 830 passed and 1 failed, versus 828 passed and 1 failed on `main` (`evidence/operators-before.txt`, `operators-after.txt`).

**Toolkit**

- Tool: the ship-gate's manifest (`ship-manifest.md`), run by hand because the gate install was blocked.
- What it produced: all four must-pass checks green, with output saved.
- Difficulties: the gate's `install.py` was blocked by my session's safety classifier, and `mo_pack` stops the workflow tests locally.
- What changed: I added dated exclusions with reasons instead of letting a pre-existing failure block or mislead the ship. I still need to install the gate hook myself.

---

## Implementation Notes

**2026-10-27**

I claimed #925 after checking the thread was empty. I named my first step (reproduce on `main` with CubeLists) and a report-back window of one week.

**2026-10-27**

I built the macOS environment. The first `conda env create` silently failed on `mo_pack`; I rebuilt without it and installed CSET with `pip install --no-deps -e .`. The repro confirmed the AttributeError, with the single-Cube control working.

**2026-10-27**

I implemented the loop by copying `aviation.py`'s shape exactly, rather than inventing my own `isinstance` branching. That keeps the convention reviewers already know, including returning a bare Cube for length 1. The first operators run showed a `test_read` failure. I checked out `main`, saw the same failure there (828 passed, 1 failed), and recorded it as a dated manifest exclusion and in the PR body.

**2026-10-27**

pr-precheck (live) accepted the package on all five checks. I pushed `fix/925-cape-ratio-cubelist` to my fork and opened MetOffice/CSET#2548. The commit and PR are authored by me alone, and the body has the AI disclosure CSET requires.

**Toolkit**

- Tool: Claude Code in my CSET clone with `CLAUDE.local.md` loaded, writing the change against `plan.md`.
- What it produced: commit `bd21808f`, +72/-28 across 2 files.
- Difficulties: the nested `claude -p` calls that my hooks rely on failed on an expired login, so the voice-guard and ship-gate couldn't run automatically.
- What changed: I ran the same checks in-session and by hand. I need to re-run `claude` and `/login` so the hooks work.

---

## Pull Request

**PR Link:** https://github.com/MetOffice/CSET/pull/2548

**PR Description:** "Fixes #925 (part of #721)." It explains why `cape_ratio` failed on CubeLists, and that the fix follows `aviation.aviation_colour_state`'s `zip(iter_maybe(...), strict=True)` loop, returning a Cube for single input and a CubeList otherwise, with `ValueError` on mismatched lengths. It shows before/after repro output and test counts (convection 7 to 9, operators 828 to 830, plus the unrelated `test_read` failure that also happens on `main`), notes that `mo_pack` can't be installed on macOS so CI covers the `.pp` tests, discloses AI help ("AI-assisted code from Claude Code (Anthropic)"), and completes CSET's PR checklist. The CI box is left unticked with the reason given.

**Maintainer Feedback:**

- 2026-10-27: none yet. The PR is REVIEW_REQUIRED, and CI hasn't started (it looks like it's waiting on first-time-contributor approval).

**Status:** Awaiting review

**Toolkit**

- Tool: pr-precheck (live, after swapping its sandbox scope for the staff `scope-live.md`).
- What it produced: `accept`. plan-fidelity, test-decisive, diff-clean, standards-met and repo-checks-run all passed, and no voice-guide rule was broken.
- Difficulties: its scope file was still the sandbox one, which would have refused a MetOffice PR outright.
- What changed: I swapped in `scope-live.md` and kept the sandbox copy as `scope.md.sandbox`.

---

## Learnings and Reflections

**Technical Skills Gained**

- iris Cube vs CubeList semantics, and why a Cube isn't `Iterable`.
- CSET's `iter_maybe` + `zip(strict=True)` pattern for multi-model operators.
- Known-good-output (KGO) testing with `np.allclose(..., equal_nan=True)`.
- Building a conda env by hand when the lockfiles don't cover my platform.
- `gh search` limits: owner caps, and `-linked:pr` not working.

**Challenges Overcome**

- **An empty wild scan.** I moved to the Issue Selection site plus my own claim/PR screen.
- **A bug that no longer reproduced** (conda#6697). I re-picked instead of forcing it.
- **The broken `mo_pack` env.** I wrote a custom environment file.
- **A pre-existing test failure.** I proved it on `main` before excluding it.
- **Overwritten voice-guide rules.** I restored them from my course repo.
- **Hooks that couldn't run** (expired login, blocked installer). I ran their checks by hand and say so here.

**What I'd Do Differently Next Time**

- Screen for linked PRs and claims before ranking, not after grading.
- Check that a bug still reproduces before investing in it.
- Keep my `claude` CLI login fresh so the guard and gate hooks actually fire.
- Install each week's tool the day it ships rather than at the end.

---

## Resources Used

- CSET contributing guide (`docs/source/contributing/index.rst`): the PR checklist and the AI disclosure rule.
- MetOffice/CSET#721: the tracking issue explaining the CubeList conversions.
- `src/CSET/operators/aviation.py` (`aviation_colour_state`): the loop pattern I followed.
- `requirements/environment.yml` and `.github/workflows/pull-request-checks.yml`: the env recipe and the checks CI runs.
