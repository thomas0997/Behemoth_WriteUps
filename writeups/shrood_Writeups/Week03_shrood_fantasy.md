# FANTASY CTF

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** General Skills

## Summary
A narrative/interactive-fiction "sanity check" challenge, run entirely through `netcat`, that walks through CTF etiquette (don't share accounts, don't share flags/artifacts with other players) via a series of in-universe dialogue choices. The flag is handed to the player directly in the story text once the choices are made correctly — no exploitation involved.

## Steps
1. Connected to the challenge instance with netcat:
   ```
   nc chatelaine.cylabacademy.net 48050
   ```
2. Read through the story prompts, pressing Enter to advance each screen.
3. At each `[a/b/c] >` or `[a/b] >` prompt, picked the choice that matched good CTF practice (per the in-game AI character's own hints), rather than the "wrong"/rule-breaking option:
   - Registration type: chose `c` — *Register a single, private account* (not sharing/multi-accounting).
   - When first asked to either play the game or "search the Ether for the flag," chose `b` first to see the in-fiction warning against submitting someone else's flag, then correctly chose `a` — *Play the game* — to actually earn the flag honestly.
4. After "playing" the simulated game (a scripted progress-bar animation), the character announces finding the flag and states it directly in the dialogue.

## Tools used
- `nc` (netcat) — connected to and interacted with the challenge's interactive text adventure

## Lesson learned
Not every challenge involves technical exploitation — some (especially "sanity check"/intro challenges) are testing whether you follow the stated rules and walkthrough correctly. Here, choosing the rule-respecting path (single account, don't scrape/share flags) was itself the intended solution, with the flag revealed as a reward for following the narrative properly rather than by any bypass.

## Flag
```
academy{m1113n1um_3d1710n_2ad04d2b}
```