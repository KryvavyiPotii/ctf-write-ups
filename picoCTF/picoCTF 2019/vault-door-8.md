# vault-door-8

## Task

*Apparently Dr. Evil's minions knew that our agency was making copies of their source code, 
because they intentionally sabotaged this source code in order to make it harder for our agents to analyze and crack into! 
The result is a quite mess, but I trust that my best special agent will find a way to solve it. 
The source code for this vault is here: **VaultDoor8.java***

### Hints

1. *Clean up the source code so that you can read it and understand what is going on.*
2. *Draw a diagram to illustrate which bits are being switched in the scramble() method,
    then figure out a sequence of bit switches to undo it.
    You should be able to reuse the switchBits() method as is.*

## Input data

* `VaultDoor8.java`. Java source code.

```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*; import javax.crypto.Cipher; import javax.crypto.spec.SecretKeySpec;
import java.security.*; class VaultDoor8 {public static void main(String args[]) {
Scanner b = new Scanner(System.in); System.out.print("Enter vault password: ");
String c = b.next(); String f = c.substring(8,c.length()-1); VaultDoor8 a = new VaultDoor8(); if (a.checkPassword(f)) {System.out.println("Access granted."); }
else {System.out.println("Access denied!"); } } public char[] scramble(String password) {/* Scramble a password by transposing pairs of bits. */
char[] a = password.toCharArray(); for (int b=0; b<a.length; b++) {char c = a[b]; c = switchBits(c,1,2); c = switchBits(c,0,3); /* c = switchBits(c,14,3); c = switchBits(c, 2, 0); */ c = switchBits(c,5,6); c = switchBits(c,4,7);
c = switchBits(c,0,1); /* d = switchBits(d, 4, 5); e = switchBits(e, 5, 6); */ c = switchBits(c,3,4); c = switchBits(c,2,5); c = switchBits(c,6,7); a[b] = c; } return a;
} public char switchBits(char c, int p1, int p2) {/* Move the bit in position p1 to position p2, and move the bit
that was in position p2 to position p1. Precondition: p1 < p2 */ char mask1 = (char)(1 << p1);
char mask2 = (char)(1 << p2); /* char mask3 = (char)(1<<p1<<p2); mask1++; mask1--; */ char bit1 = (char)(c & mask1); char bit2 = (char)(c & mask2); /* System.out.println("bit1 " + Integer.toBinaryString(bit1));
System.out.println("bit2 " + Integer.toBinaryString(bit2)); */ char rest = (char)(c & ~(mask1 | mask2)); char shift = (char)(p2 - p1); char result = (char)((bit1<<shift) | (bit2>>shift) | rest); return result;
} public boolean checkPassword(String password) {char[] scrambled = scramble(password); char[] expected = {
0xF4, 0xC0, 0x97, 0xF0, 0x77, 0x97, 0xC0, 0xE4, 0xF0, 0x77, 0xA4, 0xD0, 0xC5, 0x77, 0xF4, 0x86, 0xD0, 0xA5, 0x45, 0x96, 0x27, 0xB5, 0x77, 0xE0, 0x95, 0xF1, 0xE1, 0xE0, 0xA4, 0xC0, 0x94, 0xA4 }; return Arrays.equals(scrambled, expected); } }
```

## Analysis

The source code is unreadable.
It was beautified manually and with [Code Beautify](https://codebeautify.org/javaviewer).

```java
// These pesky special agents keep reverse engineering our source code and then
// breaking into our secret vaults. THIS will teach those sneaky sneaks a
// lesson.
//
// -Minion #0891
import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;

class VaultDoor8 {
  public static void main(String args[]) {
    Scanner b = new Scanner(System.in);
    System.out.print("Enter vault password: ");
    String c = b.next();
    String f = c.substring(8, c.length() - 1);
    VaultDoor8 a = new VaultDoor8();
    if (a.checkPassword(f)) {
      System.out.println("Access granted.");
    } else {
      System.out.println("Access denied!");
    }
  }
  
  public char[] scramble(String password) {
    /* Scramble a password by transposing pairs of bits. */
    char[] a = password.toCharArray();
    for (int b = 0; b < a.length; b++) {
      char c = a[b];
      c = switchBits(c, 1, 2);
      c = switchBits(c, 0, 3);
      c = switchBits(c, 5, 6);
      c = switchBits(c, 4, 7);
      c = switchBits(c, 0, 1);
      c = switchBits(c, 3, 4);
      c = switchBits(c, 2, 5);
      c = switchBits(c, 6, 7);
      a[b] = c;
    }
    return a;
  }
  
  public char switchBits(char c, int p1, int p2) {
    /* Move the bit in position p1 to position p2, and move the bit
    that was in position p2 to position p1. Precondition: p1 < p2 */
    char mask1 = (char)(1 << p1);
    char mask2 = (char)(1 << p2);
    char bit1 = (char)(c & mask1);
    char bit2 = (char)(c & mask2);
    char rest = (char)(c & ~(mask1 | mask2));
    char shift = (char)(p2 - p1);
    char result = (char)((bit1 << shift) | (bit2 >> shift) | rest);
    return result;
  }
  
  public boolean checkPassword(String password) {
    char[] scrambled = scramble(password);
    char[] expected = {
      0xF4,
      0xC0,
      0x97,
      0xF0,
      0x77,
      0x97,
      0xC0,
      0xE4,
      0xF0,
      0x77,
      0xA4,
      0xD0,
      0xC5,
      0x77,
      0xF4,
      0x86,
      0xD0,
      0xA5,
      0x45,
      0x96,
      0x27,
      0xB5,
      0x77,
      0xE0,
      0x95,
      0xF1,
      0xE1,
      0xE0,
      0xA4,
      0xC0,
      0x94,
      0xA4
    };
    return Arrays.equals(scrambled, expected);
  }
}
```

The source code has 4 functions: `main`, `scramble`, `switchBits` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
There user input is scrambled with the `scramble` function that switches bits of every byte with the `switchBits` function.
After that the scrambled data is compared to the `expected` byte array that is the scrambled password.
So, in order to get the password, we need to reverse the `scramble` function and call it with the `expected` byte array as an argument.

## Solution

The operation was reversed with a Python script.

```python
ints = [
    1096770097,
    1952395366,
    1600270708,
    1601398833,
    1716808014,
    1734304867,
    942695730,
    942748212
]
password = ''

for i in ints:
    first_hex  = (i & 0b11111111000000000000000000000000) >> 24 
    second_hex = (i & 0b00000000111111110000000000000000) >> 16
    third_hex  = (i & 0b00000000000000001111111100000000) >> 8
    fourth_hex = i & 0b00000000000000000000000011111111

    password += chr(first_hex)
    password += chr(second_hex)
    password += chr(third_hex)
    password += chr(fourth_hex)


print(password)
```

That way we got the password `A_b1t_0f_b1t_sh1fTiNg_dc80e28124`.
So, our flag is `picoCTF{A_b1t_0f_b1t_sh1fTiNg_dc80e28124}`.
