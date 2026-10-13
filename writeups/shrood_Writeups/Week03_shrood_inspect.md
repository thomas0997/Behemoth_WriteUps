# Inspect HTML

- **Platform:** picoCTF 2022
- **Difficulty:** Easy
- **Date completed:** 2026-10-14
- **Category:** Web Exploitation

## Summary
A quick challenge about checking a page's raw source rather than trusting the rendered page — the flag was sitting in an HTML comment.

## Steps
1. Pulled the page's raw HTML straight from the command line instead of opening it in a browser:
   ```
   curl <challenge-url>
   ```
2. Read through the returned source — a short page of text "On Histiaeus," quoting Herodotus.
3. Spotted an HTML comment at the bottom of the `<body>`, right after the source citation, containing the flag.

## Tools used
- `curl` — fetched the page's raw HTML directly instead of using the browser's Inspect Element panel

## Lesson learned
Anything left in an HTML comment ships to every visitor, whether or not it ever renders on screen — `curl` (or View Source) finds it just as fast as opening dev tools, no browser needed.

## Flag
```
academy{1n5p3t0r_0f_h7ml_cdc35adf}
```