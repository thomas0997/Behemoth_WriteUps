# SSTI1

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-21
- **Category:** Web Exploitation

## Summary
A "cool announcement website" built with Flask/Jinja2 templating accepts user input and renders it server-side without sanitization — a classic Server-Side Template Injection (SSTI). Confirming code execution with a simple arithmetic payload, then escalating through Jinja2's object internals to reach `os`/`open()`, gives direct filesystem access to the flag file.

## Steps
1. Loaded the site and found a single form that takes free-text "announcement" input, rendered back to the user — immediately suspicious for SSTI given the hint and the challenge name.
2. Referenced PortSwigger's SSTI writeup for background on the attack class before testing.
3. Tried a direct `curl` to `/announce` out of curiosity about routing — got a `405 Method Not Allowed` (wrong endpoint/method; the actual form posts to `/`, not `/announce`).
4. While poking around the dev tools during that detour, found a base64-encoded cookie value in storage and decoded it:
   ```
   echo 'YWNhZGVteXtjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzM0RUYyQ0U4fQ==' | base64 -d
   ```
   → `academy{c00k1e_m0nster_l0ves_c00kies_34EF2CE8}`. This turned out to be an unrelated flag from a different challenge instance, not this one — a red herring caused by mixing up tabs/instances rather than anything this challenge intentionally hid.
5. Refocused on the actual form and submitted the classic SSTI canary payload:
   ```
   {{7*7}}
   ```
   The rendered output came back as `49` — confirming the input is being evaluated as a real Jinja2 expression, not just echoed as text.
6. Needed to go from "arbitrary expression evaluation" to actual filesystem access. Searched for common Jinja2 SSTI escalation payloads and used the `get_flashed_messages` global (a Flask built-in always present in template context) to reach Python's builtins through its `__globals__`:
   ```
   {{ get_flashed_messages.__globals__.__builtins__.__import__('os').listdir('.') }}
   ```
   This listed the current directory's contents server-side and revealed a file literally named `flag`.
7. Read it directly through the same object chain, swapping `os.listdir` for `open().read()`:
   ```
   {{ get_flashed_messages.__globals__.__builtins__.open("flag").read() }}
   ```
   This returned the flag in the rendered page output.

## Tools used
- `curl` — initial recon and probing routes/methods
- Browser dev tools — inspected storage/cookies during the 405 detour
- `base64 -d` — decoded a cookie value (turned out to be an unrelated flag)
- Jinja2 SSTI payloads (`{{7*7}}`, `get_flashed_messages.__globals__...`) — confirmed template injection and escalated to arbitrary Python execution

## Lesson learned
A successful arithmetic SSTI payload (`{{7*7}}`) only confirms the vulnerability exists — getting from there to actual impact requires knowing (or finding) a path through the template engine's exposed globals back to Python's builtins. `get_flashed_messages` is a reliable pivot in Flask apps specifically because it's a function object always present in Jinja2's context, and function objects carry a `__globals__` dict with access to `__builtins__`. Also: don't assume a weird-looking artifact (like a stray base64 cookie) belongs to the current challenge — cross-check which instance/tab it actually came from before chasing it as a lead.

## Flag
```
academy{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_882dc002}
```