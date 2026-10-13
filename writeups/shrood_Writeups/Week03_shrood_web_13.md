# 13

- **Platform:** picoCTF 2019
- **Difficulty:** Easy
- **Date completed:** 2026-10-14
- **Category:** Crypto

## Summary
A straightforward ROT13 cipher challenge — the ciphertext `Npnqrzl{abg_gbb_onq_bs_n_ceboyrz}` decodes to the flag by shifting each letter 13 places.

## Steps
1. Looked at the ciphertext `Npnqrzl{abg_gbb_onq_bs_n_ceboyrz}` and immediately recognized it as a Caesar cipher shifted by 13 (ROT13) — the structure (braces, underscores) matched a flag format, just letter-shifted.
2. Decoded it in the Kali terminal using `tr`:
   ```
   echo "Npnqrzl{abg_gbb_onq_bs_n_ceboyrz}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'
   ```
3. Got the flag back in plaintext.

## Tools used
- `tr` — performed the ROT13 letter substitution directly in the terminal

## Lesson learned
ROT13 is its own inverse (applying it twice returns the original text), so the same `tr 'A-Za-z' 'N-ZA-Mn-za-m'` command both encodes and decodes it — no need for a separate decode mapping.

## Flag
```
Academy{not_too_bad_of_a_problem}
```