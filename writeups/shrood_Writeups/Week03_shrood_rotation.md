# Rotation

- **Platform:** picoCTF 2023
- **Difficulty:** Medium
- **Date completed:** 2026-10-15
- **Category:** Cryptography

## Summary
A Caesar cipher challenge — despite the "Medium" label, it was a straightforward brute-force rotation problem once the ciphertext was in hand.

## Steps
1. Downloaded the encrypted flag file and read it:
   ```
   cat encrypted.txt
   ```
   Output: `ikilmug{z0bib1wv_l3kzgxb3l_69m10560}`
2. Recognized the structure (`{...}` wrapper, underscores) as a flag format just letter-shifted, same as a Caesar cipher.
3. Used an online Caesar cipher rotation tool to try every shift from 1 to 26.
4. At shift +18, the ciphertext decoded cleanly into a readable flag.

## Tools used
- `cat` — read the encrypted file
- An online Caesar cipher brute-force tool — tried all 26 possible rotations

## Lesson learned
A Caesar cipher only has 26 possible shifts, so brute-forcing every rotation by eye (or with a quick online tool) is always faster than trying to calculate the shift manually — no need to overthink a "Medium"-rated challenge when the structure is this simple.

## Flag
```
academy{r0tat1on_d3crypt3d_69e10560}
```