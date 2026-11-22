# ASCII FTW

**Platform:** CyLab Security Academy (picoCTF-style)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Author:** Abhishek Agarwal
**Date completed:** 2026-11-23

## Challenge
A compiled ELF binary (`asciiftw`) constructs the flag using hex ASCII values. Disassemble it to recover the flag text.

## Steps
1. `file asciiftw` confirmed it was an ELF64 executable (not readable as plain text).
2. Ran the binary directly: it printed `The flag starts with 61` — a hint, since `0x61 = 'a'` in ASCII, matching the standard `academy{...}` flag format.
3. `strings asciiftw` showed the literal format string `"The flag starts with %x"` in `.rodata`, confirming the printed number is just one hex byte being displayed via `printf`, not the whole flag.
4. Decided static disassembly was enough here rather than live GDB debugging — the challenge description says the flag is "constructed using hex ascii values," which signals fixed, compile-time constants rather than anything computed at runtime. No need to single-step through execution if nothing is dynamic.
5. Ran `objdump -d -M intel ./asciiftw` and found a long run of `mov BYTE PTR [rbp-X],0xYY` instructions inside `main`, each writing one literal byte into consecutive stack memory slots, in order from `[rbp-0x30]` up to `[rbp-0x12]`.
6. Read the `0xYY` byte out of each instruction in sequence and converted hex → ASCII for each one:

   | Offset | Hex | ASCII |
   |---|---|---|
   | rbp-0x30 | 0x61 | a |
   | rbp-0x2f | 0x63 | c |
   | rbp-0x2e | 0x61 | a |
   | rbp-0x2d | 0x64 | d |
   | rbp-0x2c | 0x65 | e |
   | rbp-0x2b | 0x6d | m |
   | rbp-0x2a | 0x79 | y |
   | rbp-0x29 | 0x7b | { |
   | rbp-0x28 | 0x41 | A |
   | rbp-0x27 | 0x53 | S |
   | rbp-0x26 | 0x43 | C |
   | rbp-0x25 | 0x49 | I |
   | rbp-0x24 | 0x49 | I |
   | rbp-0x23 | 0x5f | _ |
   | rbp-0x22 | 0x49 | I |
   | rbp-0x21 | 0x53 | S |
   | rbp-0x20 | 0x5f | _ |
   | rbp-0x1f | 0x45 | E |
   | rbp-0x1e | 0x41 | A |
   | rbp-0x1d | 0x53 | S |
   | rbp-0x1c | 0x59 | Y |
   | rbp-0x1b | 0x5f | _ |
   | rbp-0x1a | 0x33 | 3 |
   | rbp-0x19 | 0x36 | 6 |
   | rbp-0x18 | 0x38 | 8 |
   | rbp-0x17 | 0x46 | F |
   | rbp-0x16 | 0x42 | B |
   | rbp-0x15 | 0x36 | 6 |
   | rbp-0x14 | 0x43 | C |
   | rbp-0x13 | 0x31 | 1 |
   | rbp-0x12 | 0x7d | } |

7. Read the ASCII column top to bottom to get the full flag.

## Tools used
- `file`, `strings`, `objdump -d -M intel`

## Lesson learned
- The program's own printed output can be a decoy/partial hint, not the answer — "starts with 61" only confirms the first byte, the rest still has to be pulled from the disassembly.
- Deciding tool choice: if a value is baked into the binary at compile time (literal `mov ..., 0xYY` immediates with no runtime computation or branching involved), static disassembly (`objdump`) is faster and sufficient — no need to fire up GDB and step through execution.
- A sequence of `mov BYTE PTR [rbp-X], 0xYY` instructions writing to consecutive stack offsets is a classic pattern for a hardcoded string being assembled byte-by-byte on the stack.

## Flag
`academy{ASCII_IS_EASY_368FB6C1}`