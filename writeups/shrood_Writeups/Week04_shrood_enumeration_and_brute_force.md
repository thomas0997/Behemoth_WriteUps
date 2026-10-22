# TryHackMe — Enumeration & Brute Force (Easy)

Notes from the room PDF. Lab target: `enum.thm` (`10.48.129.129`).

## Username Enumeration
- Verbose error messages are the core leak: a login form that says "Email does not exist" vs "Invalid password" tells you which usernames are valid without ever needing the password.
- Ways to induce verbose errors: invalid login attempts, SQLi probes, path traversal, form field manipulation, Burp Intruder fuzzing.
- Automation pattern: script POSTs each candidate email with a throwaway password to the login/check endpoint, and classifies by whether the response contains the "Email does not exist" string.
  - `enum.thm/labs/verbose_login/functions.php`, POST fields: `username`, `password`, `function: login`.
  - Loop over a username wordlist (e.g. `nvagrnom/username-lists` on GitHub), print `[VALID]`/`[INVALID]` per entry.
  - Lab answer: valid email found = `canderson@gmail.com`.

## Password Reset Flow Vulnerabilities
Three common reset mechanisms, each with its own weak point:
- **Email-based** — relies on the secrecy of the link/token and the security of the user's inbox.
- **Security-question-based** — breaks if the answers are guessable/OSINT-able.
- **SMS-based** — breaks under SIM-swap/interception.

Shared failure modes across all of them:
- **Predictable tokens** — sequential or low-entropy tokens can be brute-forced.
- **Token expiration issues** — long-lived or non-expiring tokens widen the attack window.
- **Insufficient validation** — weak identity checks at the reset step.
- **Information disclosure** — error messages confirming whether an email/username exists.
- **Insecure transport** — tokens sent over plain HTTP are interceptable.

### Exploiting predictable tokens (lab walkthrough)
Target used: `$token = mt_rand(100, 200);` — a 3-digit numeric token, trivially bruteforceable.
1. Submit `admin@admin.com` on the "Forgot Password" form → app confirms "reset link sent" and (for demo purposes) exposes the reset URL pattern: `.../reset_password.php?token=123`.
2. Capture that request in Burp, send to Intruder, mark `token` as the payload position.
3. Generate a numeric wordlist for the known range: `crunch 3 3 -o otp.txt -t %%% -s 100 -e 200`.
4. Load `otp.txt` as the Intruder payload, run a Sniper attack.
5. A valid token returns a much longer response body (the actual "reset password" success page) vs. the "Invalid token" responses — sort by response length to spot it.
6. Log in with the new password shown on success.
- Flag: `THM{50_pr3d1ct4BL333!!}`
- Real-world apps usually use 6-digit codes; the room uses 3 digits only because Burp Community's Intruder is rate-limited.

## HTTP Basic Authentication
- Simple scheme: client sends `Authorization: Basic base64(username:password)` after an initial `401 Unauthorized` + `WWW-Authenticate: Basic realm="..."` challenge from the server.
- Defined in RFC 7617. **Not encryption** — base64 is trivially reversible, so Basic Auth over plain HTTP exposes credentials to anyone sniffing traffic. The main real-world risk is weak credentials, which are brute-forceable.

### Brute-forcing Basic Auth (lab walkthrough)
1. Hit the Basic Auth prompt, submit any dummy creds, capture the request in Burp (holds `Authorization: Basic <base64>`).
2. Send to Intruder, mark the base64 string as the payload position.
3. Payloads tab → Simple list → load a password wordlist (e.g. SecLists `500-worst-passwords.txt`).
4. Because the whole `username:password` string must be base64'd together (not just the password), add two **payload processing** rules, applied in order:
   - "Add Prefix" → `admin:` (prepends the known username before encoding)
   - "Encode" → Base64-encode
5. In Payload Encoding, remove `=` from the auto-URL-encoded character set (it's base64 padding, not something that should get percent-encoded) — otherwise the request's base64 gets mangled.
6. Run the attack. A **200 OK** response (vs. 401 for everything else) marks the correct password.
7. Base64-decode the winning `Authorization` header value to read back `username:password` in the clear, then log in.
- Flag: `THM{b4$$1C_AuTTHHH}`
- Room suggests trying Hydra as an alternative to Burp for this same attack (not walked through in the PDF).

## Wayback URLs & Google Dorks (recon, not exploitation)
- **Wayback Machine** (`archive.org/web/`) — old/removed pages, files and directories can still be indexed from a site's history, sometimes exposing stuff no longer linked from the live site.
- **waybackurls** (`github.com/tomnomnom/waybackurls`) — dumps every URL the Wayback Machine has archived for a domain in bulk:
  ```
  git clone https://github.com/tomnomnom/waybackurls
  cd waybackurls && go build
  ./waybackurls tryhackme.com
  ```
  Useful for surfacing old `.well-known/` endpoints, forgotten config files, backup paths, etc.
- **Google Dorks** — targeted search operators to find things that shouldn't be public:
  - `site:example.com inurl:admin` — admin panels
  - `filetype:log "password" site:example.com` — log files leaking passwords
  - `intitle:"index of" "backup" site:example.com` — exposed backup directories

## Key Takeaways (room's own conclusion)
- Proper enumeration (verbose errors, wayback history, dorks) is often what actually finds the weak spot before you ever touch brute force.
- Efficient brute force = good wordlists + managing attack parameters + watching for rate limiting/account lockout defenses.
- Always get explicit permission before running enumeration/brute-force against anything — unauthorized attacks are illegal.