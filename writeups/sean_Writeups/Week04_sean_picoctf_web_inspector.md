# Write-up: Web Exploitation - Inspect Element

**Platform:** picoCTF 2022 (by LT 'syreal' Jones)
**Category:** Web Exploitation (Easy)
**Date:** October 25, 2026
**Author:** Sean

---

## Steps Taken

1. Opened the challenge's linked website in the browser.
2. Visually scanned the page — nothing obvious was shown on the rendered page itself.
3. Used the hint ("What is the web inspector in web browsers?") to open **Developer Tools** (right-click → **Inspect**, or `F12` / `Ctrl+Shift+I`).
4. Went to the **Elements** tab to view the page's raw HTML source, rather than just what's visually rendered.
5. Searched through the HTML for anything hidden — flags are often placed in HTML comments (`<!-- like this -->`), hidden `<div>`/`<span>` elements, or attributes not shown on the visible page.
6. Found the flag embedded in the HTML source.

## Tools Used

- Browser Developer Tools (Inspect Element / Elements tab) — view a page's raw HTML instead of just what's rendered visually

## Answer

- Flag: `academy{1n5p3t0r_0f_h7ml_cdc35adf}`

## Lesson Learned

_Write your own lesson here, e.g. why checking the page source is often the first move in web exploitation challenges, or where in the HTML you found the flag (a comment, hidden element, etc.)._
