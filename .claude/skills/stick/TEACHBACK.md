# Teach-back & transcript testing

Testing modalities that grade a *real performance*, not just a chat answer. Two of them
produce a transcript you grade; all four feed the weakness ledger. Testing is the point —
this is where sticking is measured.

## The transcript pipeline

- `C:\Users\Rohith\Videos\subtitle_watcher.py` transcribes videos in
  `C:\Users\Rohith\Videos\` to a `<same-name>.srt` beside each one (faster-whisper
  `large-v3`, CUDA). It **keeps filler words on purpose** (um, uh, like, basically…) — those
  are graded signal for spoken explanation, so don't discard them.
- **Before sending the user off to record, confirm the pipeline is live.** It normally runs
  as a scheduled task `AutoSubtitleWatcher`, but that task may be unregistered — then nothing
  transcribes automatically and a recording produces no `.srt`. Check with
  `schtasks /query /tn AutoSubtitleWatcher` (or just whether recent videos have matching
  `.srt`). If it's off: fall back to written free-recall, or transcribe once on demand —
  `python "C:\Users\Rohith\Videos\subtitle_watcher.py" --process-existing` in the background,
  wait for the sidecar, then stop it. Never write your own transcriber.
- To read a transcript: newest recording `ls -t "/c/Users/Rohith/Videos/"*.srt | head -1`
  (or let the user name it), then strip the index and timestamp lines, keep the text.
- **Handshake, don't poll blindly:** have the user record, then say "done" when it's saved;
  only then look for the `.srt`. If it isn't there, transcribe on demand or switch to written.

## Modalities

### 1. Written free-recall (default)
Same generation-first bet, no recording, zero setup. The user types or pastes an explanation
from memory (a blank-page brain-dump). Grade it (see Grading). This is the default teach-back
— no pipeline dependency; reach for spoken only when the user wants it.

### 2. Spoken teach-back (Feynman)
Explain the concept out loud, as if teaching it, from memory. Highest signal (filler words
expose shaky spots) but highest friction — record → transcribe → return — so offer it as an
opt-in, not the default.
0. **Pre-flight:** confirm the transcript pipeline is live (above). If it's off and the user
   doesn't want to enable it, use written free-recall — never send them to record into a
   dead pipeline.
1. Have the user record themselves explaining `<concept>` (OBS → Videos folder), then say
   "done". Generation-first: no notes, no reveal beforehand.
2. Read the newest `.srt`. Grade against ground truth (see Grading).
3. Report filler-word rate and any hedge-heavy passages — those mark the spots they don't
   actually know, even when the fact is technically present.

### 3. Explain-the-video (ground-truth extraction)
When the user points at a big explainer / system-design video:
1. Get its transcript — its `.srt` if it's in the Videos folder, otherwise have them drop
   the file there or paste the transcript.
2. Build a **ground-truth concept map**: the key claims, terms, and the causal/structural
   links the video makes. This becomes the reference you grade against — save it (a
   `reference/*.html` cheat-sheet or a note in the progress file).
3. Then test the user against that map (spoken, written, or quiz) and mark every node they
   missed, garbled, or couldn't connect.

### 4. Agent quiz
Already the stick default — live scenario/recall Q&A. Interleave it with the above so a
session mixes an old concept with a new one.

## Grading a performance

Grade against ground truth — a reference doc the user supplied, the extracted video map, or
reference files in the workspace. Prefer those over your own memory; if none exist, grade
from your knowledge but say so, so the user knows that part is unverified. Find:

- **Missing** — nodes/claims they never mentioned.
- **Wrong** — stated incorrectly.
- **Vague / hedged** — "sort of a thing that handles stuff": present in words, absent in
  understanding. Filler clusters flag these.
- **Unconnected** — facts recalled but the links between them missing. This is the real
  test of transfer.

Confront the gap between felt fluency and actual performance: when they sounded sure but
missed things, say so. Judge this from the performance — don't ask them to self-rate.

## Weakness ledger — `stick-weaknesses.md`

Lives beside `stick-progress.md` in `~/.stick/`. The running list of specific weak points,
so the next session targets them instead of starting cold.

```md
# Weaknesses

## <Concept>
- [ ] (s7) Confuses X with Y — can't say when to use which
- [ ] (s7) Vague on how Z propagates; heavy filler through that passage
- [x] (s5→s8) Base-case reasoning — resolved, clean teach-back in s8
```

- `(sN)` = session first seen. Tick the box (`[x]`, `sN→sM`) when a later rep clears it.
- Every session: open by reading this file, prioritise still-open items, log new ones after
  grading.
- Cleared items stay as a record of progress — move them to a `## Cleared` section if the
  active list gets long.
