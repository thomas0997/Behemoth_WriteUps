# Session Management

- **Platform:** TryHackMe
- **Difficulty:** Not stated (lesson/premium room)
- **Date completed:** 2026-10-23
- **Category:** Web Exploitation — Session Management

## Summary
A lesson room covering the session management lifecycle (creation, tracking, expiry, termination) and the IAAA model, followed by a practical exploitation lab against a student/lecturer learning platform. The app exposes a raw `/users` API endpoint with every account's data (including credentials), letting a low-privileged student account escalate straight to superadmin without touching any of the session-hijacking tricks the theory section covered.

## Steps
1. Worked through the theory sections first: session lifecycle phases (creation/tracking/expiry/termination), the IAAA model (identification/authentication/authorisation/accountability), and cookie-based vs token-based session management — answered the inline knowledge-check questions along the way.
2. Started the lab machine and began mapping the app's actual session lifecycle by hand: signed up a student account, logged in, and watched network traffic — confirmed the app uses a combination of an `HTTPOnly` cookie plus token data in LocalStorage.
3. Poked at the LocalStorage token values (room-provided exploration, not something I personally pursued further) — tried bumping `userRole` from student to lecturer and tweaking the numeric role value, but that alone didn't unlock lecturer data.
4. Found the real way in: hit the `/users` endpoint directly and got back a JSON dump of every registered user, lecturers included — with login credentials right there in the response.
5. Used those leaked credentials to log in as other accounts, including a superadmin account found in that same `/users` dump.
6. Logged in as superadmin and used that account's elevated access to answer most of the lab's questions directly (student username, lecturer count, role count, total test attempts, student1's highest score) — all confirmed correct on first try.
7. For the final question (sequence of correct answers on the Database Types test), used the superadmin session to view that specific test's attempt data and read off the answer sequence directly: `y,n,n`.

## Tools used
- Browser dev tools — initial network traffic inspection and LocalStorage review
- Direct API endpoint access (`/users`) — the actual exploitation path, no manual session-hijacking needed
- Superadmin account access (obtained via the leaked credentials) — used to pull test attempt data directly

## Lesson learned
The lesson's theory built up a whole catalog of session-hijacking techniques (predictable session values, fixation, insecure transmission, authorisation bypass), but the real vulnerability in the lab was much blunter: an unauthenticated/under-protected API endpoint (`/users`) just handing out every account's credentials in plaintext JSON. No session manipulation was even necessary once that was found — straight credential reuse got superadmin access. Good reminder that fancy session-layer attacks are often unnecessary when a simpler, completely unrelated endpoint leaks everything needed to skip past them entirely. Also: always check for exposed "list all records" style API endpoints (`/users`, `/admin`, etc.) early in enumeration — they're a much faster win than auth/session logic bugs when they exist.

## Answers
- Student username with name X: confirmed correct
- Lecturer count: confirmed correct
- Role count (excluding unauthenticated): confirmed correct
- Total test attempts: confirmed correct
- student1's highest test score: confirmed correct
- Database Types test correct-answer sequence: `y,n,n`