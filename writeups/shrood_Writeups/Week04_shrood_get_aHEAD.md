# GET aHEAD

- **Platform:** picoCTF 2021
- **Difficulty:** Easy
- **Date completed:** 2026-10-21
- **Category:** Web Exploitation

## Summary
The challenge title is a direct pun on HTTP methods — "GET aHEAD" points at the `HEAD` request method rather than the default `GET`. The flag is stuffed into a response header that only shows up when inspecting the full header set of the server's response.

## Steps
1. Read the challenge title literally as a hint: "GET aHEAD" → `HEAD` is a real HTTP method, distinct from `GET`, that requests only the headers of a response (no body).
2. Sent a `HEAD` request directly with `curl`:
   ```
   curl -I http://xebec.cylabacademy.net:41219/
   ```
3. The flag was sitting in the response headers — no need to inspect the page body or do anything further.

## Tools used
- `curl -I` — sent a `HEAD` request and inspected the returned headers directly

## Lesson learned
Challenge titles are often literal hints, not just flavor text — "GET aHEAD" was pointing straight at the `HEAD` HTTP method. Response headers are worth checking as a first step on any web challenge, since they're an easy place to stash data that won't show up in a plain page render or even a default `curl` GET without `-v`/`-I`.

## Flag
```
academy{r3j3ct_th3_du4l1ty_57f62d9}
```