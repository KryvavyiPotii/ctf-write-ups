# Bit-O-Asm 1

## Input data

* `disassembler-dump0_a.txt`.
    Text file with x86-64 assembly listing.

```assembly
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x4],edi
<+11>:    mov    QWORD PTR [rbp-0x10],rsi
<+15>:    mov    eax,0x30
<+20>:    pop    rbp
<+21>:    ret
```

## Analysis

The last value of `eax` was `0x30` at `<+15>`. We need to convert it to decimal.

## Solution

`0x30` in decimal is `48`. So, our flag is `picoCTF{48}`.
