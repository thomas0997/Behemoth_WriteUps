# Cookie Monster

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Category:** Web Exploitation

## Summary
A challenge about HTTP cookies rather than code — the "secret recipe" Cookie Monster hid was sitting in a Base64-encoded cookie value on the site.

## Steps
1. Opened the Cookie Monster site and read through the page — nothing visible on the surface.
2. Took the hint seriously and checked the browser's cookies for the site via dev tools.
3. Found a cookie named `secret recipe` with the value:
   ```
   YWNhZGVteXtjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzM0RUYyQ0U4fQ%3D%3D%
   ```
4. Recognized the URL-encoded trailing `%3D%3D` as `==` — a double equals-sign padding is a strong Base64 tell.
5. Decoded it:
   ```
   echo "YWNhZGVteXtjMDBrMWVfbTBuc3Rlcl9sMHZlc19jMDBraWVzXzM0RUYyQ0U4fQ==" | base64 -d
   ```
6. Got the flag straight out.

## Tools used
- Browser dev tools — inspected the page's cookies
- `base64 -d` — decoded the cookie value

## Lesson learned
URL-encoded `%3D%3D` at the end of a string is a quick giveaway for Base64 padding — recognizing encoding artifacts like that saves time over throwing every string at a hash identifier first. Also, cookies are a legitimate place to hide (or leak) data, not just session tokens.

## Flag
```
academy{c00k1e_m0nster_l0ves_c00kies_34EF2CE8}
```