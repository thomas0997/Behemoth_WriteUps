# Write-up: Number Cipher (A1Z26)

**Platform:** picoCTF / CyLab
**Category:** Cryptography
**Date:** October 25, 2026
**Author:** Sean

---

## Steps Taken

1. Received an image containing a sequence of numbers:
   ```
   16 9 3 15 3 20 6 { 20 8 5 14 21 13 2 5 18 19 13 1 19 15 14 }
   ```
2. Recognized the pattern as an **A1Z26 cipher** — a number-to-letter substitution where each number represents a letter's position in the alphabet (A=1, B=2, C=3 ... Z=26).
3. Mapped each number to its corresponding letter by hand using the A-Z reference:
   ```
   A=1  B=2  C=3  D=4  E=5  F=6  G=7  H=8  I=9  J=10
   K=11 L=12 M=13 N=14 O=15 P=16 Q=17 R=18 S=19 T=20
   U=21 V=22 W=23 X=24 Y=25 Z=26
   ```
4. Double-checked the letter count against the number count to catch any skipped or duplicated letters.

## Tools Used

- A-Z number reference chart — manual substitution, no digital tool needed for this one

## Answer

- Flag: _fill in your corrected answer here after rechecking the mapping_

## Lesson Learned

_Write your own lesson here, e.g. how to recognize A1Z26 by its number sequence format, or what helped you double-check your letter-by-letter mapping for mistakes._
