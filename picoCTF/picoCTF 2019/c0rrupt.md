# c0rrupt

## Task

*We found this **[file](https://jupiter.challenges.picoctf.org/static/ab30fcb7d47364b4190a7d3d40edb551/mystery)**. Recover the flag.*

### Hints

*Try fixing the file header*

## Input data

* `mystery` file.

## Analysis

### `file`

The file is just a binary data.

```console
$ file mystery
mystery: data
```

### `xxd`
  
File's header seems to be a malformed PNG header.

```console
$ xxd mystery | head -n 3
00000000: 8965 4e34 0d0a b0aa 0000 000d 4322 4452  .eN4........C"DR
00000010: 0000 066a 0000 0447 0802 0000 007c 8bab  ...j...G.....|..
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
$ xxd mystery | tail -n 3
00031890: 0000 53b6 cf00 0000 0060 caf6 1900 0000  ..S......`......
000318a0: 004c fcbf fff7 ff01 cf09 313c 6ff1 75b8  .L........1<o.u.
000318b0: 0000 0000 4945 4e44 ae42 6082            ....IEND.B`.
```

## Solution

It seems like `mystery` is a PNG file with a broken header.
So, we need to fix the header first. Errors were identified with [PNG file chunk inspector by Project Nayuki](https://www.nayuki.io/page/png-file-chunk-inspector).
They were fixed with [neovim](https://vi.stackexchange.com/questions/2232/how-can-i-use-vim-as-a-hex-editor).

```console
$ xxd mystery| head -n 6
00000000: 8950 4e47 0d0a 1a0a 0000 000d 4948 4452  .PNG........IHDR
00000010: 0000 066a 0000 0447 0802 0000 007c 8bab  ...j...G.....|..
00000020: 7800 0000 0173 5247 4200 aece 1ce9 0000  x....sRGB.......
00000030: 0004 6741 4d41 0000 b18f 0bfc 6105 0000  ..gAMA......a...
00000040: 0009 7048 5973 aa00 1625 0000 1625 0149  ..pHYs...%...%.I
00000050: 5224 f000 00ff a549 4441 5478 5eec bd3f  R$.....IDATx^..?
```

After that the file was properly identified as PNG.

```console
$ file mystery
mystery.png: PNG image data, 1642 x 1095, 8-bit/color RGB, non-interlaced
```

Now we can open the image in an image viewer and see the flag `picoCTF{c0rrupt10n_1847995}`.

![mystery.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/c0rrupt/mystery.png)
