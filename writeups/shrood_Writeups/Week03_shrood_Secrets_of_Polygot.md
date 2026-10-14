# Secret of the Polyglot

- **Platform:** picoCTF 2024
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Forensics

## Summary
A single file that reports as two different file types — a PNG and a PDF stitched together into one "polyglot" file. The flag was split across both halves, hidden in plain sight: one half as literal tiny-scaled text in the PDF, the other rendered as text inside the PNG image.

## Steps
1. Downloaded the suspicious file with `wget` since the challenge page had no direct download button:
   ```
   wget https://challenge-files.cylabacademy.net/library/.../flag2of2-final.pdf
   ```
2. Ran `exiftool` on it — nothing useful returned.
3. Ran `cat` on the raw file and spotted `IHDR`, `PLTE`, `IDAT`, `IEND` — standard PNG chunk markers — appearing *before* the file's own `%PDF-1.4` header. That meant the file was actually two formats concatenated together.
4. Confirmed with `binwalk`:
   ```
   binwalk flag2of2-final.pdf
   ```
   Output showed a PNG image starting at offset 0 and a PDF document starting at offset 453 (`0x1C5`).
5. Tried `binwalk --extract` and `binwalk --dd='png:png'` to auto-carve the PNG — both failed with "no utility found" (extraction dependency missing).
6. Manually carved the PNG out with `dd`, using the exact offset binwalk reported as the boundary:
   ```
   dd if=flag2of2-final.pdf of=flag1.png bs=1 count=453
   ```
   Opened `flag1.png` directly and read the first half of the flag off the image: `academy{f1u3n7`
7. For the PDF half, ran `binwalk --extract` to pull out the compressed content stream, then manually inflated/read the object containing the page's text-drawing instructions. Found this inside the PDF's content stream:
   ```
   q 0.1 0 0 0.1 0 0 cm
   ...
   (1n_pn9_&_pdf_96385309})Tj
   ```
   The `0.1 0 0 0.1 0 0 cm` matrix scales everything drawn afterward down to 10% size — so the text was technically on the page the whole time, just rendered too small to see without extracting the raw instructions.
8. Combined both halves into the full flag.

## Tools used
- `wget` — downloaded the challenge file
- `exiftool` — checked metadata (came back empty)
- `cat` / `strings` — spotted PNG chunk signatures inside what was labeled a PDF
- `binwalk` — identified both embedded file types and their exact byte offsets
- `dd` — manually carved the PNG out of the combined file using the offset binwalk provided

## Lesson learned
A file extension or a single signature check doesn't guarantee what a file actually is — tools that disagree about a file's type is a strong signal it's a polyglot. `binwalk` is the fast way to find where one format ends and another begins, and `dd` with a precise byte offset is a reliable fallback when auto-extraction tools are missing dependencies.

## Flag
```
academy{f1u3n7_1n_pn9_&_pdf_96385309}
```