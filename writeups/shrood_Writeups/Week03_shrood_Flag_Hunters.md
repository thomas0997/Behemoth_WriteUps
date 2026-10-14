# Flag Hunters

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Reverse Engineering

## Summary
A custom mini-interpreter built in Python plays a song by jumping between labeled sections (verses, a refrain, and crowd-input prompts) like a simple bytecode VM. The flag lives in an intro section that's never reached by normal execution — reaching it requires exploiting how user input gets spliced back into the script as executable instructions.

## Steps
1. Downloaded `lyric-reader.py` and read through the source to understand the custom interpreter (`reader()` function).
2. Mapped out the script's structure by simulating it locally with a placeholder flag:
   - Lines 0–3: a hidden "secret intro" section ending in `"The ether's ours to conquer, " + flag`
   - Line 6: the `[REFRAIN]` label
   - Line 11: a `CROWD (Singalong here!);` prompt
   - Line 14: `[VERSE1]`, where normal execution actually starts — meaning the intro (and the flag) is never printed by default.
3. Found the vulnerability in how the `CROWD` prompt is handled:
   ```python
   elif re.match(r"CROWD.*", line):
       crowd = input('Crowd: ')
       song_lines[lip] = 'Crowd: ' + crowd
       lip += 1
   ```
   Whatever you type at the prompt permanently overwrites that line in the script's own source — and since `[REFRAIN]` gets revisited multiple times later in the song, that overwritten line gets re-parsed as a script instruction on the next pass, not just displayed as text.
4. Noticed the interpreter splits each line on `;` and processes every resulting segment in the same pass, letting a later segment's jump instruction override an earlier one.
5. Crafted the payload `;RETURN 0` for the crowd input. This overwrites the CROWD line with `'Crowd: ;RETURN 0'`, which splits into `['Crowd: ', 'RETURN 0']` — the first part prints harmlessly, the second matches the `RETURN [0-9]+` pattern and sets the line pointer back to 0.
6. Connected to the real challenge instance and ran the exploit live:
   ```
   nc xebec.cylabacademy.net 45116
   ```
   Let it play through Verse 1 to the first `Crowd:` prompt, entered `;RETURN 0`.
7. On the next refrain loop, execution jumped back to line 0 and printed the hidden intro — including the flag, embedded inline at the end of the last intro line.
8. The jump point is permanent, so the song loops forever from here on — hit `Ctrl+C` once the flag is visible.

## Tools used
- Python — read and simulated the interpreter's line-indexing logic locally (with a placeholder flag) to find the exact exploit payload before running it for real
- `nc` (netcat) — connected to and interacted with the live challenge instance

## Lesson learned
Treating user input as trusted enough to literally overwrite your own program's "source" (even a toy interpreter's line array) turns any later re-execution of that line into an injection point. The semicolon-delimited multi-instruction-per-line design made it worse: a single crafted input could smuggle a jump instruction past the "Crowd: " prefix by placing it in a later split segment, which the loop processes clean and unprefixed.

## Flag
```
academy{70637h3r_f0r3v3r_697ad4d5}
```