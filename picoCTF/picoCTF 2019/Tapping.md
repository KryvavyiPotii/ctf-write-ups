# Tapping

## Task

*Theres tapping coming in from the wires. What's it saying `nc jupiter.challenges.picoctf.org 9422`.*

## Hints

1. *What kind of encoding uses dashes and dots?*
2. *The flag is in the format PICOCTF{}*

## Input data

Upon connection to 

```console
$ nc jupiter.challenges.picoctf.org 9422
.--. .. -.-. --- -.-. - ..-. { -- ----- .-. ... ...-- -.-. ----- -.. ...-- .---- ... ..-. ..- -. ..--- -.... ---.. ...-- ---.. ..--- ....- -.... .---- ----- }
```

## Analysis

The most well-known encoding that uses dots and dashes is [Morse code](https://en.wikipedia.org/wiki/Morse_code).
A received message resembles a string with every letter except for curly braces encoded with this code.
So, in order to get the flag, we need to decode every letter.

## Solution

With [morsecode.world](https://morsecode.world/international/translator.html) we decoded the message and got the flag `PICOCTF{M0RS3C0D31SFUN2683824610}`.
