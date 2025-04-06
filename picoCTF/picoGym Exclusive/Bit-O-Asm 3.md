# Bit-O-Asm 3

## Input data

* `disassembler-dump0_c.txt`.
    Text file with x86-64 assembly listing.

```assembly
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

## Analysis

The last value of `eax` was `DWORD PTR [rbp-0x4]` that was assigned at `<+44>`.
At first `DWORD PTR [rbp-0x4]` corresponded to `0x9fe1a` on a stack.
However, later it was multiplied by `DWORD PTR [rbp-0x8]` that corresponded to `0x4` on the stack. 
After that `0x1f5` was added to it.
The result of these calculation is `0x9fe1a * 0x4 + 0x1f5 = 0x27FA5D`.
So, we need to convert the result in `DWORD PTR [rbp-0x4]` to decimal.

## Solution

`0x27FA5D` in decimal is `2619997`. So, our flag is `picoCTF{2619997}`.
