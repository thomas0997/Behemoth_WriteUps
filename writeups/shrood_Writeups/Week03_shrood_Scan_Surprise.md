# Scan Surprise

- **Platform:** picoCTF 2024
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Forensics

## Summary
A challenge that delivers the flag as a QR code image instead of plain text — the extracted file tree led to a `flag.png` that needed scanning rather than reading.

## Steps
1. Downloaded `challenge.zip` and extracted it, revealing a nested directory structure mimicking a real filesystem: `home/ctf-player/drop-in/`.
2. Navigated into that path and found a single file: `flag.png`.
3. With no phone on hand to scan it with a camera, followed the hint and installed `zbar-tools`:
   ```
   sudo apt install zbar-tools
   ```
4. Ran `zbarimg` directly on the image:
   ```
   zbarimg flag.png
   ```
5. It decoded the QR code and printed the flag straight to the terminal.

## Tools used
- `zbarimg` (from `zbar-tools`) — decoded the QR code image into text without needing a phone camera

## Lesson learned
A flag doesn't have to be stored as plain text — command-line QR decoders like `zbarimg` handle what a phone camera normally would, so there's no need for a physical device to read an encoded image in a terminal-only environment.

## Flag
```
academy{p33k_@_b00_36769c4b}
```