# convertme.py
- **FILE NAME FORMAT** Week01_dein_convertme.py
- **Platform:** picoCTF
- **Difficulty:** Easy
- **Date completed:** 2026-10-02
- **Category:** Other

## Summary
A beginner picoMini 2022 General Skills challenge by LT 'syreal' Jones. A Python script asks for the binary form of a given decimal number, and answering correctly reveals the flag.

## Steps
1. Downloaded the provided Python script from the challenge page.
2. Opened and ran the script in VS Code.
3. The script prompted me to enter the binary value of the decimal number `52`.
4. Converted 52 from decimal to binary using an online decimal-to-binary converter, which gave `00110100`.
5. Entered `00110100` at the prompt. The script replied "That is correct! Here's your flag" and printed `academy{4ll_y0ur_b4535_fff0af3d}`.

## Tools used
- VS Code: to open and run the downloaded Python script
- Online decimal-to-binary converter: to convert 52 to binary

## Lesson learned
Practiced decimal-to-binary conversion and running a challenge script locally. Next time I would work out the binary by hand (52 = 32 + 16 + 4, so `110100`) instead of relying on an online converter.