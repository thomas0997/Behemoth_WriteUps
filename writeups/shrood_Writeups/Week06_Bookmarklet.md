# Bookmarklet

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Date completed:** 2026-11-08
- **Category:** Web Exploitation

## Summary
A page provides a "bookmarklet" (a `javascript:` URI meant to be saved as a browser bookmark) that decrypts and prints the flag — but the input field it's meant to be pasted into has paste disabled via a JS handler. Bypassing that restriction and running the decryption code directly gets the flag without ever needing a real bookmark.

## Steps
1. Loaded the challenge page and found an input form with a JavaScript function attached — immediately recognized this as a paste-blocking handler (a common "allowpaste" pattern used to annoy/slow down manual copy-paste).
2. Tried pasting into the field first anyway, since paste restrictions are usually only enforced client-side and might not actually be solid — confirmed it was blocked (`allowpaste` not defined error visible in the console once probed).
3. Rather than fighting the paste restriction in the input field, went straight to the browser DevTools console and pasted/ran the bookmarklet's JavaScript code there directly instead — bypassing the input field (and its paste block) entirely, since the console has no such restriction.
4. The bookmarklet code XOR-decrypted an embedded `encryptedFlag` string against a hardcoded key (`piccctf`), character by character, and printed the result via `alert()`.
5. Running it in the console triggered the alert with the decrypted flag.

## Tools used
- Browser DevTools (Console) — ran the bookmarklet's JS payload directly instead of using it as an actual bookmarklet

## Lesson learned
A client-side paste restriction on an input field is a UI-layer inconvenience, not a security boundary — the same JavaScript that's blocked from running "through" a restricted input can always be run directly in the console, since the console isn't subject to the page's input-level event handlers. When a challenge hands you ready-made JS code (like a bookmarklet payload) and the delivery mechanism is being obstructed, the fix is usually to skip the intended delivery path and execute the code yourself.

## Flag
```
academy{pdjg3_lum3r_1b8cd5e0}
```