# Shift Registers

- **Platform:** picoCTF 2026
- **Difficulty:** Medium
- **Date completed:** 2026-10-15
- **Category:** Cryptography

## Summary
A custom LFSR (Linear Feedback Shift Register) stream cipher challenge. The script looked intimidating at first, but the actual flaw was a tiny keyspace: the key was masked down to a single byte before it was ever used.

## Steps
1. Downloaded `chall.py` and `output.txt` (ciphertext: `30cbb50e507774fdafd01e0bfdbc38d5eadf92991cdd347064e37444e517d661cea9`).
2. Ran `chall.py` directly to see what it produced — got output that looked like layered/multiple encryption, but running the ciphertext back through it led nowhere.
3. Tried feeding the `output.txt` ciphertext into the script directly — no result.
4. Ran it through a hash identifier — came back with nothing, confirming it wasn't a standard hash.
5. Read through `chall.py` properly and found the actual bug: `lfsr = key & 0xFF`. Even though the script generates a huge 126-byte random key, only its lowest 8 bits are ever used to seed the LFSR — meaning there are only 256 possible starting states, regardless of key size.
6. Used AI assistance to write a brute-force script: loop through all 256 possible seeds, replicate the challenge's own `steplfsr` function exactly, and XOR each generated keystream against the ciphertext to get a candidate plaintext.
7. Wrote the script into a file with `nano`, ran it, and filtered results for printable/readable output.
8. One seed produced a clean, readable flag.

## Tools used
- Python — replicated the LFSR stepping logic and brute-forced all 256 possible seeds
- `nano` — wrote the cracking script on the Kali VM
- A hash identifier tool — ruled out the ciphertext being a standard hash first
- AI assistance — used to help write the brute-force script after manual analysis stalled

## Lesson learned
A big random key means nothing if the code masks it down before use — always check exactly how much of a "secret" value actually makes it into the algorithm, not just its stated size. An 8-bit effective keyspace (256 options) is trivially brute-forceable, no matter how large the key looks on paper.

## Flag
```
academy{l1n3ar_f33dback_sh1ft_r3g}
```