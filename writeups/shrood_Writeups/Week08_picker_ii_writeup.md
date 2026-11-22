# Picker II

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Date completed:** 2026-11-22
- **Category:** Reverse Engineering

## Summary
A follow-up to Picker I's `eval()`-on-raw-input bug, but this time a naive substring `filter()` tries to block calling `win()` directly by rejecting any input containing the literal text `"win"`. The filter only does a dumb string search — it doesn't understand that Python can construct the string `"win"` at runtime through concatenation, which completely sidesteps it.

## Steps
1. Downloaded and read `picker-II.py`. Same core vulnerability as Picker I — raw input gets `eval()`'d with `()` auto-appended — but now wrapped in:
   ```python
   def filter(user_input):
     if 'win' in user_input:
       return False
     return True
   ```
   So typing `win()` directly gets blocked with `Illegal input`.
2. **A lot of failed attempts first**, trying to find any way around it:
   - `win()` → blocked (`Illegal input`)
   - `esotric2()` (typo) → `name 'esotric2' is not defined`
   - `''`, `flag.txt`, `()`, `WIN`, `break`, `pass`, `generateRandomNumber` → various syntax errors or undefined names, none worked
   - `filter(win)` / `filter(win())` / `filter(WIN)` → blocked or undefined, since `win` as a bare word still contains the literal substring
   - `'w' + 'i' + 'n'` → ran fine but just evaluated to the string `'win'`, then errored since a plain string isn't callable (`'str' object is not callable`) — needed to actually *look up* the function by that name, not just construct the string
   - `niw`, `77 69 6E`, `119 105 110` (ASCII decimal attempts) → all invalid syntax or undefined, wrong approach entirely
   - `globals()['win']` → still blocked, because typing the literal characters `w`,`i`,`n` together inside the quotes still contains the substring `"win"` in the raw text sent to the filter — the filter checks the *text you typed*, not the evaluated result
3. **The breakthrough:** realized the filter never evaluates anything — it just substring-searches your raw input text. So the fix was to avoid ever typing `w`, `i`, `n` contiguously, while still having Python *construct* that exact string at runtime.
4. Tested the general technique locally first with a different word to confirm the mechanic:
   ```
   globals()['s'+'e'+'c'+'r'+'e'+'t']
   ```
   Sent to the server, this returned `'secret'` (an existing name in that script) — confirmed string concatenation inside the brackets avoids the literal-substring filter.
5. Applied the same technique to the actual target:
   ```
   globals()['w'+'i'+'n']
   ```
   The raw text has `'w'+'i'+'n'` — no contiguous `"win"` anywhere — so it passed the filter. The server auto-appends `()`, making the full executed line `globals()['w'+'i'+'n']()`, which looks up `win` by its runtime-constructed name and calls it.
6. This printed the flag as hex bytes, same format as Picker I:
   ```
   0x61 0x63 0x61 0x64 0x65 0x6d 0x79 0x7b 0x66 0x31 0x6c 0x37 0x33 0x72 0x35 0x5f 0x66 0x34 0x31 0x6c 0x5f 0x63 0x30 0x64 0x33 0x5f 0x72 0x33 0x66 0x34 0x63 0x37 0x30 0x72 0x5f 0x6d 0x31 0x67 0x68 0x37 0x5f 0x35 0x75 0x63 0x63 0x33 0x33 0x64 0x5f 0x63 0x39 0x61 0x61 0x37 0x61 0x33 0x63 0x7d
   ```
7. Decoded the hex bytes back to ASCII to get the plaintext flag.

## Tools used
- `nc` — connected to and interacted with the challenge service
- Direct source code review — identified the exact filter logic to defeat it precisely rather than guessing blindly
- Local Python shell (`python3`) — validated the string-construction bypass technique on a harmless test case (`secret`) before using it on the real target over the live connection
- CyberChef "From Hex" — decoded the flag's hex byte output back to readable text

## Lesson learned
A substring blocklist filter (`if 'word' in input`) only ever sees the exact text you typed — it has zero awareness of what that text actually evaluates to once it reaches `eval()`. Any way of constructing the blocked string at runtime instead of typing it literally (concatenation, reversal, character codes, etc.) defeats it completely. This is also why the challenge's own flag text says "filters fail, code refactor might succeed" — a blocklist is fundamentally the wrong fix for an `eval()`-on-raw-input vulnerability; the real fix would be removing `eval()` entirely, not trying to pattern-match against malicious-looking input. Also: testing the bypass technique on something harmless (`secret`) in a safe sandbox first, before trying it on the actual target over the network, saved a lot of wasted connection attempts once the real technique was confirmed to work.

## Flag
```
academy{f1l73r5_f41l_c0d3_r3f4c70r_m1gh7_5ucc33d_c9aa7a3c}
```