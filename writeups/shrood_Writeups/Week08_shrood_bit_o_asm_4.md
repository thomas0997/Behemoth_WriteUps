# Bit-O-Asm-4

**Platform:** CyLab Security Academy (picoCTF-style)
**Category:** Reverse Engineering
**Difficulty:** Easy
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-23

## Challenge
Same series as Bit-O-Asm-1/2/3, but this one adds a conditional branch (`cmp` + `jle`) — the first challenge in the set where the path `eax` takes actually depends on a comparison instead of running straight through.

## Dump
```
<+0>:     endbr64
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a
<+22>:    cmp    DWORD PTR [rbp-0x4],0x2710
<+29>:    jle    0x55555555514e <main+37>
<+31>:    sub    DWORD PTR [rbp-0x4],0x65
<+35>:    jmp    0x555555555152 <main+41>
<+37>:    add    DWORD PTR [rbp-0x4],0x65
<+41>:    mov    eax,DWORD PTR [rbp-0x4]
<+44>:    pop    rbp
<+45>:    ret
```

## Steps
1. `[rbp-0x4]` starts at `0x9fe1a` = 654874 decimal.
2. `cmp DWORD PTR [rbp-0x4],0x2710` compares that value against `0x2710` = 10000 decimal. `cmp` doesn't store a result anywhere — it just sets CPU flags for the next conditional jump to read.
3. `jle <main+37>` means "jump if the compared value is **less than or equal to** 0x2710." Since 654874 is *not* ≤ 10000, the jump is **not taken** — execution falls straight through to the next line instead.
4. That next line, `sub DWORD PTR [rbp-0x4],0x65`, runs: `654874 − 101 = 654773`.
5. The following `jmp <main+41>` skips over the `add` branch entirely (that branch only runs if the `jle` *had* been taken), landing directly on `mov eax,DWORD PTR [rbp-0x4]`.
6. `eax` ends up holding `654773`.

## Mistakes along the way
- Initially misread the comparison direction — thought `<main+37>` (the jump target) was itself a value being compared, rather than a code address to jump to. The actual comparison already happened in the preceding `cmp`; the jump target is just "where to go if the condition holds."

## Tools used
- `cat`

## Lesson learned
- `cmp a, b` only sets flags — it never changes `a` or `b`, and never produces a value you can read directly. The *following* conditional jump is what interprets those flags.
- `jle` = "jump if less than or equal." If the condition is false, you don't jump — you just fall through to the very next instruction, which is easy to miss if you're only scanning for jump targets.
- A `jmp` right after a branch's "false" path is usually there to skip past the "true" path's code — classic if/else compiled down to asm.

## Flag
`academy{654773}`