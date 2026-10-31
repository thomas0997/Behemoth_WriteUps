# ASCII Numbers

- **Platform:** picoCTF
- **Difficulty:** Easy
- **Date completed:** 2026-10-31
- **Category:** General Skills

## Summary
A string of hex-encoded ASCII byte values needs decoding back into readable text — the flag itself.

## Steps
1. Recognized the format immediately: a space-separated list of `0x..` hex values, each representing one ASCII character's byte code.
2. Used CyberChef's "From Hex" operation (via the "Bake" button) to decode the whole string in one pass rather than converting byte-by-byte by hand.
3. The decoded output was the flag directly — no further steps needed.

## Tools used
- CyberChef — "From Hex" recipe to decode the ASCII byte values in bulk

## Lesson learned
`0x` is the standard prefix for hex notation, and a string of space-separated hex pairs is almost always ASCII-encoded text waiting to be converted — CyberChef's "From Hex" handles this in one step instead of manually converting each byte. Good first instinct: recognize the encoding format before reaching for any tool.

## Flag
```
academy{45c11_n0_qu35710n5_1ll_t311_y3_n0_l135_e96c8ca8}
```