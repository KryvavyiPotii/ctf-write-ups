# extensions

## Task

*This is a really weird text file **[TXT](https://jupiter.challenges.picoctf.org/static/e7e5d188621ee705ceeb0452525412ef/flag.txt)**?
Can you find the flag?*

### Hints

1. *How do operating systems know what kind of file it is? (It's not just the ending!*
2. *Make sure to submit the flag as picoCTF{XXXXX}*

## Input data

* `flag.txt` file

## Analysis

### `file`

Our text file turned out to be a PNG.

```console
$ file flag.txt
flag.txt: PNG image data, 1697 x 608, 8-bit/color RGB, non-interlaced
```

## Solution

File's extension was changed `.png`, so that we can look at it as an image.
After opening it in an image viewer we saw the flag `picoCTF{now_you_know_about_extensions}`.

![flag.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/extensions/flag.png)
