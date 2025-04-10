# So Meta

## Task

*Find the flag in this **[picture](https://jupiter.challenges.picoctf.org/static/916b07b4c87062c165ace1d3d31ef655/pico_img.png)**.*

## Hints

1. *What does meta mean in the context of files?*
2. *Ever heard of metadata?*

## Input data

* `pico_img.png` file.

![pico_img.png](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/So%20Meta/pico_img.png)

## Analysis

### Image viewer

Image depicts a set of cocentrical partial circles.

### `file`

The file is, indeed, a PNG.

```
$ file pico_img.png 
pico_img.png: PNG image data, 600 x 600, 8-bit/color RGB, non-interlaced
```

### `strings`

The flag was found.

```
$ strings pico_img.png  | grep pico
picoCTF{s0_m3ta_d8944929}
```

### `exiftool`

Although the flag was already found, we did an extra check of metadata, because it is mentioned in the title.

```
$ exiftool pico_img.png 
ExifTool Version Number         : 12.42
File Name                       : pico_img.png
Directory                       : .
File Size                       : 109 kB
File Modification Date/Time     : 2025:04:03 15:27:44+03:00
File Access Date/Time           : 2025:04:03 15:28:53+03:00
File Inode Change Date/Time     : 2025:04:03 15:27:50+03:00
File Permissions                : -rw-rw-r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 600
Image Height                    : 600
Bit Depth                       : 8
Color Type                      : RGB
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Software                        : Adobe ImageReady
XMP Toolkit                     : Adobe XMP Core 5.3-c011 66.145661, 2012/02/06-14:56:27
Creator Tool                    : Adobe Photoshop CS6 (Windows)
Instance ID                     : xmp.iid:A5566E73B2B811E8BC7F9A4303DF1F9B
Document ID                     : xmp.did:A5566E74B2B811E8BC7F9A4303DF1F9B
Derived From Instance ID        : xmp.iid:A5566E71B2B811E8BC7F9A4303DF1F9B
Derived From Document ID        : xmp.did:A5566E72B2B811E8BC7F9A4303DF1F9B
Warning                         : [minor] Text/EXIF chunk(s) found after PNG IDAT (may be ignored by some readers)
Artist                          : picoCTF{s0_m3ta_d8944929}
Image Size                      : 600x600
Megapixels                      : 0.360
```

We found out that the flag was stored in the `Artist` field.

## Solution

So, the flag is `picoCTF{s0_m3ta_d8944929}`.

