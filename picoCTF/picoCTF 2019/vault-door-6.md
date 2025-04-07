# vault-door-6

## Task

*This vault uses an XOR encryption scheme. The source code for this vault is here: **VaultDoor6.java***

### Hints

*If X ^ Y = Z, then Z ^ Y = X. Write a program that decrypts the flag based on this fact.*

## Input data

* `VaultDoor6.java`. Java source code.

```java
import java.util.*;

class VaultDoor6 {
    public static void main(String args[]) {
        VaultDoor6 vaultDoor = new VaultDoor6();
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

    // Dr. Evil gave me a book called Applied Cryptography by Bruce Schneier,
    // and I learned this really cool encryption system. This will be the
    // strongest vault door in Dr. Evil's entire evil volcano compound for sure!
    // Well, I didn't exactly read the *whole* book, but I'm sure there's
    // nothing important in the last 750 pages.
    //
    // -Minion #3091
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
            0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
            0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
            0xa , 0x6c, 0x61, 0x6d, 0x37, 0x6d, 0x6d, 0x6d,
        };
        for (int i=0; i<32; i++) {
            if (((passBytes[i] ^ 0x55) - myBytes[i]) != 0) {
                return false;
            }
        }
        return true;
    }
}
```

## Analysis

The source code has 2 functions: `main` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
There the entered password is at first checked by length.
After that it is converted to bytes with the `getBytes` method.
Finally, it is checked with a operation (XOR with subtraction) in the loop.
This operation acts as an "encryption" method with the `myBytes` array as a key.
So, to get the password we need to reverse it.

## Solution

The operation was reversed with a Python script.

```python
sub_bytes = [0x3b, 0x65, 0x21, 0xa , 0x38, 0x0 , 0x36, 0x1d,
    0xa , 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0xa ,
    0x21, 0x1d, 0x61, 0x3b, 0xa , 0x2d, 0x65, 0x27,
    0xa , 0x6c, 0x61, 0x6d, 0x37, 0x6d, 0x6d, 0x6d]
password = [0] * 32

for i in range(0, 32):
    password[i] = chr(sub_bytes[i] ^ 0x55)

print(''.join(password))
```

That way we got the password `n0t_mUcH_h4rD3r_tH4n_x0r_948b888`.
So, our flag is `picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_948b888}`.
