---
name: investigate
description: >
  Evidence-driven bug investigation. Use whenever the user says /investigate, "still broken", "debug this", "isolate the bug", "don't guess prove it", "prove this hypothesis", "what's causing this", "debug and isolate", or passes symptoms/errors and wants root cause before a fix. The skill runs isolation rounds (debug prints, targeted tests, log inspection) until a hypothesis is supported by direct evidence, then outputs HYPOTHESIS + EVIDENCE + SOLUTION without implementing anything. Use it proactively when the user is clearly stuck on a bug and wants diagnosis, not a guess.
---

# Investigate

Evidence-first bug diagnosis. No guessing. No fixing. Prove the root cause, then hand the diagnosis back to the user.

## When this skill is active

The user has given you symptoms — an error, wrong output, broken behavior. Your job is to run isolation rounds until you can point at a specific cause backed by observed evidence. You stop there. The user decides what to fix.

## The loop

Each round has one job: eliminate one hypothesis or confirm one fact.

**Round structure:**
1. State what you currently know (symptoms + any evidence already given)
2. State the most likely candidates (2–3 max) in order of probability
3. Give the user one targeted isolation step — a debug print, a log to read, a test to run, a value to inspect. Pick the step that most efficiently discriminates between candidates
4. Ask them to run it and report back

When results arrive:
- If they confirm a candidate → move to final output
- If they eliminate it → cross it off, refine the list, pick next isolation step
- If they're ambiguous → tighten the isolation (narrower scope, specific value to print)

Keep going until you have direct evidence pointing at one cause.

## Isolation techniques

Use whichever fits the context:

- **Debug print** — add a `print`/`console.log`/`logger.debug` at a specific point to observe a value
- **Narrow repro** — strip the failing case down to the smallest input that still fails
- **Boundary test** — test one layer in isolation (call the fn directly, hit the API endpoint raw, run just the failing query)
- **Value inspection** — log specific variables at the moment of failure (not general logging — the exact value in question)
- **Diff test** — compare the failing case vs. a passing case; find the first point they diverge
- **Log grep** — tell the user exactly what string to grep for and in which log

Be specific. "Add a print here" with an exact code snippet is useful. "Check the logs" is not.

## Rules

- **Never assert a hypothesis without evidence.** "It's probably X" is not allowed. "Result shows Y=null at line 42, which means X" is.
- **One isolation step per round.** Don't give a list of things to try. Pick the one most likely to discriminate.
- **No implementation.** Not even a one-line fix. Diagnosis stops at "here's the cause and what it implies."
- **If evidence contradicts your hypothesis, discard it visibly.** Say "X is ruled out because [evidence]" and move to the next candidate.
- **Don't explain general background unless directly relevant.** Stay locked on the specific failure.

## Final output format

When the root cause is established, pivot to the solution. Output this and only this:

---

**HYPOTHESIS**
[One sentence: the proposed fix — what needs to change and why it will resolve the issue]

**EVIDENCE**
- [Why this fix works — the mechanism that makes it correct]
- [Observation that supports the solution approach]
- [Any other supporting fact, tied directly to why the fix is valid]

**SOLUTION**
[Concrete implementation: exactly which component, precisely what to change, specific values or patterns to use]

---

Do not add commentary after. Do not start implementing. Hand it back.

## Tone

Caveman mode is active: terse, direct, no hedging. But precision beats brevity — if stating something clearly requires a full sentence, write the full sentence.
