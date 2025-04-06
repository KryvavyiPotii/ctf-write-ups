# Picker IV

## Input data

* `picker-IV`
    Binary file.
* `picker-IV.c`
    C source code.

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>


void print_segf_message(){
  printf("Segfault triggered! Exiting.\n");
  sleep(15);
  exit(SIGSEGV);
}

int win() {
  FILE *fptr;
  char c;

  printf("You won!\n");
  // Open file
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL)
  {
      printf("Cannot open file.\n");
      exit(0);
  }

  // Read contents from file
  c = fgetc(fptr);
  while (c != EOF)
  {
      printf ("%c", c);
      c = fgetc(fptr);
  }

  printf("\n");
  fclose(fptr);
}

int main() {
  signal(SIGSEGV, print_segf_message);
  setvbuf(stdout, NULL, _IONBF, 0); // _IONBF = Unbuffered

  unsigned int val;
  printf("Enter the address in hex to jump to, excluding '0x': ");
  scanf("%x", &val);
  printf("You input 0x%x\n", val);

  void (*foo)(void) = (void (*)())val;
  foo();
}
```

## Analysis

After a brief look at the code we found out that we need to find an address of the `win` function in memory. 

## Solution

We found the `win` memory address with `gdb`. It was `0x40129e`.

```console
$ gdb -q picker-IV 
Reading symbols from picker-IV...
(No debugging symbols found in picker-IV)
(gdb) disas win
Dump of assembler code for function win:
   0x000000000040129e <+0>:	endbr64
   ...
```

Entering it to the remote instance gave us the flag:
`picoCTF{n3v3r_jump_t0_u53r_5uppl13d_4ddr35535_14bc5444}`
