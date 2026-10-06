# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

olaborde

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58#issuecomment-6007600090

Plan for #58, built from my repro above (main at `2f4e82f`).

**Diagnosis.** My control run, `detect_bias("bootcamp graduates lack rigor")` → `(True, 'Dismissive language about educational background')`, next to `(False, '')` for both of the issue's sentences, shows the matching loop in `detect_bias()` works; the problem is the pattern lists in `safety/bias_detector.py`. Each regex in `DISMISSIVE_PATTERNS` and `DEMOGRAPHIC_PATTERNS` needs one fixed word sequence, so the 9 failing tests miss on: subjects (`coding bootcamp`, `programmers`, plural `young developers`, `developers from poor backgrounds`), verbs (`can't` after `bootcamp graduates`, `lacks` right after `education`, `are not equal`), and forms like "bootcamp attendance means inadequate training". The issue's two sentences ("only attended a bootcamp ... lacks the rigor", "Given their age ... cannot keep up") also have no matching form.

**Scope.** Widen those two pattern lists only, remove the 9 `@pytest.mark.xfail` markers for #58, and add a test for each of the issue's two sentences. Not changing `detect_bias()`'s logic or return values, the reason strings, or any other module, and not trying to catch every possible biased phrasing.

**Files.** `safety/bias_detector.py`, `tests/unit/test_bias_detector.py`.

**Test plan.** Re-run my repro: the issue's two sentences should go from `(False, '')` to `(True, <reason>)`, the `bootcamp graduates lack rigor` control should stay flagged, and `pytest tests/unit/test_bias_detector.py -q` should go from `23 passed, 9 xfailed` to all passing with no xfails, including the 9 "not flagged" tests. Then `make lint`, `make typecheck`, `make test-unit`.

**Unknowns.** Wider patterns could flag neutral feedback. The widest change is that the `<bootcamp|self-taught> <graduates|developers|programmers> <verb>` pattern will no longer require `rigor|fundamentals|proper training` after the verb (so `can't write production code` matches), which means "bootcamp graduates lack X" flags for any X; the existing "not flagged" tests guard that, but I haven't tested phrasings beyond the suite. I'll also check `pyproject.toml` for any suppression tied to #58.

AI disclosure: I used Claude Code to help analyze the code and draft this plan; I reviewed it and will review every diff myself.

---

## Your branch

**Branch**

fix/58-bias-detector-patterns

**Evidence**

Commands (run from the top of my fork clone, macOS 26.4.1, Python 3.11.15):

```bash
.venv/bin/python -c '
from safety.bias_detector import BiasDetector
print(BiasDetector.detect_bias("The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education"))
print(BiasDetector.detect_bias("Given their age, they likely cannot keep up with modern frameworks"))
print(BiasDetector.detect_bias("bootcamp graduates lack rigor"))'
.venv/bin/pytest tests/unit/test_bias_detector.py -q
make test-unit   # before only, for the suite baseline
```

Before (`main` at `2f4e82f`):

```
## before (main at 2f4e82f)
(False, '')
(False, '')
(True, 'Dismissive language about educational background')
23 passed, 9 xfailed in 0.77s
================= 375 passed, 53 xfailed, 3 warnings in 10.98s =================
```

After (branch `fix/58-bias-detector-patterns` at `01f7993`):

```
## after (branch fix/58-bias-detector-patterns)
(True, 'Dismissive language about educational background')
(True, 'Demographic assumptions detected')
(True, 'Dismissive language about educational background')
34 passed in 0.76s
```

The issue's two sentences go from `(False, '')` to flagged, the `bootcamp graduates lack rigor` control stays flagged, and the bias detector suite goes from 23 passed + 9 xfailed to 34 passed (the 9 former xfails plus 2 new tests for the issue's sentences). On the branch, `make test-unit` gives `386 passed, 44 xfailed` (main: `375 passed, 53 xfailed`), `make lint` gives "All checks passed!", and `make typecheck` gives "Success: no issues found in 76 source files".

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run 1 (saved as `eval-run.txt`): **20/20**, categories clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. PASS. This was my only full run, so it is also the final one.

**Package analysis**

**pkg-16** (pandas-dev/pandas#57666, pyarrow engine strips leading zeros, wrong-cause). Gold label: `reject`. My rubric: `reject`. Because the run agreed, the table doesn't list the failing check; under my rubric the check this package hits is `cause-grounded`.

The plan is long and confident and blames the pandas-side cast after the read ("The cast is where the data is damaged, so the cast is what must change"), with a fix that re-pads the rendered strings. My procedure reads the repro-evidence block before the plan and writes down what each step rules in or out. The package's step 4 shows the zeros are already gone in pyarrow's inferred int64 table before any cast runs. So by the time the plan's fix would act, the original widths are unknown and re-padding can't recover them. My `cause-grounded` check fails as soon as one repro artifact rules the stated cause out, so a plan like this is rejected however polished it is. Reading evidence before the plan is what made this one easy: if the plan is read first, its framing ("the cast is where the data is damaged") makes step 4 look like a detail instead of the deciding fact.

**Check rationale**

Quoted from `tools/plan-check/rubric.md`:

> | cause-grounded | The plan's stated cause/diagnosis, read against every artifact in the repro-evidence block (including control runs, debug output, and step-by-step observations) | The plan names a cause, and that cause is consistent with ALL repro artifacts. Fails if any repro artifact (a control run that works, a step showing the bad state already exists earlier, a debug trace pointing elsewhere) rules the named cause out, if the plan ignores the repro evidence, or if no cause is stated. A plausible-sounding cause that a control contradicts fails even if the thread proposed it. | required |

Why it reads this way: the wrong-cause packages (pkg-01, 07, 11, 16) and calib-03 all share one trap. The diagnosis sounds right, sometimes because the thread proposed it, but a control or an intermediate step in the package's own repro evidence rules it out. A check like "the cause is plausible" passes all of them. So the pass condition is about consistency with ALL artifacts, says a single contradicting control is enough to fail, and explicitly says a thread-proposed cause still fails if a control contradicts it (that is calib-03's lesson, where the thread's key-binding theory was wrong).

**Trade-offs**

`cause-grounded` gives up plans whose cause is right but whose repro evidence happens to include a noisy or badly designed control that looks like a contradiction. A strict "one contradicting artifact fails" rule will hold those, and the author has to explain the control away first. I accept that miss. Nothing changed elsewhere, and I know because the run shows clear-accept 7/7: every good plan's control actually supports its cause (e.g. pkg-02's saturating clamp at the two subtraction sites the repro isolates), so the strictness cost no accept in this set.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
