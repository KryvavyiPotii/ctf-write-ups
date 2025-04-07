# vault-door-7

## Task

*This vault uses bit shifts to convert a password string into an array of integers. 
Hurry, agent, we are running out of time to stop Dr. Evil's nefarious plans! The source code for this vault is here: **VaultDoor7.java***

### Hints

1. *Use a decimal/hexadecimal converter such as this one: https://www.mathsisfun.com/binary-decimal-hexadecimal-converter.html*
2. *You will also need to consult an ASCII table such as this one: https://www.asciitable.com/*

## Input data

* `VaultDoor7.java`. Java source code.

```java
import java.util.*;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.security.*;

class VaultDoor7 {
    public static void main(String args[]) {
        VaultDoor7 vaultDoor = new VaultDoor7();
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter vault password: ");
        String userInput = scanner.next();
	String input = userInput.substring("picoCTF{".length(),userInput.length()-1);
	if (vaultDoor.checkPassword(input)) {
	    System.out.println("Access granted.");
	} else {
	    System.out.println("Access denied!");
        }
    }

    // Each character can be represented as a byte value using its
    // ASCII encoding. Each byte contains 8 bits, and an int contains
    // 32 bits, so we can "pack" 4 bytes into a single int. Here's an
    // example: if the hex string is "01ab", then those can be
    // represented as the bytes {0x30, 0x31, 0x61, 0x62}. When those
    // bytes are represented as binary, they are:
    //
    // 0x30: 00110000
    // 0x31: 00110001
    // 0x61: 01100001
    // 0x62: 01100010
    //
    // If we put those 4 binary numbers end to end, we end up with 32
    // bits that can be interpreted as an int.
    //
    // 00110000001100010110000101100010 -> 808542562
    //
    // Since 4 chars can be represented as 1 int, the 32 character password can
    // be represented as an array of 8 ints.
    //
    // - Minion #7816
    public int[] passwordToIntArray(String hex) {
        int[] x = new int[8];
        byte[] hexBytes = hex.getBytes();
        for (int i=0; i<8; i++) {
            x[i] = hexBytes[i*4]   << 24
                 | hexBytes[i*4+1] << 16
                 | hexBytes[i*4+2] << 8
                 | hexBytes[i*4+3];
        }
        return x;
    }

    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        int[] x = passwordToIntArray(password);
        return x[0] == 1096770097
            && x[1] == 1952395366
            && x[2] == 1600270708
            && x[3] == 1601398833
            && x[4] == 1716808014
            && x[5] == 1734304867
            && x[6] == 942695730
            && x[7] == 942748212;
    }
}
```

## Analysis

The source code has 3 functions: `main`, `passwordToIntArray` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
The first check happens on the length of the entered user password.
After that the password is converted to an integer array with the `passwordToIntArray` function.
Then every obtained integer is compared to some specific integers that resemble a true password.
So, in order to get the true password, we need to reverse the `passwordToIntArray` function and call it with these constant integers as arguments.

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
