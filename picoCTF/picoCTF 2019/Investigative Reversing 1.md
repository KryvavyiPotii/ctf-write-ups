# Investigative Reversing 1

## Task

*We have recovered a [binary](https://jupiter.challenges.picoctf.org/static/c600f6c1bbe8969aefd6f9da0cbdc01c/mystery) and 
a few images: [image](https://jupiter.challenges.picoctf.org/static/c600f6c1bbe8969aefd6f9da0cbdc01c/mystery.png), 
[image2](https://jupiter.challenges.picoctf.org/static/c600f6c1bbe8969aefd6f9da0cbdc01c/mystery2.png), 
[image3](https://jupiter.challenges.picoctf.org/static/c600f6c1bbe8969aefd6f9da0cbdc01c/mystery3.png).
See what you can make of it. There should be a flag somewhere.*

### Hints

*1. Try using some forensics skills on the image*
*2. This problem requires both forensics and reversing skills*
*3. A hex editor may be helpful*

## Input data

* `mystery` binary file.
* `mystery.png` image file.

![mystery.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%201/mystery.png)

* `mystery2.png` image file.

![mystery2.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%201/mystery2.png)

* `mystery3.png` image file.

![mystery3.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%201/mystery3.png)

## Analysis of `mystery`

### `file`

`mystery` is an executable.

```console
$ file mystery
mystery: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=1b08f7a782a77a6eeb80d7c1d621b4f16f76200a, not stripped
```

### `strings`
    
Some `flag.txt`- and `mystery.png`-related strings were found.

```console
$ strings mystery
...
flag.txt
mystery.png
mystery2.png
mystery3.png
No flag found, please make sure this is run on the server
mystery.png is missing, please run this on the server
...
```

It seems like `mystery` needs `flag.txt` and `mystery.png` in order to function properly.

### Dynamic analysis with execution
    
Process terminates immediately with a segmentation fault after giving a message about absence of flag.

```console
$ ./mystery 
No flag found, please make sure this is run on the server
Segmentation fault
```

Creating a mentioned earlier `flag.txt` just makes the program terminate without errors.
    
```console
$ echo 'CTF{you_won}' > flag.txt
$ ./mystery
$
```
    
### Static analysis with Ghidra

The function of interest is `main`.
Below we can see it decompiled by Ghidra and manually edited. 

```c
void main(void) {
  long lVar1;
  FILE *flag_file;
  FILE *mystery_png_file;
  FILE *mystery2_png_file;
  FILE *mystery3_png_file;
  long in_FS_OFFSET;
  char flag_char_at_3;
  int i;
  int j;
  int k;
  char flag_buffer [26];
  
  lVar1 = *(long *)(in_FS_OFFSET + 0x28);

  flag_file = fopen("flag.txt","r");
  mystery_png_file = fopen("mystery.png","a");
  mystery2_png_file = fopen("mystery2.png","a");
  mystery3_png_file = fopen("mystery3.png","a");

  if (flag_file == (FILE *)0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (mystery_png_file == (FILE *)0) {
    puts("mystery.png is missing, please run this on the server");
  }

  fread(flag_buffer,26,1,flag_file);

  fputc((int)flag_buffer[1],mystery3_png_file);
  fputc((int)(char)(flag_buffer[0] + 21),mystery2_png_file);
  fputc((int)flag_buffer[2],mystery3_png_file);
  flag_char_at_3 = flag_buffer[3];
  fputc((int)flag_buffer[4],mystery_png_file);
  fputc((int)flag_buffer[5],mystery3_png_file);

  for (i = 6; i < 10; i = i + 1) {
    flag_char_at_3 = flag_char_at_3 + 1;
    fputc((int)flag_buffer[i],mystery_png_file);
  }

  fputc((int)flag_char_at_3,mystery2_png_file);

  for (j = 10; j < 15; j = j + 1) {
    fputc((int)flag_buffer[j],mystery3_png_file);
  }

  for (k = 15; k < 26; k = k + 1) {
    fputc((int)flag_buffer[k],mystery_png_file);
  }

  fclose(mystery_png_file);
  fclose(flag_file);

  if (lVar1 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }

  return;
}
```

As we can see, `mystery` opens `flag.txt` file for reading and stores 26 bytes of it to the `flag_buffer` char array.
After that it opens image files in append mode.
Then it appends data from the buffer to image files in this order and manner:
1. char 1 - appended to `mystery3.png`
2. char 0 - appended to `mystery2.png` with addition of 21
3. char 2 - appended to `mystery3.png`
4. char 4 - appended to `mystery.png`
5. char 5 - appended to `mystery3.png`
6. chars 6-9 - appended to `mystery.png`
7. char 3 - appended to `mystery2.png` with addition of 4
8. chars 10-14 - appended to `mystery3.png`
9. chars 15-25 - appended to `mystery.png`

To check this assumption a new `flag.txt` file was created that contains 26 'A' chars with Python.

```python
>>> with open('flag.txt', 'w') as f:
...     f.write('A' * 26)
...     
26
```

Also, new empty files were created to prevent modification of original image files.

```console
$ mv mystery.png mystery_orig.png
$ mv mystery2.png mystery2_orig.png
$ mv mystery3.png mystery3_orig.png
$ touch mystery.png
$ touch mystery2.png
$ touch mystery3.png
```

Execution of `mystery`, indeed, appended modified data to `mystery.png`, `mystery2.png` and `mystery3.png` from `flag.txt`.

```console
$ ./mystery 
$ cat mystery.png 
AAAAAAAAAAAAAAAA  # new line added manually for better readability
$ cat mystery2.png 
VE                # new line added manually for better readability
$ cat mystery3.png 
AAAAAAAA          # new line added manually for better readability
```

So, our assumption was right and further analysis of image files is pointless.
    
## Solution

Putting everything we discovered in Analysis stage together, we need to reverse flag "encryption" routine in the `main` function of `mystery`.
It was done with Python.

```python
IEND_CHUNK = b'IEND\xae\x42\x60\x82'
IEND_CHUNK_LEN = len(IEND_CHUNK)


def get_offset_to_data_after_iend(data):
    return data.find(IEND_CHUNK) + IEND_CHUNK_LEN


def get_data_after_iend(data):
    iend_offset = get_offset_to_data_after_iend(data)
    return data[iend_offset:]


def int_data_to_str(int_data):
    return ''.join([chr(x) for x in int_data])


def read_from_mystery_png(flag):
    with open('mystery.png', 'rb') as f:
        data = f.read()
        
        flag_data = get_data_after_iend(data)

        flag[4] = flag_data[0]
        flag[6:10] = flag_data[1:5]
        flag[15:] = flag_data[5:15]


def read_from_mystery2_png(flag):
    with open('mystery2.png', 'rb') as f:
        data = f.read()
        
        flag_data = get_data_after_iend(data)

        flag[0] = flag_data[0] - 21
        flag[3] = flag_data[1] - 4


def read_from_mystery3_png(flag):
    with open('mystery3.png', 'rb') as f:
        data = f.read()

        flag_data = get_data_after_iend(data)

        flag[1] = flag_data[0]
        flag[2] = flag_data[1]
        flag[5] = flag_data[2]
        flag[10:15] = flag_data[3:8]


def get_flag():
    flag = [b''] * 26
    
    read_from_mystery_png(flag)
    read_from_mystery2_png(flag)
    read_from_mystery3_png(flag)

    print(int_data_to_str(flag))


if __name__ == '__main__':
    get_flag()
```

After renaming original image files and executing this `get_flag.py` script we got our flag.

```console
$ python get_flag.py 
picoCTF{An0tha_1_9a47141}
```

So, the flag is `picoCTF{An0tha_1_9a47141}`.
