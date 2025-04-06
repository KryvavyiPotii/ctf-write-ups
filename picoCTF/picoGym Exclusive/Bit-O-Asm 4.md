# Bit-O-Asm 4

## Input data

* `disassembler-dump0_d.txt`.
    Text file with x86-64 assembly listing.

```assembly
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

## Analysis

The last value of `eax` was `DWORD PTR [rbp-0x4]` that was assigned at `<+41>`.
At first `DWORD PTR [rbp-0x4]` corresponded to `0x9fe1a` on a stack.
At `<+22>` it was compared with `0x2710`.
Because `0x9fe1a` is greater than `0x2710`, conditional jump at `<+22>` was not done.
After that `0x9fe1a` was subtracted with `0x65`.
The result of this calculation is `0x9fe1a - 0x65 = 0x9fdb5`.
So, we need to convert the result in `DWORD PTR [rbp-0x4]` to decimal.

## Solution

`0x9fdb5` in decimal is `654773`. So, our flag is `picoCTF{654773}`.
