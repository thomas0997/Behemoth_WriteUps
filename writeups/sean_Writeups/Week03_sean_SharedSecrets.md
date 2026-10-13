# Write-up: Shared Secrets

**Platform:** picoCTF 2026
**Category:** Cryptography Shared Secrets(Easy)
**Date:** October 14, 2026
**Author:** Sean

---

## Steps Taken

1. Opened the challenge and read the description: a message was encrypted using a Diffie-Hellman shared secret, but "one side of the exchange leaked something."
2. Downloaded both provided files: the **source code** (`encryption.py`) and the **message** (`message.txt`).
3. Read `message.txt`, which contained the public DH parameters and the ciphertext:
   ```
   g, p, A, b, enc
   ```
4. Read `encryption.py` to understand exactly how the server generated these values and performed the encryption.
5. **Found the vulnerability:** the script writes out `b` — the client's actual **private** exponent — into `message.txt`, instead of the public value `B = pow(g, b, p)` that a real Diffie-Hellman exchange should expose. This is the "leak" the challenge title refers to.
6. Because the real private `b` was exposed, the shared secret could be computed directly with no need to solve a discrete logarithm:
   ```python
   shared = pow(A, b, p)
   ```
   This matches the server's own computation in the source (`shared = pow(A, b, p)`).
7. Followed the source code's encryption step in reverse to decrypt:
   ```python
   key_byte = shared % 256
   enc_bytes = bytes.fromhex(enc_hex)
   flag = bytes([x ^ key_byte for x in enc_bytes])
   print(flag)
   ```
8. Wrote this script in a Python file, ran it, and copied the printed output into a separate file to extract the plaintext flag.

## Tools Used

- Python 3 (`pow()`, `bytes.fromhex()`, list comprehension with XOR) — to compute the shared secret and reverse the XOR encryption
- Source code review — reading the challenge's own `encryption.py` to understand the exact encrypt/decrypt logic instead of guessing

## Answer

- Flag: `academy{dh_s3cr3t_441eb9ee}`

## Lesson Learned

_Write your own lesson here, e.g. why leaking a private DH exponent breaks the whole exchange (it skips the "hard" discrete log problem entirely), or the difference between a public value (g^secret mod p) and the raw private secret itself._