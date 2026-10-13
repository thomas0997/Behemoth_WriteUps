# Looking For The Secret

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy/Medium
- **Date completed:** 2026-10-14
- **Category:** Crypto

## Summary
A netcat challenge that hands you a chain of hashes one at a time — identify each hash type, crack it to get a password, submit that password to unlock the next hash in the chain, until the final crack reveals the flag.

## Steps
1. Connected to the challenge: `nc xebec.cylabacademy.net 34337`.
2. Got handed the first hash: `482c811da5d5b4bc6d497ffa98491e38`.
3. Guessed `base16` and `sha256` first — both wrong.
4. Correctly identified it as MD5 and cracked it with an online MD5 decrypt tool → plaintext `password123`.
5. Submitted `password123`, which confirmed the MD5 crack and revealed the next hash.
6. Ran that hash through `hashid` and `hash-identifier` in Kali — came back as SHA-1. Cracked it via an online MD5/SHA decrypt tool → plaintext `letmein`.
7. Submitted `letmein`, which revealed a third hash — identified as SHA-256.
8. Cracked the SHA-256 hash using an online cracking tool (10015.io) → plaintext `qwerty098`.
9. Submitted `qwerty098`, and the server returned the flag directly.

## Tools used
- `nc` (netcat) — connected to and interacted with the challenge server
- `hashid` / `hash-identifier` — identified hash types (MD5, SHA-1, SHA-256) when unsure
- dcode.fr — initial (incorrect) guess at hash type
- Online MD5/SHA decrypt tools, 10015.io — cracked each hash to recover its plaintext password

## Lesson learned
Weak, common passwords (`password123`, `letmein`, `qwerty098`) are crackable in seconds regardless of which hash algorithm protects them — hashing isn't a substitute for a strong password, it only slows down a weak one.

## Flag
```
academy{UseStr0nG_h@shEs_&PaSswDs!_a8894b1b}
```