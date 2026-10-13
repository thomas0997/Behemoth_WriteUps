# head-dump

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-14
- **Category:** Web Exploitation

## Summary
A simple blog site links out to its own Swagger/OpenAPI docs, which in turn exposes a debug `/heapdump` endpoint — a memory dump of the live server process that leaks the flag in plaintext.

## Steps
1. Opened the blog website and browsed its articles.
2. Found a post about "API Documentation" with a link that led to the Swagger UI at:
   ```
   http://chatelaine.cylabacademy.net:12820/api-docs/#/
   ```
3. Scrolled through the documented endpoints (standard CRUD for `/api/posts`) down to a "Diagnosing" section listing `GET /heapdump`.
4. Opened the URL directly:
   ```
   http://chatelaine.cylabacademy.net:12820/heapdump
   ```
   which downloaded a `.heapsnapshot` file — a full V8/Node.js memory dump.
5. Searched the dump for the flag pattern:
   ```
   cat heapdump-1791897917527.heapsnapshot | grep -i "academy{"
   ```
6. Got the flag straight out of the memory dump.

## Tools used
- Browser — navigated the blog and its linked Swagger/API docs
- `curl`/browser download — pulled the `.heapsnapshot` file from the exposed `/heapdump` endpoint
- `grep` — searched the raw memory dump for the flag format

## Lesson learned
Debug and diagnostic endpoints (heap dumps, memory profilers, etc.) should never ship to production — a full memory snapshot can leak secrets, tokens, and other sensitive data that only ever existed in RAM, with zero exploitation needed beyond just downloading the file.

## Flag
```
academy{Pat!3nt_15_Th3_K3y_1dc68c38}
```