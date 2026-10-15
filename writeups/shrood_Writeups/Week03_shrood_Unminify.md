# Unminify

- **Platform:** picoCTF 2024
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Web Exploitation

## Summary
A "flag distribution" site claims the flag is only visible once minified code is "unminified" / formatted. In reality the flag is sitting in plain text in the raw page source the whole time, hidden among a sea of decoy elements that all share the same empty placeholder class.

## Steps
1. Ran `curl` directly against the challenge instance to grab the raw (minified) HTML — no need for dev tools or a formatter.
2. Noticed nearly every element on the page has the exact same class: `class="academy{}"` — an empty, templated placeholder repeated dozens of times as noise/decoy.
3. Scanning through, one element breaks the pattern:
   ```html
   <p class="academy{pr3tty_c0d3_f2dc9e9e}"></p>
   ```
   Every other tag has an empty `academy{}` class; this one has the flag filled directly into the class attribute.
4. Extracted the flag straight from that class value — no minification/formatting tool was actually needed, since `curl`'s raw output already contained the plaintext flag.

## Tools used
- `curl` — fetched the raw page source directly

## Lesson learned
The hint path (view-source, formatters, pretty-printers) suggests minification is obscuring the flag, but minifying code doesn't encode or hide any of its literal string content — it only strips whitespace and shortens formatting. Scanning raw source for a pattern-breaking value among repeated noise (here, one non-empty class among many identical empty ones) is often faster than reaching for a prettifier.

## Flag
```
academy{pr3tty_c0d3_f2dc9e9e}
```