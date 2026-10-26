# c4ptur3th3fl4g

- **FILE NAME FORMAT:** Week05_dein_c4ptur3th3fl4g
- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Date completed:** 2026-10-26
- **Category:** Cryptography / Steganography / Forensics

## Summary

Completed the *c4ptur3th3fl4g* room on TryHackMe, which introduced me to different encoding methods, character shifting, audio spectrograms, and steganography. I learned how to identify and decode hidden messages, inspect files for concealed data, and use different tools to investigate suspicious files.

## Steps

### 1. Task 1: Translation and Shifting

- Practiced identifying and decoding messages using different encoding methods and character substitutions.
- Learned to recognize and decode:
  - **Leetspeak:** Replaced numbers with similar-looking letters, such as `4` for `a` and `3` for `e`.
  - **Binary:** Converted groups of binary digits into ASCII text.
  - **Decimal ASCII:** Converted decimal character codes into readable text.
  - **Base32:** Decoded text using the Base32 character set and padding.
  - **Hexadecimal:** Converted hexadecimal values into readable text.
  - **ROT13:** Shifted letters by 13 positions in the alphabet.
  - **ROT47:** Shifted printable ASCII characters, including numbers and symbols.
- Worked through a layered message that required decoding Base64, Morse code, binary, decimal values, and a JSON-style report before applying ROT47 and another decimal conversion.
- Learned that one message can contain multiple encoding layers, so each layer must be identified and decoded in the correct order.
- Also learned to clear old CyberChef operations between challenges because leftover steps can cause errors, and to inspect the end of the output when a decoded message appears incomplete.

### 2. Task 2: Spectrograms

- Learned that audio files can contain hidden messages represented visually in their frequencies.
- Explored how spectrograms can reveal text or patterns that may not be noticeable when listening to the audio normally.
- Learned that adjusting the frequency range, window size, gain, and zoom can make hidden information easier to read.
- Understood that analyzing audio visually can be just as important as listening to it during a CTF challenge.

### 3. Task 3: Steganography

- Learned how information can be hidden inside files through metadata, appended data, embedded files, or password-protected payloads.
- Studied a basic investigation workflow:
  1. View the image for anything unusual.
  2. Use `exiftool` to inspect metadata.
  3. Use `strings` to search for readable text.
  4. Use `binwalk` to identify embedded files or data.
  5. Use `file` to identify the file type.
  6. Try suitable steganography tools depending on the file format.
- Learned about tools such as `steghide`, `stegseek`, `zsteg`, StegSolve, OpenStego, and Aperi'Solve.
- Discovered that automated tools may not find everything, especially when hidden data is password-protected. A negative result does not always mean a file contains no hidden information.
- Also learned that extraction errors from `binwalk` may happen because required helper utilities are missing. In some cases, data can be extracted manually using `dd`.

### 4. Task 4: Security Through Obscurity

- Investigated a JPEG file that appeared normal but contained additional hidden data.
- Used `file` to identify the file and `binwalk` to discover a RAR archive appended at offset `74407`.
- Used `dd` to extract the archive into a separate file named `hidden.rar`.
- Used `unrar l` to list the archive's contents and `unrar x` to extract them.
- Finally, used `strings hackerchat.png | tail` to inspect readable text at the end of the file and find hidden content after the PNG `IEND` marker.
- Learned that data can remain hidden from normal image viewers even when it is simply appended to the file.
- This demonstrated why relying on appearance alone is not enough to determine whether a file is safe or contains concealed information.

## Tools Used

- **CyberChef:** Decoded binary, decimal ASCII, Base32, hexadecimal, Base64, Morse code, ROT13, and ROT47.
- **Audacity / Sonic Visualiser / Spek:** Viewed audio spectrograms to look for hidden messages.
- **Aperi'Solve:** Automated several image analysis and steganography checks.
- **`file`:** Identified file types.
- **`exiftool`:** Inspected file metadata.
- **`strings`:** Extracted readable text from files.
- **`binwalk`:** Detected embedded data and files.
- **`dd`:** Extracted data from a specific file offset.
- **`steghide`, `stegseek`, `zsteg`, StegSolve, OpenStego:** Explored tools for investigating hidden data in images.
- **`unrar`:** Listed and extracted RAR archives.

## Lesson Learned

I learned that solving CTF challenges requires more than just recognizing a hidden message. I need to identify the file format, choose the appropriate tool, and carefully examine the results one layer at a time. This room also taught me that encoding, encryption, and steganography are different concepts, and that a file can look completely normal while containing hidden information. Most importantly, I realized that automated tools are useful starting points, but I should not depend on them completely because they may miss data or require additional steps. These skills will help me become more systematic when investigating suspicious files and solving future cybersecurity challenges.