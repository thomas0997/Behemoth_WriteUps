# Bit-O-Asm-1

**Platform:** CyLab Security Academy (picoCTF-style)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-23

## Challenge
Given a static assembly dump, figure out what ends up in the `eax` register. Answer format: `academy{n}` where `n` is the decimal value.

## Dump
```
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```

## Steps
1. `file` confirmed the dump was plain ASCII text (just a disassembly listing, not a binary), so no disassembler tool was even needed — `cat` was enough.
2. Scanned for the one line that actually touches `eax` directly: `mov eax,0x30`. Everything above it (`push rbp`, `mov rbp,rsp`, the `edi`/`rsi` stores) is just standard function-prologue noise that doesn't affect the final register value — ignored per the challenge's own hint.
3. Converted `0x30` to decimal.

## Mistakes along the way
- First pass: miscalculated `0x30` as `40` decimal. Actual conversion: `0x30 = 3×16 + 0 = 48`.

## Tools used
- `cat`, `file`

## Lesson learned
- Not every line in a disassembly dump matters — isolate the instruction that writes to the register the question actually asks about.
- Hex-to-decimal by hand: multiply each hex digit by its place value (16^1, 16^0, ...) and sum — don't eyeball it.

## Flag
`academy{48}`