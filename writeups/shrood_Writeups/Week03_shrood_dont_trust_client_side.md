# dont-use-client-side

- **Platform:** picoCTF 2019
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Web Exploitation

## Summary
A "secure login portal" validates the password entirely in client-side JavaScript, checking 8 separate 4-character substrings against hardcoded values. Since the real flag is split across those hardcoded comparisons, reading the page source hands you the flag directly — no actual login or bypass needed.

## Steps
1. Ran `curl` against the challenge instance and got the full page source back directly (no auth, no JS execution needed to see it).
2. Found the `verify()` function performs a chain of nested `if` statements, each comparing a 4-character substring of the user's input (`checkpass`) against a hardcoded string:
   ```js
   split = 4;
   checkpass.substring(0, split) == 'acad'
   checkpass.substring(split, split*2) == 'emy{'
   checkpass.substring(split*2, split*3) == 'no_c'
   checkpass.substring(split*3, split*4) == 'lien'
   checkpass.substring(split*4, split*5) == 'ts_p'
   checkpass.substring(split*5, split*6) == 'lz_7'
   checkpass.substring(split*6, split*7) == '43d4'
   checkpass.substring(split*7, split*8) == '3c8}'
   ```
3. Recognized that these 8 hardcoded 4-character chunks, concatenated in order, are the actual password/flag — the "validation" logic never needed to be passed at all, since the answer is sitting in plaintext in the comparison values.
4. Concatenated the chunks in sequence:
   ```
   acad + emy{ + no_c + lien + ts_p + lz_7 + 43d4 + 3c8}
   ```
   to get the full flag.

## Tools used
- `curl` — fetched the raw page source directly, bypassing the need to run any JavaScript at all

## Lesson learned
Any validation logic that runs entirely in the browser is visible to the user — the hint ("Never trust the client") points directly at this. Here it went a step further: the developer hardcoded the actual secret values as literal comparison targets in client-side JS, which is the worst case of this mistake — the secret itself, not just the validation logic, is exposed in page source.

## Flag
```
academy{no_clients_plz_743d43c8}
```