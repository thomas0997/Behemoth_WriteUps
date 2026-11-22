# Picker I

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Date completed:** 2026-11-22
- **Category:** Reverse Engineering

## Summary
My first ever Reverse Engineering flag. A netcat service offers a menu that only advertises one function (`getRandomNumber`), but the downloadable source reveals several other hidden functions defined in the script — including a `win()` function that reads and prints the flag. Since the program routes user input straight into Python's `eval()`, calling any function defined in the script (not just the advertised one) is possible.

## Steps
1. Connected with `nc xebec.cylabacademy.net 20563` and saw it only prompts: `Try entering "getRandomNumber" without the double quotes...`
2. Downloaded the source (`picker-I.py`) to see what's actually running server-side instead of trusting the prompt's suggestion.
3. Reading the source, found the real logic at the bottom:
   ```python
   while(True):
     try:
       print('Try entering "getRandomNumber" without the double quotes...')
       user_input = input('==> ')
       eval(user_input + '()')
     except Exception as e:
       print(e)
       break
   ```
   This takes raw input, appends `()`, and `eval()`s it directly — meaning whatever name you type gets called as a function, not just the one suggested.
4. Scanned the rest of the file for other defined functions and found `win()`:
   ```python
   def win():
     flag = open('flag.txt', 'r').read()
     flag = flag.strip()
     str_flag = ''
     for c in flag:
       str_flag += str(hex(ord(c))) + ' '
     print(str_flag)
   ```
   This reads `flag.txt` and prints it out as a space-separated list of hex byte values (not plaintext).
5. Went back to the netcat connection and entered `win()` directly at the prompt instead of the suggested `getRandomNumber`.
6. The server printed the flag as hex bytes:
   ```
   0x61 0x63 0x61 0x64 0x65 0x6d 0x79 0x7b 0x34 0x5f 0x64 0x31 0x34 0x6d 0x30 0x6e 0x64 0x5f 0x31 0x6e 0x5f 0x37 0x68 0x33 0x5f 0x72 0x30 0x75 0x67 0x68 0x5f 0x33 0x33 0x39 0x38 0x34 0x31 0x38 0x38 0x7d
   ```
7. Decoded the hex bytes back to ASCII (same technique as the ASCII Numbers challenge — CyberChef "From Hex") to get the plaintext flag.

## Tools used
- `nc` — connected to and interacted with the challenge service
- Direct source code review — the actual vulnerability (unrestricted `eval()`) was only visible by reading the downloadable `.py` file, not from the netcat prompt alone
- CyberChef "From Hex" — decoded the flag's hex byte output back to readable text

## Lesson learned
The hint ("Can you point the program to a function that does somet[hing else]") was telling me exactly what to look for: the program exposes an `eval()` call on raw user input, so the real attack surface isn't the one function it advertises — it's every function defined anywhere in the source. Always read the full source when it's provided rather than only interacting with what the service prompts you toward; the intended path and the actual vulnerable path are often different by design in these challenges. Also: an `eval()`-based input handler with no restriction on which function name gets called is a textbook arbitrary-function-execution bug, regardless of language — same category of issue as command injection, just via Python's own `eval`.

## Flag
```
academy{4_d14m0nd_1n_7h3_r0ugh_33984188}
```