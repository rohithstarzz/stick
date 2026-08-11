# stick-progress.md Format

The spaced-repetition queue: it tells you what's due and tracks whether each concept is
sticking. Keep everything in this one file. It lives in the canonical store `~/.stick/`, so
one queue spans every folder regardless of where you run `/stick` from.

## Template

```md
# Stick Progress

Session: 7           <!-- bump +1 only on the first run of a new calendar day -->
Last run: 2026-07-15 <!-- date of the run that set the current Session -->

## Review Queue
| Concept | Last session | Rung | Score (correct/total) | Due at session |
|---------|--------------|------|-----------------------|----------------|
| Recursion base cases | 5 | 4 | 3/4 | 9 |
```

- **Session**: a counter that advances at most once per calendar day — the first `/stick`
  run of a new day bumps it; later runs the same day don't. `Last run` records the date of
  the run that set the current value, so you can tell whether today is new. Spacing is in
  sessions (≈ days), so an irregular schedule still works.
- **Last session** / **Due at session**: the session number, not a date.
- **Rung**: the current interval gap in sessions, from the ladder below (1, 2, 4, 8, or 16).
  `Due = Last session + Rung`. Stored explicitly so advancement isn't reverse-engineered
  from Due − Last.
- **Score**: cumulative correct/total across reps for that concept. Judged by you from the
  attempt — the user is never asked to self-rate.

## Scheduling ladder

Interval ladder per concept — the **Rung** value (gap in sessions): `1 → 2 → 4 → 8 → 16`.

- Clean, unaided recall → advance to the next rung (e.g. 4 → 8; Due = current session + new rung).
- Wrong, or only recalled with heavy prompting → reset to rung 1 (Due = current session + 1).
- Partial recall → keep the current rung (Due = current session + same rung).

<!-- ponytail: spacing in sessions ≈ days (one session per calendar day), not wall-clock
     hours. Good enough for an irregular login schedule; swap to real timestamps only if
     you need intra-day spacing, which deliberate practice doesn't. -->

## Optional

If the user is drilling many concepts, add a `## Retired` section for concepts that have
reached the 1mo rung and stuck — keeps the active queue readable.
