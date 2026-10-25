# Write-up: Multi-Layer Encoded Flag (Base64 x2 + Caesar Cipher)

**Platform:** picoCTF / CyLab
**Category:** Cryptography
**Date:** October 25, 2026
**Author:** Sean

---

## Steps Taken

1. Received the encoded file (`enc_flag`) containing:
   ```
   YidhR3BvYTJ4MFpudHFhR3g2YUhsZmF6TnFlVGwzWVROclh6azJhamN6TnpRMWZRPT0nCg==
   ```
2. Recognized the Base64 character set (letters, numbers, padding `==`) and decoded it with CyberChef's **From Base64**.
3. The output printed as `b'aGpoa2x0ZntqaGx6aHlfazNqeTl3YTNrXzk2ajczNzQ1fQ=='` — noticed the `b'...'` wrapper is just how Python displays raw bytes, not actual data. Copied only the string **inside** the quotes for the next step.
4. Decoded that clean string with a second **From Base64** operation, which gave:
   ```
   hjhkltf{jhlzhy_k3jy9wa3k_96j73745}
   ```
5. Recognized this as a **substitution-style cipher** (letters shuffled, but underscores, braces, and numbers untouched) — a pattern consistent with a Caesar cipher.
6. Used an online Caesar cipher tool, initially set to **Encode** by mistake, which scrambled the text further instead of reversing it. Corrected this by switching to **Decode** mode.
7. Tried different shift values in Decode mode until the output read as plain English with a valid flag format.
8. Found the correct shift and recovered the final flag.

## Tools Used

- CyberChef — double Base64 decode
- Online Caesar cipher tool (Decode mode, shift brute-force by trial)

## Answer

- Flag: `academy{caesar_d3cr9pt3d_96c73745}`

## Lesson Learned

_Write your own lesson here, e.g. the mistake of copying a Python bytes-repr wrapper (b'...') into the next decode step and how that broke the output, or the difference between Encode and Decode mode costing you a wrong first attempt._
