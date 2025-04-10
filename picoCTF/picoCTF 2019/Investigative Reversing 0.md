# Investigative Reversing 0

## Task

*We have recovered a **[binary](https://jupiter.challenges.picoctf.org/static/70fd416f817ab1e59beaf19dc2b586cd/mystery)** and an **[image](https://jupiter.challenges.picoctf.org/static/70fd416f817ab1e59beaf19dc2b586cd/mystery.png)**. See what you can make of it. There should be a flag somewhere.*

### Hints

*1. Try using some forensics skills on the image*
*2. This problem requires both forensics and reversing skills*
*3. A hex editor may be helpful*

## Input data

* `mystery` binary file.
* `mystery.png` image file.

![mystery.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/Investigative%20Reversing%200/mystery.png)

## Analysis of `mystery`

### `file`

`mystery` is an executable.

```console
$ file mystery
mystery: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=34b772a4f30594e2f30ac431c72667c3e10fa3e9, not stripped
```

### `strings`
    
Some flag- and `mystery.png`-related strings were found.

```console
$ strings mystery
...
flag.txt
mystery.png
No flag found, please make sure this is run on the server
mystery.png is missing, please run this on the server
at insert
...
```

It seems like `mystery` needs `mystery.png` in order to function properly.

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
  FILE *png_file;
  size_t sVar2;
  long in_FS_OFFSET;
  int i;
  int j;
  char flag_buffer [26];
  
  lVar1 = *(long *)(in_FS_OFFSET + 0x28);
  flag_file = fopen("flag.txt","r");
  png_file = fopen("mystery.png","a");
  if (flag_file == (FILE *)0x0) {
    puts("No flag found, please make sure this is run on the server");
  }
  if (png_file == (FILE *)0x0) {
    puts("mystery.png is missing, please run this on the server");
  }
  sVar2 = fread(flag_buffer,26,1,flag_file);
  if ((int)sVar2 < 1) {
                    /* WARNING: Subroutine does not return */
    exit(0);
  }
  puts("at insert");
  fputc((int)flag_buffer[0],png_file);
  fputc((int)flag_buffer[1],png_file);
  fputc((int)flag_buffer[2],png_file);
  fputc((int)flag_buffer[3],png_file);
  fputc((int)flag_buffer[4],png_file);
  fputc((int)flag_buffer[5],png_file);
  for (i = 6; i < 15; i = i + 1) {
    fputc((int)(char)(flag_buffer[i] + '\x05'),png_file);
  }
  fputc((int)(char)(flag_buffer[0xf] + -3),png_file);
  for (j = 16; j < 26; j = j + 1) {
    fputc((int)flag_buffer[j],png_file);
  }
  fclose(png_file);
  fclose(flag_file);
  if (lVar1 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
    __stack_chk_fail();
  }
  return;
}
```

As we can see, `mystery` opens `flag.txt` file for reading and stores 26 bytes of it to a buffer.
After that it opens `mystery.png` file in append mode.
Then it appends data from the buffer to `mystery.png`:
* bytes 0-5 - appended without modification
* bytes 6-14 - appended with addition of 5
* byte 15 - appended with subtraction of 3
* bytes 16-26 - appended without modification

To check this assumption a new `flag.txt` file was created that contains 26 'A' chars with Python.

```python
>>> with open('flag.txt', 'w') as f:
...     f.write('A' * 26)
...     
26
```

Also, a new empty `mystery.png` file was created.

```console
$ mv mystery.png mystery_orig.png
$ touch mystery.png
```

Execution of `mystery`, indeed, appended modified data to `mystery.png` from `flag.txt`.

```console
$ ./mystery 
at insert
$ cat mystery.png 
AAAAAAFFFFFFFFF>AAAAAAAAAA
```

So, our assumption was right.


## Analysis of `mystery.png`

### `file`

`mystery.png` is, indeed, a PNG.

```console
$ file mystery.png
mystery.png: PNG image data, 1411 x 648, 8-bit/color RGB, non-interlaced
```

### Image viewer
    
`mystery.png` depicts a text "WHERE'D The FLAG GO!".

### `exiftool`
    
There was some data found after IEND chunk.

```console
$ exiftool mystery.png | grep Warning
    Warning                         : [minor] Trailer data after PNG IEND chunk
```

### `xxd`

Apparently, the data found with `exiftool` seems similar to a flag.

```console
$ xxd mystery.png | tail -n 3
0001e860: ed5a 9d38 d01f 5600 0000 0049 454e 44ae  .Z.8..V....IEND.
0001e870: 4260 8270 6963 6f43 544b 806b 357a 7369  B`.picoCTK.k5zsi
0001e880: 6436 715f 6431 6465 6564 6161 7d         d6q_d1deedaa}
```
    
### `stegoveritas`

Nothing, except previously identified string was found.
    
```console
$ stegoveritas mystery.png
...
Discovered Trailing Data:
b'picoCTK\x80k5zsid6q_d1deedaa}'
...
```
    
## Solution

Putting everything we discovered in Analysis stage together, we need to reverse flag "encryption" routine in the `main` function of `mystery`.
It was done with `bash` (as an extra exercise):

```shell
hex_to_dec () {
    local hex=$1
    
    printf "%d\n" "0x$hex"
}

hex_array_to_dec_array () {
    local hex_array=("$@")
    local dec_array=()
    
    for h in "${hex_array[@]}"; do
        dec=$( hex_to_dec "$h" )
        dec_array+=( "$dec" )
    done
    
    echo "${dec_array[@]}"
}

ascii_code_to_char () {
    local ascii_code=$1
    
    echo $ascii_code | awk '{ printf "%c", $1}'
}

ascii_code_array_to_char_array () {
    local ascii_code_array=("$@")
    local char_array=()
    
    for ascii_code in "${ascii_code_array[@]}"; do
        local char=$( ascii_code_to_char "$ascii_code" )
        char_array+=( "$char" )
    done
    
    echo "${char_array[@]}"
    
}

decrypt_flag () {
    local encrypted_flag=("$@")
    local decrypted_flag=("${encrypted_flag[@]:0:6}")
    
    for ascii_code in ${encrypted_flag[@]:6:9}; do
        local modified_ascii_code=$(( $ascii_code - 5 ))
    	decrypted_flag+=( "$modified_ascii_code" )
    done
    
    local modified_ascii_code=$(( ${encrypted_flag[15]} + 3 ))
    decrypted_flag+=( "$modified_ascii_code" )
    decrypted_flag+=( "${encrypted_flag[@]:16}" )
    
    local decrypted_flag_in_ascii=$( ascii_code_array_to_char_array ${decrypted_flag[@]} )
    
    echo $decrypted_flag_in_ascii
}


INPUT_DATA=(
    70 69 63 6F 43 54 4B 80 
    6B 35 7A 73 69 64 36 71 
    5F 64 31 64 65 65 64 61 
    61 7D
)

encrypted_flag=$( hex_array_to_dec_array ${INPUT_DATA[@]} )
decrypted_flag=$( decrypt_flag ${encrypted_flag[@]} )

echo "$decrypted_flag" | sed -e 's/ //g'
```

As a result, we got the flag `picoCTF{f0und_1t_d1deedaa}`.
