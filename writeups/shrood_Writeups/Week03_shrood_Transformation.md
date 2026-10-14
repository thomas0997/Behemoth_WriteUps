# Transformation

- **Platform:** picoCTF 2021
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Reverse Engineering

## Summary
A challenge that gives you the encoding logic itself (not the flag) and an encoded output file — the goal is to read the Python one-liner, understand what transformation it applies, and write the inverse to recover the flag.

## Steps
1. Opened the challenge file `enc` and found a garbled block of CJK-looking characters:
   ```
   慣慤敭祻ㄶ形楴獟楮獴㌴摟潦弸強㤰扡㌷敽
   ```
2. Read the provided encoding source:
   ```python
   ''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])
   ```
3. Broke down what it does: steps through the flag two characters at a time, takes the first character's byte value, shifts it left by 8 bits (into the "high byte" of a 16-bit number), adds the second character's byte value into the low 8 bits, then converts that combined 16-bit value back into a single (wide) character. The result packs every 2 original characters into 1 output character — which is exactly why the output displayed as unfamiliar CJK-range glyphs instead of plain text.
4. Wrote the inverse: for each wide character in the encoded string, extract the high byte (`>> 8`) to recover the first original character, and the low byte (`& 0xFF`) to recover the second original character.
   ```python
   s = '慣慤敭祻ㄶ形楴獟楮獴㌴摟潦弸強㤰扡㌷敽'
   out = ''
   for c in s:
       code = ord(c)
       high = (code >> 8) & 0xFF
       low = code & 0xFF
       out += chr(high) + chr(low)
   print(out)
   ```
5. Ran it and recovered the flag.

## Tools used
- Python — wrote the inverse of the given encoding function to decode the flag

## Lesson learned
When a challenge hands you the forward transformation directly, the task isn't decoding a mystery cipher — it's just carefully reading what each operation does (bit shift, mask, combine) and writing the exact opposite sequence of operations in reverse order. Bit-packing two characters into one wide character is a common trick for making normal text look unreadable without actually encrypting it.

## Flag
```
academy{16_bits_inst34d_of_8_790ba37e}
```