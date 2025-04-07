# What Lies Within

## Task

*There's something in the **building**. Can you retrieve the flag?*

### Hints

*There is data encoded somewhere... there might be an online decoder.*

## Input data

* `buildings.png` file.

![buildings.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/What%20Lies%20Within/buildings.png)

## Analysis

### Image viewer

Image itself contains only a building.

### `stegoveritas`

Nothing interesting was found in metadata but some ASCII text was extracted.

```console
$ stegoveritas buildings.png 
Running Module: SVImage
+---------------------------+------+
|        Image Format       | Mode |
+---------------------------+------+
| Portable network graphics | RGBA |
+---------------------------+------+
Found something worth keeping!
ASCII text, with no line terminators
+---------+------------------+------------------------------------------------------------------------------------------------------+-----------+
| Offset  | Carved/Extracted | Description                                                                                          | File Name |
+---------+------------------+------------------------------------------------------------------------------------------------------+-----------+
| 0x8972  | Carved           | LZMA compressed data, properties: 0x5B, dictionary size: 0 bytes, uncompressed size: 144 bytes       | 8972.7z   |
| 0x8972  | Extracted        | LZMA compressed data, properties: 0x5B, dictionary size: 0 bytes, uncompressed size: 144 bytes       | 8972      |
| 0x11748 | Carved           | LZMA compressed data, properties: 0xD8, dictionary size: 16777216 bytes, uncompressed size: 32 bytes | 11748.7z  |
| 0x11748 | Extracted        | LZMA compressed data, properties: 0xD8, dictionary size: 16777216 bytes, uncompressed size: 32 bytes | 11748     |
+---------+------------------+------------------------------------------------------------------------------------------------------+-----------+
Found something worth keeping!
ASCII text, with no line terminators
Found something worth keeping!
Non-ISO extended-ASCII text, with no line terminators
Found something worth keeping!
dBase III DBT, version number 0, next free block index 2170473605
Found something worth keeping!
ISO-8859 text, with very long lines, with no line terminators
Found something worth keeping!
ISO-8859 text, with very long lines, with no line terminators
Found something worth keeping!
ISO-8859 text, with very long lines, with no line terminators
Found something worth keeping!
ASCII text, with no line terminators
Found something worth keeping!
ISO-8859 text, with very long lines, with no line terminators
WARNING:StegoVeritas:Modules:Image:Analysis:Filters:Error: not supported for this image mode. Op: autocontrast
WARNING:StegoVeritas:Modules:Image:Analysis:Filters:Error: not supported for this image mode. Op: equalize
WARNING:StegoVeritas:Modules:Image:Analysis:Filters:Error: not supported for this image mode. Op: invert
WARNING:StegoVeritas:Modules:Image:Analysis:Filters:Error: not supported for this image mode. Op: solarize
Running Module: MultiHandler

Found something worth keeping!
PNG image data, 657 x 438, 8-bit/color RGBA, non-interlaced
+--------+------------------+----------------------------------+-----------+
| Offset | Carved/Extracted | Description                      | File Name |
+--------+------------------+----------------------------------+-----------+
| 0x29   | Carved           | Zlib compressed data, compressed | 29.zlib   |
| 0x29   | Extracted        | Zlib compressed data, compressed | 29        |
+--------+------------------+----------------------------------+-----------+
Exif
====
+---------------------+------------------------------------------------------------------+
| key                 | value                                                            |
+---------------------+------------------------------------------------------------------+
| SourceFile          | /home/.../picoCTF/picoCTF 2019/What Lies Within/buildings.png    |
| ExifToolVersion     | 12.42                                                            |
| FileName            | buildings.png                                                    |
| Directory           | /home/.../picoCTF/picoCTF 2019/What Lies Within                  |
| FileSize            | 625 kB                                                           |
| FileModifyDate      | 2025:03:31 13:03:02+03:00                                        |
| FileAccessDate      | 2025:04:07 20:10:40+03:00                                        |
| FileInodeChangeDate | 2025:03:31 13:03:05+03:00                                        |
| FilePermissions     | -rw-rw-r--                                                       |
| FileType            | PNG                                                              |
| FileTypeExtension   | png                                                              |
| MIMEType            | image/png                                                        |
| ImageWidth          | 657                                                              |
| ImageHeight         | 438                                                              |
| BitDepth            | 8                                                                |
| ColorType           | RGB with Alpha                                                   |
| Compression         | Deflate/Inflate                                                  |
| Filter              | Adaptive                                                         |
| Interlace           | Noninterlaced                                                    |
| ImageSize           | 657x438                                                          |
| Megapixels          | 0.288                                                            |
+---------------------+------------------------------------------------------------------+
```

## Solution

The flag was found in one of the files inside the `stegoveritas` output directory - `./results/keepers`.

```console
$ cat 1744045997.9686785-981fa7d383f0861ecea2ecb1c30d0161
picoCTF{h1d1ng_1n_th3_b1t5}
```

So, the flag is `picoCTF{h1d1ng_1n_th3_b1t5}`.
