# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a student in CodePath AI301 making a first contribution to Path
Review. I work with an AI coding assistant (Claude Code) and I review and
run everything myself before I post. Readers can expect exact commands,
real output, and plain statements of what I did and did not see.

## Rules I write by

### Rule: Name the specifics

Every comment names the function, file, input, or error from this issue,
so it could not be pasted onto a different issue.

- Wrong: "Hi, I'd like to work on this issue, please assign me."
- Right: "I'd like to work on this: I'll reproduce the `ZeroDivisionError` in `keyword_search` with an empty index and post the output here."

### Rule: Promise the investigation, not the fix

I commit to the next thing I'll actually do (reproduce, report back),
never to a fix, a result, or a date.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'll post a reproduction report with my environment and output next."

### Rule: Show, then say

Any claim ("reproduced", "the cause is") points at output pasted in the
same comment. If I only suspect something, I say "I suspect."

- Wrong: "Confirmed, the bug is definitely in the parser."
- Right: "Reproduced on `main` at `abc1234`; output below. I suspect the fallback branch in `parse_output`, but haven't confirmed that."

### Rule: Disclose the AI help

When I used Claude Code to draft or investigate, I say so in one line
naming the tool and what it did.

- Wrong: (no mention of AI help)
- Right: "AI disclosure: I used Claude Code to help draft this and run the repro; I reviewed and re-ran every command myself."

### Rule: Say how sure I am about the approach

A plan states the approach I'll take and marks what I haven't confirmed
as an unknown, and it engages any direction a maintainer already gave
in the thread instead of ignoring it.

- Wrong: "The fix is obviously to rewrite the detector with an NLP model."
- Right: "I plan to widen the regex patterns in `bias_detector.py` to cover the 9 tested phrasings; I haven't yet checked whether the wider patterns cause false positives on neutral feedback, so that's in my test plan."

## Things I never post

- "+1", "same here", or "same as above, can confirm" with no evidence.
- A deadline or a promise that a fix will be merged.
- "Guaranteed", "definitely", or "100%" about anything I have not shown.
- Pasted AI output I have not read and checked against my own run.
- Apologies or filler ("sorry to bother", "hope this helps!!").
- A plan that grows past the issue ("while I'm in there I'll also refactor...").
