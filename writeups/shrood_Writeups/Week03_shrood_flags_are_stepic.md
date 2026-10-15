# Flags Are Stepic

- **Platform:** picoCTF 2025
- **Difficulty:** Medium
- **Date completed:** 2026-10-15
- **Category:** Forensics

## Summary
A site displaying a grid of country flags turns out to have one fake "country" planted among the real ones — `Upanzi, Republic The` — with a custom CSS override on its flag image. The real flag image behind it is a massively oversized PNG compared to every other flag on the page, and hides a message encoded with `stepic` (LSB steganography).

## Steps
1. Pulled the page source with `curl` and noticed the flag list is a hardcoded JS array of `{name, img}` objects — one entry, `"Upanzi, Republic The"`, isn't a real country and has an inline `style` override (`width: 120px!important; height: 90px!important;`) that none of the other flags have.
2. Tried fetching `flags/upz.png` directly and initially got a 404 (not a real file at that moment/path) — this was resolved by re-fetching it as part of a bulk download rather than a one-off request.
3. Confirmed the base flags path otherwise works correctly by fetching a known-real flag (`us.png` — valid PNG, 2375 bytes).
4. Checked `styles.css` for any extra clues — found nothing unusual beyond standard grid/flex layout rules.
5. Wrote a Python script to scrape every `img: "flags/..."` path out of the page's JS array and bulk-download all of them, printing each file's size:
   ```python
   import re, requests

   html = requests.get('http://xebec.cylabacademy.net:46818/').text
   imgs = re.findall(r'img:\s*"(flags/[^"]+)"', html)

   for path in imgs:
       url = f'http://xebec.cylabacademy.net:46818/{path}'
       r = requests.get(url)
       fname = path.split('/')[-1]
       with open(fname, 'wb') as f:
           f.write(r.content)
       print(fname, len(r.content), r.status_code)
   ```
6. Sorted all downloaded flags by file size (`ls -la *.png | sort -k5 -n`). Every real flag was under ~20KB — except `upz.png`, which came back as **1,788,395 bytes**, dwarfing every other file by two orders of magnitude.
7. Installed `stepic` (`pip install stepic --break-system-packages`) and decoded the oversized image:
   ```python
   import stepic
   from PIL import Image
   img = Image.open('upz.png')
   data = stepic.decode(img)
   print(data)
   ```
8. Got a `DecompressionBombWarning` (expected, given the image's absurd pixel count) but it decoded cleanly to the flag.

## Tools used
- `curl` — fetched the page HTML and `styles.css`
- Python (`requests`) — bulk-scraped and downloaded every flag image referenced in the page's JS
- `file`, `ls -la` / `sort` — identified the file-size anomaly among the flag images
- `stepic` + `Pillow` — decoded the LSB-steganographed message from the oversized PNG

## Lesson learned
A fake/decoy entry (the "Upanzi" country with a suspicious CSS override) can be a red herring pointing at a broken link, while the actual hidden data sits in a legitimately-referenced file elsewhere on the page. When steganography tools are named directly in a challenge title ("stepic"), that's often a literal pointer to the exact library needed — but the harder part is finding *which* file was tampered with. Comparing file sizes across a large set of otherwise-similar assets is a fast way to spot one that's been bloated by embedded data, since LSB steganography alone doesn't bloat a file this much — this file was deliberately oversized/bomb-like to hide a payload.

## Flag
```
academy{fl4g_h45_fl4g4fd6a57e}
```