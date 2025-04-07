# The Numbers

## Task

*The **[numbers...](https://jupiter.challenges.picoctf.org/static/f209a32253affb6f547a585649ba4fda/the_numbers.png)** what do they mean?*

### Hints

*The flag is in the format PICOCTF{}*

## Input data

* `the_numbers.png`.

![Link to image](https://github.com/KryvavyiPotii/ctf-write-ups/blob/main/picoCTF/picoCTF%202019/The%20Numbers/the_numbers.png)

## Analysis

The text on the image looks like a flag because of curly braces.
The numbers seem to mean the number of a letter in alphabet.
For example, 1 corresponds to the 1st letter 'a', 2 - to the 2nd letter 'b' etc.
So, to get the flag we need to convert every number to corresponding letter.

## Solution

After converting every number to a letter we got `PICOCTF{THENUMBERSMASON}`.
