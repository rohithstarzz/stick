# /stick — a Claude Code skill for making things actually stick

`/stick` is a [Claude Code](https://claude.com/claude-code) skill that turns any concept you
want to learn into an active practice session instead of a reading session. It's built
directly on the research summarized in *[Make It Stick: The Science of Successful
Learning](https://www.hup.harvard.edu/books/9780674729018)* (Brown, Roediger, McDaniel,
2014) — one of the most well-supported books on learning science, drawing on decades of
cognitive psychology research on memory and retrieval.

## The problem it solves

Rereading notes, highlighting text, and watching tutorials all *feel* productive. They
create a sense of fluency — "yeah, I get this" — that fades within days, because fluency and
learning are not the same thing. The research is blunt about this: **the things that feel
best while you're doing them (rereading, massed practice, highlighting) are the ones that
produce the weakest long-term memory.** The things that feel effortful and frustrating in
the moment (retrieval practice, spacing, interleaving) are the ones that actually last.

`/stick` exists to force the effortful version, because a chat model is uniquely suited to
run it: it can generate a fresh quiz, a scenario, or a teach-back prompt every time, watch
you struggle, catch when you're bluffing, and hold a spaced-repetition queue across
sessions — the parts of learning science that are hardest to do alone.

## The science behind it

Eight methods from the book, all implemented in the skill's behavior (see
[`PRINCIPLES.md`](.claude/skills/stick/PRINCIPLES.md)):

1. **Retrieval practice** — recalling a fact strengthens memory far more than re-reading
   it. The struggle to retrieve *is* the learning event, not a test of it.
2. **Spaced repetition** — revisiting a concept after a delay (not immediately) makes the
   next retrieval harder, and that difficulty is what strengthens the memory. Drives the
   skill's review queue.
3. **Interleaving** — mixing different concepts in one session instead of blocking one
   until mastered. Feels worse in the moment, builds the ability to *pick* the right tool
   under ambiguity — which is the actual real-world skill.
4. **Generation** — attempting an answer before being shown one, even incorrectly, primes
   the brain to absorb the correct answer when it arrives.
5. **Elaboration** — explaining something in your own words and connecting it to what you
   already know creates more retrieval paths back to it.
6. **Reflection & calibration** — confronting the gap between "this felt easy" and "I could
   actually reproduce this," judged from your attempt rather than a self-reported confidence
   score (self-ratings are notoriously unreliable).
7. **Desirable difficulties** — learning that feels effortless is usually not sticking.
   Difficulty you can overcome, not difficulty that defeats you, is the target.
8. **Concrete examples & varied practice** — abstract ideas anchor to one vivid concrete
   case, then get tested on a *different* case so the concept transfers instead of staying
   welded to the first example.

## What a session looks like

- **Simulator** — for decision-heavy or procedural concepts, you're dropped into a
  scenario, given a decision, shown the consequence, and branched. Can emit a
  self-contained offline HTML page for scenarios worth revisiting.
- **Retrieval quiz** — for recall/definitions/facts: asked, not told.
- **Concrete story / analogy** — for abstract or slippery concepts: anchored to one vivid
  case, then transfer-tested on a different one.
- **Teach-back** — you explain the concept from memory (written by default; spoken
  teach-back if you have a transcription pipeline available) and get graded on a real
  performance, the strongest test of understanding there is.

Every rep follows the same non-negotiables: you attempt before anything is revealed, the
model watches for fluent-but-wrong or hesitant-but-right answers (the "illusion of
knowing"), feedback is immediate, and the difficulty is real.

Progress and a running list of your weak points persist across sessions in `~/.stick/`, so
each session picks up where the last one left off and interleaves due reviews with new
material — see [`PROGRESS-FORMAT.md`](.claude/skills/stick/PROGRESS-FORMAT.md) and
[`TEACHBACK.md`](.claude/skills/stick/TEACHBACK.md).

## Install

Copy the `stick/` folder into your Claude Code skills directory:

```sh
git clone https://github.com/rohithstarzz/stick.git
cp -r stick/.claude/skills/stick ~/.claude/skills/stick
```

Then invoke it with `/stick` in Claude Code, optionally naming a concept: `/stick recursion
base cases`.

## Why this fits a coding agent well

This isn't a flashcard app bolted onto a chatbot. A coding agent can read your actual code,
your actual notes, or a video you point it at, generate a scenario grounded in that specific
material, track a spaced queue across arbitrarily many concepts without user upkeep, and
notice patterns in *how* you're failing (not just whether). That's the gap `/stick` is built
to fill.

## License

MIT
