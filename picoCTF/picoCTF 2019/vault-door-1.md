# vault-door-1

## Task

*This vault uses some complicated arrays! I hope you can make sense of it, special agent. The source code for this vault is here: **VaultDoor1.java***

### Hints

*Look up the `charAt()` method online.*

## Input data

* `VaultDoor1.java`. Java source code.

```java
import java.util.*;

class VaultDoor1 {
    public static void main(String args[]) {
        VaultDoor1 vaultDoor = new VaultDoor1();
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

    // I came up with a more secure way to check the password without putting
    // the password itself in the source code. I think this is going to be
    // UNHACKABLE!! I hope Dr. Evil agrees...
    //
    // -Minion #8728
    public boolean checkPassword(String password) {
        return password.length() == 32 &&
               password.charAt(0)  == 'd' &&
               password.charAt(29) == 'a' &&
               password.charAt(4)  == 'r' &&
               password.charAt(2)  == '5' &&
               password.charAt(23) == 'r' &&
               password.charAt(3)  == 'c' &&
               password.charAt(17) == '4' &&
               password.charAt(1)  == '3' &&
               password.charAt(7)  == 'b' &&
               password.charAt(10) == '_' &&
               password.charAt(5)  == '4' &&
               password.charAt(9)  == '3' &&
               password.charAt(11) == 't' &&
               password.charAt(15) == 'c' &&
               password.charAt(8)  == 'l' &&
               password.charAt(12) == 'H' &&
               password.charAt(20) == 'c' &&
               password.charAt(14) == '_' &&
               password.charAt(6)  == 'm' &&
               password.charAt(24) == '5' &&
               password.charAt(18) == 'r' &&
               password.charAt(13) == '3' &&
               password.charAt(19) == '4' &&
               password.charAt(21) == 'T' &&
               password.charAt(16) == 'H' &&
               password.charAt(27) == '6' &&
               password.charAt(30) == 'f' &&
               password.charAt(25) == '_' &&
               password.charAt(22) == '3' &&
               password.charAt(28) == 'd' &&
               password.charAt(26) == 'f' &&
               password.charAt(31) == '4';
    }
}
```

## Analysis

The source code has 2 functions: `main` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
There the entered password is checked by length and symbols with `charAt` method that works as an array index.
From these `charAt`-checks we can derive the secret password.

## Solution

By replacing parts of source code in `vim` a Python script was made to recover the password.

```python
password = ['_' for _ in range(0, 32)]

password[0 ] = 'd'
password[29] = 'a'
password[4 ] = 'r'
password[2 ] = '5'
password[23] = 'r'
password[3 ] = 'c'
password[17] = '4'
password[1 ] = '3'
password[7 ] = 'b'
password[10] = '_'
password[5 ] = '4'
password[9 ] = '3'
password[11] = 't'
password[15] = 'c'
password[8 ] = 'l'
password[12] = 'H'
password[20] = 'c'
password[14] = '_'
password[6 ] = 'm'
password[24] = '5'
password[18] = 'r'
password[13] = '3'
password[19] = '4'
password[21] = 'T'
password[16] = 'H'
password[27] = '6'
password[30] = 'f'
password[25] = '_'
password[22] = '3'
password[28] = 'd'
password[26] = 'f'
password[31] = '4'

print(''.join(password))
```

After execution of this script we got the password `d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4`.
So, our flag is `picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4}`.
