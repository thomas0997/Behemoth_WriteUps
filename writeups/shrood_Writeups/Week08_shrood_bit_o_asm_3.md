# Bit-O-Asm-3

**Platform:** CyLab Security Academy (picoCTF-style)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-23

## Challenge
Same format as Bit-O-Asm-1/2, but this one does actual arithmetic on the way to `eax` — a multiply and an add.

## Dump
```
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0xc],0x9fe1a
<+22>:    mov    DWORD PTR [rbp-0x8],0x4
<+29>:    mov    eax,DWORD PTR [rbp-0xc]
<+32>:    imul   eax,DWORD PTR [rbp-0x8]
<+36>:    add    eax,0x1f5
<+41>:    mov    DWORD PTR [rbp-0x4],eax
<+44>:    mov    eax,DWORD PTR [rbp-0x4]
<+47>:    pop    rbp
<+48>:    ret
```

## Steps
1. Two constants get stashed in memory first: `[rbp-0xc] = 0x9fe1a` and `[rbp-0x8] = 0x4`.
2. `eax` is loaded with `[rbp-0xc]` → `eax = 0x9fe1a` (654874 decimal).
3. `imul eax,DWORD PTR [rbp-0x8]` multiplies `eax` in-place by the value at `[rbp-0x8]` → `eax = 654874 × 4 = 2619496`.
4. `add eax,0x1f5` adds `0x1f5` (501 decimal) → `eax = 2619496 + 501 = 2619997`.
5. The result gets stored back to `[rbp-0x4]` and immediately reloaded into `eax` — that last round-trip doesn't change the value, it's just how the compiler laid out the stack variable.

## Tools used
- `cat`

## Lesson learned
- `imul reg, src` multiplies the register in place by `src` and stores the result back in the register — same in-place pattern as `add`/`sub`.
- When a dump does several operations in a row, walk it top to bottom tracking one running value rather than trying to read the final answer off any single line.

## Flag
`academy{2619997}`