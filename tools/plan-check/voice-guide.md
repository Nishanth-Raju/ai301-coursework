# Voice guide: how I talk upstream

## Who I am in threads

I'm a student in CodePath AI301, making my first contributions to Path
Review. I work on Windows with Git Bash, and I use Claude Code as an
assistant. Readers can expect me to post what I actually ran and saw,
say plainly when I'm unsure, and not promise more than I've done.

## Rules I write by

### Rule: Show the run, don't vouch for it

Every "reproduced" or "confirmed" sits right above the command and output
that prove it. If I can't paste the output, I don't use the word.

- Wrong: "Confirmed, the test is broken on my machine too."
- Right: "Ran `pytest tests/unit/test_readme_scorer.py -q --runxfail` at `2f4e82f`; it fails with `assert 51 > 100` (output below)."

### Rule: Promise the next step, not the finish line

A claim says what I'll look at next. It never promises a fix, a PR, or
a date, because I don't know yet how big the work is.

- Wrong: "I'll have a PR up for this by Friday."
- Right: "I'd like to work on this. First I'll check which word count the second assertion actually needs."

### Rule: Label guesses as guesses

When I explain why something happens, I either point at the line of
code or the output that shows it, or I write "I think" or "my guess".

- Wrong: "The scorer's thresholds are wrong."
- Right: "From `readme_scorer.py` lines 68-73, `comprehensive` starts at 500 words, so I think the fixture, not the scorer, is what needs to change."

### Rule: Say what differed

If my setup differs from the documented one (Windows instead of
macOS/Linux, skipping Docker, a different Python), I say so in the
comment instead of hoping it doesn't matter.

- Wrong: "Set up the project per the docs and ran the tests."
- Right: "Windows 11 + Git Bash, Python 3.12.6. I skipped `docker compose` and the DB steps of `make setup` because this unit test doesn't touch the database."

### Rule: Name the tool when the tool did the work

If Claude Code helped me write the comment or analyze the output, and
the repo asks for disclosure, I say so in one plain sentence. I never
post AI text I haven't read, run, and checked.

- Wrong: (posting an AI-drafted analysis with no mention and without re-running it)
- Right: "I used Claude Code to help draft this; I ran every command above myself and checked the output."

### Rule (plan register): Propose, don't announce

A plan comment commits me to an approach in front of the people who
maintain the code. I state it as what I intend to do and invite
correction, and I name the one thing I'm least sure of.

- Wrong: "The fix is to rewrite the fixture. PR incoming."
- Right: "My plan is to extend the fixture to 500+ words so both assertions hold. If you'd rather the test assert `adequate` instead, I'll switch to that."

### Rule (plan register): Say what I'm not touching

Every plan comment names at least one thing that stays out of scope,
so a reviewer can see the change is bounded before reading the diff.

- Wrong: "I'll clean up the scorer tests while I'm in there."
- Right: "I'm not changing the scorer's thresholds or any other test."

## Things I never post

- A fix date, a "PR shortly", or "should be easy".
- "+1", "same here", or "can confirm" with nothing attached.
- A root cause stated as fact when I haven't shown it.
- Output I didn't run myself, or output trimmed so it looks like a different error.
- Pressure on maintainers: "please prioritize", "this is urgent", "any update?" after a day.
- A plan that piggybacks on someone else's ("same approach as above").
