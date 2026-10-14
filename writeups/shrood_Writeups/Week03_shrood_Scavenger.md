# Scavenger Hunt

- **Platform:** picoCTF 2021
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Web Exploitation

## Summary
A five-part flag scattered across different parts of a website — HTML comments, a CSS file, a JS file, `robots.txt`, `.htaccess`, and finally a leaked macOS `.DS_Store` file. Each location gave a clue pointing to the next.

## Steps
1. Opened the site and ran `curl` on the root page — found an HTML comment with the first part of the flag:
   ```
   <!-- Here's the first part of the flag: academy{t -->
   ```
2. Checked the linked CSS file (`/mycss.css`) and found part 2 in a comment there:
   ```
   /* ...Here's part 2: h4ts_4_l0 */
   ```
3. Checked the linked JS file (`/myjs.js`) — no flag part, but a hint instead:
   ```
   /* How can I keep Google from indexing my website? */
   ```
4. Recognized that as a reference to `robots.txt` and fetched it:
   ```
   curl http://chatelaine.cylabacademy.net:38614/robots.txt
   ```
   Found part 3 plus another hint about the server being Apache:
   ```
   # Part 3: t_0f_pl4c
   # I think this is an apache server... can you Access the next flag?
   ```
5. Probed common Apache-exposed paths (`/server-status` → 403, `/server-info` → 404) before trying `.htaccess` directly:
   ```
   curl http://chatelaine.cylabacademy.net:38614/.htaccess
   ```
   Found part 4 plus a pointed hint about Mac and "Store":
   ```
   # Part 4: 3s_2_lO0k
   # I love making websites on my Mac, I can Store a lot of information there.
   ```
6. Recognized the capitalized "Store" as a nod to `.DS_Store`, the metadata file macOS silently creates in every folder — and a common one developers forget to exclude from their deployed site:
   ```
   curl http://chatelaine.cylabacademy.net:38614/.DS_Store
   ```
7. Got the final part and a completion message.
8. Assembled all five parts into the full flag.

## Tools used
- `curl` — fetched every page, stylesheet, script, and hidden file directly from the command line

## Lesson learned
Sensitive data doesn't only hide in obvious places — HTML/CSS/JS comments, `robots.txt`, `.htaccess`, and OS-generated metadata files like `.DS_Store` are all commonly overlooked and accidentally deployed to production web servers. Each file format has its own comment syntax, so checking all linked assets (not just the main page) is worth the extra few requests.

## Flag
```
academy{th4ts_4_l0t_0f_pl4c3s_2_lO0k_2b9c61f}
```