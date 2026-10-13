# Verify

- **Platform:** picoCTF 2024 (CyLab Security Academy)
- **Difficulty:** Easy
- **Date completed:** 2026-10-13
- **Category:** Forensics

## Summary
A challenge that hands you a SHA-256 checksum and warns that fake flags are floating around — you have to verify which file in a folder of decoys is the real one before decrypting it.

## Steps
1. SSH'd into the challenge server using the password provided by CyLab.
2. Enumerated the environment with `ls` — found `checksum.txt`, `decrypt.sh`, and a `files/` directory full of randomly-named files.
3. Followed the hint and ran a checksum over the whole directory:
   ```
   sha256sum files/* >> hash.txt
   ```
4. Tried decrypting with `hash.txt` directly — failed ("bad magic number", not a valid target file).
5. Tried hashing `hash.txt` itself and decrypting with that — also not the right approach.
6. Realized `hash.txt` already contained every file's individual hash, so the checksum given in the challenge had to be one of those lines. Searched for it:
   ```
   grep deb0a6ad170cd3033d30cae15146719dd4abf37d39136bcb10d88d9c33413374 hash.txt
   ```
   This returned `files/a906870d`.
7. Ran the decrypt script on that specific file:
   ```
   ./decrypt.sh files/a906870d
   ```
8. Got the flag.

## Tools used
- `ssh` — connected to the challenge instance
- `sha256sum` — generated checksums for every file in `files/`
- `grep` — matched the given checksum against the generated hash list to find the real file
- `decrypt.sh` — decrypted the verified file to reveal the flag

## Lesson learned
Trust but verify, literally. A checksum is only identifying one file — if a decrypt or hash-of-a-hash attempt fails, step back and check whether the "file" you're acting on is actually the thing the hash is meant to describe.

## Flag
```
academy{trust_but_verify_a906870d}
```