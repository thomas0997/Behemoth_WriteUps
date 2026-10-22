# Includes

- **Platform:** picoCTF 2022
- **Difficulty:** Easy
- **Date completed:** 2026-10-22
- **Category:** Web Exploitation

## Summary
A page about "include directives" (CSS and JS files linked into the main HTML) hides the flag split across the two included files themselves — half in a CSS comment, half in a JS comment — rather than anywhere in the main page.

## Steps
1. Fetched the main page with `curl` and read through the HTML — noted it links two external files, `style.css` and `script.js`, via standard `<link>`/`<script>` include tags. The page's own content (a Wikipedia blurb about include directives) was a thematic hint to actually go check those included files rather than just the page body.
2. Fetched `style.css` directly:
   ```
   curl http://xebec.cylabacademy.net:36346/style.css
   ```
   Found a CSS comment at the bottom holding the first half of the flag:
   ```
   /*  academy{1nclu51v17y_1of2_  */
   ```
3. Fetched `script.js` directly:
   ```
   curl http://xebec.cylabacademy.net:36346/script.js
   ```
   Found a JS comment at the bottom holding the second half:
   ```
   //  f7w_2of2_64d6df37}
   ```
4. Concatenated both halves in order (CSS half first, JS half second) to get the complete flag.

## Tools used
- `curl` — fetched the main HTML page and both of its linked static assets directly

## Lesson learned
Linked/included files (CSS, JS, images, etc.) are part of a page's full attack surface, not just styling or behavior — comments inside them are an easy, often-overlooked place to stash data, since most people only read the rendered page or the main HTML source and skip the linked assets entirely. The challenge's own description (literally about "include" directives) was a direct pointer toward checking those files specifically.

## Flag
```
academy{1nclu51v17y_1of2_f7w_2of2_64d6df37}
```