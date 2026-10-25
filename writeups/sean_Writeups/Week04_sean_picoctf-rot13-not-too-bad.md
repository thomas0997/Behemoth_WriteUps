# Write-up: ROT13 Flag Decode

**Platform:** picoCTF 2019 (by Alex Putton / Daniel Tunitis)
**Category:** Cryptography
**Date:** October 25, 2026
**Author:** Sean

---

## Steps Taken

1. Opened the challenge: "Cryptography can be easy, do you know what ROT13 is?" with the encoded string:
   ```
   npnqrzl{abg_gbb_onq_bs_n_ceboyrz}
   ```
2. Recognized the pattern as **ROT13** — mixed-case letters shifted, underscores and braces untouched.
3. Decoded it using an online ROT13 tool, as the hint suggested ("This can be solved online if you don't want to do it by hand").
4. Got readable output on the first decode.

## Tools Used

- Online ROT13 decoder (rot13.com / CyberChef ROT13 operation)

## Answer

- Flag: `academy{not_too_bad_of_a_problem}`

## Lesson Learned

_Write your own lesson here, e.g. how quickly you could recognize ROT13 by now after doing several of these, or when it's worth using an online tool vs decoding by hand._
