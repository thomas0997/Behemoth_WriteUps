# Bit-O-Asm-2

**Platform:** CyLab Security Academy (picoCTF-style)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-23

## Challenge
Same setup as Bit-O-Asm-1 — figure out what ends up in `eax`, format `academy{n}` in decimal. This one introduces `DWORD PTR` memory references instead of a plain immediate move.

## Dump
```
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret
```

## Steps
1. Needed to understand `DWORD PTR [rbp-0x4]` first: it's not a value, it's an address — "the 4-byte memory location at `rbp - 0x4`". `mov DWORD PTR [rbp-0x4],0x9fe1a` writes the constant `0x9fe1a` into that memory slot.
2. The next instruction, `mov eax,DWORD PTR [rbp-0x4]`, reads that same memory slot back out into `eax`. So `eax` just ends up holding `0x9fe1a`.
3. Converted `0x9fe1a` to decimal.

## Tools used
- `cat`

## Lesson learned
- A value doesn't have to be written directly into a register to end up there — it can get stashed in a stack memory slot first (`[rbp-offset]`) and read back out later. Track what's stored where, not just what each register shows at a glance.
- `DWORD PTR` just tells you the size being accessed (4 bytes here); it doesn't change the math, only how much memory is touched.

## Flag
`academy{654874}`