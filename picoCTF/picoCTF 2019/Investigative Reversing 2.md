# Investigative Reversing 2

## Task

*We have recovered a [binary](https://jupiter.challenges.picoctf.org/static/1d8e2ff583796340cf3eafbf81bf7b70/mystery) and 
an [image](https://jupiter.challenges.picoctf.org/static/1d8e2ff583796340cf3eafbf81bf7b70/encoded.bmp) See what you can make of it. 
There should be a flag somewhere.*

### Hints



## Input data

* `mystery` binary file.
* `encoded.bmp` image file.

![encoded.bmp](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%202/encoded.bmp)

## Analysis of `mystery`

### `file`

`mystery` is an executable.

```console
$ file mystery 
mystery: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=2d2155d1fe9b3de7809f36ce63468d6e9a9ebbf7, not stripped
```

### `strings`
    
Some `flag.txt`-, `encoded.bmp` and `original.bmp`-related strings were found.

```console
$ strings mystery
...
flag.txt
original.bmp
encoded.bmp
No flag found, please make sure this is run on the server
original.bmp is missing, please run this on the server
flag is not 50 chars
...
```

It seems like `mystery` needs `flag.txt` and `original.bmp` in order to function properly.

### Dynamic analysis with execution
    
Process terminates immediately with a segmentation fault after giving a message about absence of the `original.bmp` file.

```console
$ ./mystery 
No flag found, please make sure this is run on the server
original.bmp is missing, please run this on the server
Segmentation fault
```

Creating mentioned earlier `flag.txt` and `original.bmp` files makes the program terminate with an error related to flag length.
    
```console
$ echo 'CTF{you_won}' > flag.txt
$ echo 'binary data' > original.bmp
$ ./mystery 
flag is not 50 chars
```

So, these two files are keys to finding the flag.
    
### Static analysis with Ghidra

The function of interest is `main`.
Below we can see the decompiled by Ghidra and manually edited code. 

```c
int main(void) {
  size_t bytes_read;
  long in_FS_OFFSET;
  char char_buffer;
  char encoded_char;
  int temp;
  undefined4 local_6c;
  int bytes_to_write;
  int temp2;
  FILE *flag_file;
  FILE *original_file;
  FILE *encoded_file;
  char flag_buffer [56];
  long local_10;
  
  local_10 = *(long *)(in_FS_OFFSET + 0x28);
  local_6c = 0;
	
  flag_file = fopen("flag.txt","r");
  original_file = fopen("original.bmp","r");
  encoded_file = fopen("encoded.bmp","a");
  
  if (flag_file == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (original_file == (FILE *)0x0) {
    puts("original.bmp is missing, please run this on the server");
  }

  // Writing 2000 bytes from `original.bmp` to `encoded.bmp` byte by byte.
  bytes_read = fread(&char_buffer,1,1,original_file);
  temp = (int)bytes_read;
  bytes_to_write = 2000;
  for (int i = 0; i < bytes_to_write; i = i + 1) {
    fputc((int)char_buffer,encoded_file);
    bytes_read = fread(&char_buffer,1,1,original_file);
    temp = (int)bytes_read;
  }

  // Reading 50 bytes from `flag.txt` into `flag_buffer`.
  bytes_read = fread(flag_buffer,50,1,flag_file);
  temp2 = (int)bytes_read;
  if (temp2 < 1) {
    puts("flag is not 50 chars");
    exit(0);
  }

  // Encoding data in `flag_buffer` with the `char_buffer` data and the `k` index and
  // appending it to `encoded.bmp`.
  for (int j = 0; j < 50; j = j + 1) {
    for (int k = 0; k < 8; k = k + 1) {
      encoded_char = codedChar(k,flag_buffer[j] - 5,char_buffer);
      fputc((int)encoded_char,encoded_file);
      fread(&char_buffer,1,1,original_file);
    }
  }
  
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
  __stack_chk_fail();
}
```

As we can see, `mystery` opens `flag.txt`, `original.bmp` and `encoded.bmp`.
First, it reads 2000 bytes from `original.bmp` and 50 bytes from `flag.txt`.
Second, it encrypts the flag data with the `codedChar` function with `original.bmp` and `flag.txt` data as arguments and
appends it to `encoded.bmp`.
It's decompiled code is given below.

```c
char codedChar(int index,char flag_char,uchar original_char) {
  uchar encoded_char;
  
  encoded_char = flag_char;
  if (index != 0) {
    encoded_char = (uchar)((int)flag_char >> ((byte)index & 0x1f));
  }
  return original_char & 0xfe | encoded_char & 1;
}
```

Finally, `mystery` appends the rest of the `original.bmp` data to `encoded.bmp`.
So, in order to get the flag we need to extract 50 bytes at offset 2000 from `encoded.bmp` and decode it.
   
## Solution

Data extraction and decoding was done with Python.

```python

```

After executing this `get_flag.py` script we got our flag.

```console
$ python get_flag.py 
picoCTF{An0tha_1_9a47141}
```

So, the flag is `picoCTF{An0tha_1_9a47141}`.
