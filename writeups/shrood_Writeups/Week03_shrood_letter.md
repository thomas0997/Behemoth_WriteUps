# Letter

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Date completed:** 2026-10-13
- **Category:** Forensics

## Summary
A challenge involving a `digits.bin` file that looked like raw binary data at first glance, but turned out to be an ASCII-encoded binary string hiding an image with a hidden flag.

## Steps
1. Downloaded the task files from the room and extracted the zip.
2. Ran `binwalk digits.bin` — came back empty, no embedded files detected.
3. Checked the file size: `cat digits.bin | wc -c` → 71312 bytes.
4. Inspected the content directly with `head -c 200 digits.bin` and noticed it wasn't raw binary — it was plain text made up of literal `0` and `1` characters.
5. Did the math: 71312 ÷ 8 = 8914, which divides evenly — confirmed this was 8-bit-per-byte binary text, not corrupted data.
6. Converted the text to real bytes:
   ```
   tr -d '\n\r ' < digits.bin | fold -w8 | while read b; do printf "\\x$(printf '%02x' $((2#$b)))"; done > out.bin
   ```
7. Ran `file out.bin` → identified as a valid **JPEG image, 800×500**.
8. Renamed it to `out.jpg` and opened it with `xdg-open out.jpg` to view the image.
9. The flag was visible directly in the image itself.

## Tools used
- `binwalk` — initial file carving attempt (came back empty)
- `file` — identified the decoded output as a JPEG
- `tr`, `fold`, `printf` — converted the ASCII binary text to real bytes
- `xdg-open` — viewed the resulting image

## Lesson learned
Don't assume a `.bin` file is actually binary data — check with `head`/`xxd` first. If it's literal `0`/`1` text characters instead of real bytes, `binwalk` won't find anything because there's nothing embedded yet; you have to decode the ASCII binary into real bytes first before any carving tool can do its job.

## Flag
```
Academy{h1dd3n_1n_th3_b1n4ry_75fdbfb71}
```