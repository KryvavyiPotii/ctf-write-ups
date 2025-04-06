# Bit-O-Asm 2

## Input data

* `disassembler-dump0_b.txt`.
    Text file with x86-64 assembly listing.

```assembly
<+0>:     endbr64 
<+4>:     push   rbp
<+5>:     mov    rbp,rsp
<+8>:     mov    DWORD PTR [rbp-0x14],edi
<+11>:    mov    QWORD PTR [rbp-0x20],rsi
<+15>:    mov    DWORD PTR [rbp-0x4],0x9fe1a // = 654874 => picoCTF{654874}
<+22>:    mov    eax,DWORD PTR [rbp-0x4]
<+25>:    pop    rbp
<+26>:    ret
```

## Analysis

The last value of `eax` was `DWORD PTR [rbp-0x4]` that was assigned at `<+22>`.
`DWORD PTR [rbp-0x4]` corresponds to `0x9fe1a` on a stack.
We need to convert it to decimal.

## Solution

`0x9fe1a` in decimal is `654874`. So, our flag is `picoCTF{654874}`.
