# Plan for #58: Bias detector patterns are too narrow to match common phrasings

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58
Built from my repro: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/58#issuecomment-6007412211 (main at `2f4e82f`, macOS 26.4.1, Python 3.11.15)

## Diagnosis

From my repro:

> `(False, '')` for "The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education"
> `(False, '')` for "Given their age, they likely cannot keep up with modern frameworks"
> control: `(True, 'Dismissive language about educational background')` for "bootcamp graduates lack rigor"
> `--runxfail`: 9 failed, 23 passed (e.g. `test_dismissive_bootcamp_language_detected`: `E assert False is True`)

The control shows `detect_bias()` runs and the matching loop works: the exact
phrase is flagged. So the cause is not the control flow in `detect_bias()`, it is
the pattern lists. Each regex in `DISMISSIVE_PATTERNS` and `DEMOGRAPHIC_PATTERNS`
in `safety/bias_detector.py` requires one fixed word sequence. Reading the 9
failing tests against them:

- subjects are too narrow: no `coding bootcamp`, no `programmers`, no plural
  `people`/`developers` before `from <background>`, no `young developers`
  (age words only pair with singular `person|developer|programmer`);
- verbs are too narrow: `bootcamp graduates can't ...` (only `lack|missing`
  accepted), `bootcamp education lacks ...` (only `is insufficient|inadequate|lacks`
  after `is`), `self-taught developers are not equal` (only `is not|never`);
- no form for "X means inadequate training" or for the issue's two sentences
  ("only attended a bootcamp ... lacks the rigor", "given their age ... cannot").

## Scope

In scope: widen the regexes in `BiasDetector.DISMISSIVE_PATTERNS` and
`BiasDetector.DEMOGRAPHIC_PATTERNS` so the 9 xfail tests and the issue's two
example sentences are flagged, keep every currently passing test passing
(including the 9 "not flagged" tests), and remove the 9 `@pytest.mark.xfail`
markers for #58.

Not in scope: changing `detect_bias()`'s control flow or return values,
the reason strings, any ML/NLP approach, other safety modules, and any other
issue's tests. I won't try to catch every possible biased phrasing; the target
is the tested phrasings plus the issue's two examples.

## Files to touch

- `safety/bias_detector.py`: the two pattern lists only.
- `tests/unit/test_bias_detector.py`: remove the 9 xfail markers for #58; add
  one test for each of the issue's two example sentences.

## Approach

1. Edit the patterns one family at a time (subjects, verbs, new forms), running
   `pytest tests/unit/test_bias_detector.py --runxfail` after each edit.
2. Dismissive: allow `(?:coding\s+)?bootcamp|self-taught|online\s+course` as the
   subject, `graduates?|developers?|programmers?` as the noun, and
   `can't|cannot|won't|lack|lacks|missing` as the verb; allow `education|training`
   followed by `lacks` directly; allow `are` as well as `is` before
   `not|never equal|comparable`; add `<bootcamp|self-taught> <noun> means
   inadequate|insufficient`; add `bootcamp ... lacks the rigor`.
3. Demographic: allow plural and `young|younger|old|older|aged` subjects with
   `people|developers|programmers`; allow `people|developers|programmers from
   poor|rich|working-class`; add `given (their|his|her) age ... can't|cannot|won't`.
4. Remove the 9 xfail markers, add the 2 issue-sentence tests.
5. Run `make lint`, `make typecheck`, `make test-unit`.

## Test plan

Re-run my unit 2 repro steps on the branch:

- `detect_bias("The candidate only attended a bootcamp, so this project lacks the rigor of a formal CS education")`
  → expected after: `(True, 'Dismissive language about educational background')` (before: `(False, '')`).
- `detect_bias("Given their age, they likely cannot keep up with modern frameworks")`
  → expected after: `(True, 'Demographic assumptions detected')` (before: `(False, '')`).
- control `detect_bias("bootcamp graduates lack rigor")` → still `(True, ...)`.
- `pytest tests/unit/test_bias_detector.py -q` → expected after: `34 passed`, 0 xfailed
  (before: `23 passed, 9 xfailed`), which includes the 9 "not flagged" tests still passing.
- `make test-unit`, `make lint`, `make typecheck` → no new failures.

## Risks and unknowns

- False positives: wider patterns could flag neutral feedback. The existing
  "not flagged" tests (e.g. "your bootcamp training has given you a solid
  foundation") guard this, but I haven't tested phrasings beyond the suite.
- I haven't confirmed whether ruff or mypy has a suppression in
  `pyproject.toml` for this module; I'll check and remove one if it's tied to #58.
- Classmates have posted plans on this issue; mine is built from my own repro.

## Deviations

The build followed the plan's files, scope, and test plan. Two details differ
from the approach as written:

1. In the `<bootcamp|self-taught> <graduates|developers|programmers> <verb>`
   pattern I dropped the old required object after the verb
   (`rigor|fundamentals|proper training`). Why: `can't`/`cannot`/`won't` are
   followed by an action ("can't write production code", "can't handle
   production systems"), not one of those nouns, so keeping the object would
   still miss 3 of the 9 tests. Effect: "bootcamp graduates lack X" now flags for
   any X. All 9 "not flagged" tests still pass.
2. The new "X means inadequate|insufficient" pattern also accepts
   `online course` as the subject, to match the other dismissive patterns. The
   plan named only `bootcamp|self-taught`.

Nothing else changed: same two files, the 9 xfail markers removed, 2 tests added,
no `pyproject.toml` suppression existed for this module (checked), and the test
plan's expected results all held (see the PR's evidence).
