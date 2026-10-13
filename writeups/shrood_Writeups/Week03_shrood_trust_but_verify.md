# Trust But Verify

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Date completed:** 2026-10-13
- **Category:** OSINT / Other

## Summary
An AI-ethics interactive fiction delivered over netcat, not a technical challenge. You play through a story where an AI assistant (ARIA) feeds the main character a bad statistic, buggy code, and an almost-correct citation, and you pick the "verify it" option each time to see why that matters.

## Steps
1. Connected to the challenge with `nc chatelaine.cylabacademy.net 36288`.
2. Read through the story, pressing Enter to advance each screen.
3. Scene 1: ARIA gives a wildly wrong ocean-plastic statistic stated with full confidence. Chose `c) Look it up independently first` — the real figure was off by a factor of ~50 from what ARIA claimed.
4. Scene 2: ARIA writes a Python script with a subtle unexplained `+ 1` bug in an average calculation. Chose `b) Read through it carefully before running` — caught the bug by asking ARIA to explain the extra term.
5. Scene 3: ARIA gives a citation that's mostly real (real researcher, real university) but has the year and the strength of the finding wrong. Chose `b) Verify it anyway` — confirmed the study existed but corrected the year and toned down "confirmed" to "preliminary."
6. At the end of the story, ARIA hands over the flag directly.

## Tools used
- `nc` (netcat) — connected to the interactive fiction server

## Lesson learned
The dangerous AI mistake isn't the obviously wrong answer, it's the plausible one — a confident number, code that still runs, a citation that's 90% right. "Trust but verify" means checking outputs *before* you know which ones are wrong, not just the ones that feel off.

## Flag
```
academy{7ru57_15_34rn3d_2fd6fcdc}
```