# No FA

**Platform:** CyLab Security Academy (picoCTF 2026)
**Category:** Web Exploitation
**Difficulty:** Medium
**Author:** Darkraicg492
**Date completed:** 2026-11-27

## Challenge
"Seems like some data has been leaked! Can you get the flag?" — a login page protected by 2FA/OTP.

## Steps
1. Queried the provided database and found leaked credentials, including a hashed password for an admin account.
2. Used `hash-identifier` to confirm the hash type — SHA-256.
3. Cracked the hash with an online SHA-256 decrypter → recovered the plaintext password `apple@123`.
4. Logged in with `admin` / `apple@123`. The app accepted the password but then prompted for a 2FA OTP code.
5. Recalled that Flask session cookies are signed (not encrypted) — meaning their contents are readable by anyone without needing the app's secret key, just base64-decodable.
6. Grabbed the session cookie from the browser and ran:
   ```
   flask-unsign --decode --cookie "<session_cookie>"
   ```
   This decoded the cookie's payload directly, revealing:
   ```
   {'logged': 'false', 'otp_secret': '2942', 'otp_timestamp': ..., 'username': 'admin'}
   ```
7. The `otp_secret` field was sitting in plain sight inside the cookie the server itself had handed to the client — the app was storing the actual OTP value client-side instead of keeping it server-only.
8. Typed that `otp_secret` value into the OTP prompt on the login page → authenticated as admin → flag revealed.

## Mistakes along the way
- First command attempt used `flask-unsign –decrypt –cookies` (wrong flags and smart-quote dashes pasted from a note, not real `--` flags) → failed with "unrecognized arguments."
- Second attempt accidentally typed `flask -unsign` (space instead of hyphen) → shell interpreted it as running the actual `flask` CLI with an invalid `-u` option, not the `flask-unsign` tool at all.
- Also had leftover terminal escape-sequence garbage (`^[[200~` / `~`) pasted into one cookie string from a bracketed-paste-mode artifact, though the command still happened to succeed around it.
- Once the command was correctly `flask-unsign --decode --cookie "<cookie>"`, it worked immediately.

## Tools used
- `hash-identifier`, an online SHA-256 cracker, `flask-unsign`

## Lesson learned
- Flask's default session cookies are **signed, not encrypted** — anyone can decode and read the full payload without the server's secret key; you'd only need the key to *forge/modify* a valid cookie. "Decrypting" isn't the right mental model here — it's just decoding.
- 2FA is only as secure as where the secret lives: storing the OTP value in the client-side session (rather than purely server-side, checked against what the user submits) completely defeats the purpose of the second factor — the "proof" is sitting in the same place the attacker already controls.
- Watch out for literal `–` (en-dash/smart-dash from pasted text) vs real `--` (double hyphen) in copied commands — they look nearly identical but the shell treats them completely differently.

## Flag
`academy{n0_r4t3_n0_4uth_24652f75}`