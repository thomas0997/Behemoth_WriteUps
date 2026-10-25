# Write-up: ROT13 Flag Decode

**Platform:** picoCTF / CyLab
**Category:** Cryptography
**Date:** October 25, 2026
**Author:** Sean

---

## Steps Taken

1. Received the encoded string:
   ```
   npnqrzl{arkg_gvzr_V'yy_gel_2_ebhaqf_bs_ebg13_5p5s5o36}
   ```
2. Recognized the pattern as **ROT13** — mixed-case letters shifted, with underscores, apostrophes, and numbers left untouched (a signature of ROT13 since it only shifts alphabetic characters).
3. Decoded it using rot13.com (pasted the string in, applied ROT13 once).
4. The output was fully readable on the first pass — a single round of ROT13 was enough to decode it.

## Tools Used

- rot13.com — quick single-purpose ROT13 decoder

## Answer

- Flag: `academy{next_time_I'll_try_2_rounds_of_rot13_5c5f5b36}`

## Lesson Learned

_Write your own lesson here, e.g. how to recognize ROT13 by its letter-shift pattern without touching punctuation/numbers, or why the flag text jokes about "2 rounds" even though this instance only needed one._
