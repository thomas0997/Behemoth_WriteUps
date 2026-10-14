# logon

- **Platform:** picoCTF 2019
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Web Exploitation

## Summary
A login challenge that asks you to specifically authenticate as the user "Joe." The twist: the app has a hardcoded app-level filter that blocks the literal string "Joe" at the login form, but the actual session state is trackable (and editable) through a client-side cookie.

## Steps
1. Instinctively tried a classic SQL injection auth bypass (`' OR 1=1 --`) on the login form — it succeeded in logging in, but the site responded with "Success: You logged in! Not sure you'll be able to see the flag though," and the `/flag` page said "No flag for you." This confirmed the app has no real password check for any username — anything logs in — but `/flag` specifically requires being authenticated as Joe.
2. Tried submitting the literal username `Joe` directly — blocked by a hardcoded message: "I'm sorry Joe's password is super secure. You're not getting in that way." This confirmed there's an app-level string filter specifically blocking the exact string "Joe" at the login form, regardless of password.
3. Tried several SQL-injection-flavored bypasses targeting that filter (case variants like `JOE'--`/`jOe'--`, string concatenation tricks like `Jo'||'e`) — none worked; the app kept logging in as the literal string submitted, not as Joe.
4. Switched approach: opened browser dev tools and inspected the cookies set after a normal (non-"Joe") login. Found a cookie holding the session's username value in a readable/editable form, along with what looked like a boolean flag.
5. Manually edited the cookie: changed the username value from `joe` (lowercase, from a successful login attempt) to `Joe` (correct case), and flipped an associated false boolean value to true.
6. Reloaded `/flag` with the edited cookie active — got the flag.

## Tools used
- Browser dev tools (Application/Storage → Cookies) — inspected and directly edited the session cookie's values after logging in normally

## Lesson learned
Server-side input filters only protect the endpoint they run on — if the resulting session state is stored client-side in a plain, editable cookie, the filter can be bypassed entirely by editing the cookie directly instead of trying to sneak a different login payload past the check. Trust boundaries matter: never assume a cookie's value is safe just because the login form validated something before setting it.

## Flag
```
academy{th3_c0nsp1r4cy_l1v3s_30d47a57}
```