# Investigative Reversing 3

## Task

*We have recovered a [binary](https://jupiter.challenges.picoctf.org/static/fd9d5bc48b1a6821ce8128672faf3edf/mystery) and 
an [image](https://jupiter.challenges.picoctf.org/static/fd9d5bc48b1a6821ce8128672faf3edf/encoded.bmp) 
See what you can make of it. There should be a flag somewhere.*

### Hints

**

## Input data

* `mystery` binary file.
* `encoded.bmp` image file.

![encoded.bmp](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%203/encoded.bmp)

## Analysis of `mystery`

### `file`

`mystery` is an executable.

```console
$ file mystery 
mystery: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=84b6f32deb8d6ef5099ab1fac1a24f3f273cfaa0, not stripped
```

### Static analysis with Ghidra

The function of interest is `main`.
Below we can see the decompiled by Ghidra and manually edited code. 

```c
int main(void) {
  int coded_char;
  size_t bytes_read;
  long in_FS_OFFSET;
  char char_buffer;
  char temp3;
  int temp;
  int i;
  int j;
  int k;
  undefined4 local_6c;
  int bytes_to_write;
  int temp2;
  FILE *flag_file;
  FILE *original_file;
  FILE *encoded_file;
  char flag_buffer [56];
  long local_10;

  // Opening the `flag.txt` file for reading, the `original.bmp` file for reading and
  // the `encoded.bmp` file for writing in append mode. 
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_6c = 0;
  flag_file = fopen("flag.txt","r");
  original_file = fopen("original.bmp","r");
  encoded_file = fopen("encoded.bmp","a");

  if (flag_file == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (original_file == (FILE *)0x0) {
    puts("No output found, please run this on the server");
  }

  // Appending 723 bytes from the `original.bmp` file to the `encoded.bmp` file. 
  bytes_read = fread(&char_buffer,1,1,original_file);
  temp = (int)bytes_read;
  bytes_to_write = 723;
  for (i = 0; i < bytes_to_write; i = i + 1) {
    fputc((int)char_buffer,encoded_file);
    bytes_read = fread(&char_buffer,1,1,original_file);
    temp = (int)bytes_read;
  }

  // Reading 50 bytes from the `flag.txt` file.
  bytes_read = fread(flag_buffer,50,1,flag_file);
  temp2 = (int)bytes_read;
  if (temp2 < 1) {
    puts("Invalid Flag");
                    /* WARNING: Subroutine does not return */
    exit(0);
  }

  // Appending 100 bytes to the `encoded.bmp` file from `flag.txt`
  // and `original.bmp` files.
  for (j = 0; j < 100; j = j + 1) {
    // Using the LSB steganography method on bytes with even indices...
    if ((j & 1U) == 0) {
      for (k = 0; k < 8; k = k + 1) {
        coded_char = codedChar(k,flag_buffer[j / 2],(int)char_buffer);
        temp3 = (char)coded_char;
        fputc((int)temp3,encoded_file);
        fread(&char_buffer,1,1,original_file);
      }
    }
    // ...and regular copying of bytes with odd indices.
    else {
      fputc((int)char_buffer,encoded_file);
      fread(&char_buffer,1,1,original_file);
    }
  }

  // Appending the rest bytes from the `original.bmp` file to the `encoded.bmp` file.
  while (temp == 1) {
    fputc((int)char_buffer,encoded_file);
    bytes_read = fread(&char_buffer,1,1,original_file);
    temp = (int)bytes_read;
  }

  fclose(encoded_file);
  fclose(original_file);
  fclose(flag_file);

  if (local_10 == *(long *)(in_FS_OFFSET + 0x28)) {
    return 0;
  }
                    /* WARNING: Subroutine does not return */
  __stack_chk_fail();
}
```

Encrytion happends in the `codedChar` function
It's decompiled code is given below.

```c
char codedChar(int index,char flag_char,char original_char) {
  char flag_char_bit;
  
  flag_char_bit = flag_char;
  if (index != 0) {
    flag_char_bit = (char)((int)flag_char >> ((byte)index & 0x1f));
  }
  return original_char & 0xfeU | flag_char_bit & 1U;
}
```

As we can see, this function returns the byte from the `original.bmp` with the last bit set to
a bit of a `flag.txt` byte.
So, in order to get the flag we need to extract 100 bytes at offset 723 from `encoded.bmp` and
decode it.
   
## Solution

Data extraction and decoding was done with Python.

```python

```

After executing this `get_flag.py` script we got our flag.

```console
$ python get_flag.py 

```

So, the flag is ``.
