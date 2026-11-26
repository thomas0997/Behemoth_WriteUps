# Operation Orchid

**Platform:** CyLab Security Academy (picoCTF 2022)
**Category:** Forensics
**Difficulty:** Medium
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-26

## Challenge
Download a disk image and find the flag.

## Steps
1. `wget` + `gunzip` to download and decompress `disk.flag.img`.
2. `mmls disk.flag.img` → partition table showed three real regions: two Linux `(0x83)` partitions and one Linux Swap. Picked the second Linux partition, offset `411648`.
3. `fls -o 411648 disk.flag.img` → only showed a `home` directory at root, nothing else obvious — needed to dig deeper than a flat listing.
4. Found two file inodes of interest while exploring (from earlier recon in this same image): `1876` (deleted, `(realloc)`, named `flag.txt`) and `1782` (`flag.txt.enc`).
5. `icat -o 411648 disk.flag.img 1876` → came back as two unrelated floating-point numbers (GPS-looking coordinates), not the flag — confirmed this inode's blocks had been reallocated to a different file after the original was deleted. Dead end.
6. `icat -o 411648 disk.flag.img 1782` → came back as binary data starting with `Salted__`, the standard OpenSSL-encrypted-file header. Extracted it to `flag.txt.enc`.
7. Checked `.ash_history` (inode `1875`) — shell history left behind on the filesystem — and found the exact commands originally used:
   ```
   touch flag.txt
   nano flag.txt
   openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567
   shred -u flag.txt
   ```
   This handed over both the cipher (`aes256`) and the passphrase (`unbreakablepassword1234567`) directly.
8. Ran the same command in decrypt mode:
   ```
   openssl aes256 -d -salt -in flag.txt.enc -out flag.txt -k unbreakablepassword1234567
   ```
9. `cat flag.txt` → flag.

## Mistakes along the way
- First decrypt attempt accidentally re-ran the *encrypt* command verbatim from history (missing `-d`, with `-in`/`-out` backwards) — errored because `flag.txt` didn't exist yet (it had been `shred`-deleted by the original user).
- Second attempt added `-d` but got `bad decrypt` / "deprecated key derivation" warnings — this was from OpenSSL's newer default KDF behavior mismatching the original encryption; the command needed to be run exactly as the original cipher/flags specified (`aes256 -salt`, no extra options), which then succeeded.

## Tools used
- `wget`, `gunzip`, `mmls`, `fls`, `icat`, `openssl`

## Lesson learned
- A deleted file's inode being `(realloc)`'d is a real dead end — don't waste more time trying to recover it, its blocks now belong to something else.
- Shell history files (`.ash_history`, `.bash_history`) left on a disk image are a goldmine in forensics challenges — they can hand you the exact attacker/victim commands, including passwords typed in cleartext on the command line.
- When replaying a historical `openssl enc` command in reverse to decrypt, match its exact flags (cipher name, `-salt`) rather than just adding `-d` — mismatched KDF/cipher defaults between OpenSSL versions can cause `bad decrypt` even with the right password.

## Flag
`academy{h4un71ng_p457_718ebd29}`