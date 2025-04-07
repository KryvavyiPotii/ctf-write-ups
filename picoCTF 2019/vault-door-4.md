# vault-door-4

## Task

*This vault uses ASCII encoding for the password. The source code for this vault is here: **VaultDoor4.java***

### Hints

1. *Use a search engine to find an "ASCII table".*
2. *You will also need to know the difference between octal, decimal, and hexadecimal numbers.*

## Input data

* `VaultDoor4.java`. Java source code.

```java
import java.util.*;

class VaultDoor4 {
    public static void main(String args[]) {
        VaultDoor4 vaultDoor = new VaultDoor4();
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

    // I made myself dizzy converting all of these numbers into different bases,
    // so I just *know* that this vault will be impenetrable. This will make Dr.
    // Evil like me better than all of the other minions--especially Minion
    // #5620--I just know it!
    //
    //  .:::.   .:::.
    // :::::::.:::::::
    // :::::::::::::::
    // ':::::::::::::'
    //   ':::::::::'
    //     ':::::'
    //       ':'
    // -Minion #7781
    public boolean checkPassword(String password) {
        byte[] passBytes = password.getBytes();
        byte[] myBytes = {
            106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
            0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
            0142, 0131, 0164, 063 , 0163, 0137, 0143, 061 ,
            '9' , '4' , 'f' , '7' , '4' , '5' , '8' , 'e' ,
        };
        for (int i=0; i<32; i++) {
            if (passBytes[i] != myBytes[i]) {
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
There the entered password is converted to bytes with `getBytes` method and is stored in the `passBytes` variable.
After that it is compared with every byte of the `myBytes` array.
In this array bytes are presented in decimal, hexadecimal, octal and as ASCII `char`'s.
So, to get the password we need to convert every element of the `myBytes` array to ASCII `char`'s.

## Solution

By replacing parts of source code in `vim` a Python script was made to recover the password.

```python
password_bytes = [106 , 85  , 53  , 116 , 95  , 52  , 95  , 98  ,
    0x55, 0x6e, 0x43, 0x68, 0x5f, 0x30, 0x66, 0x5f,
    '0142', '0131', '0164', '063' , '0163', '0137', '0143', '061' ,
    '9' , '4' , 'f' , '7' , '4' , '5' , '8' , 'e']

password = []

for b in password_bytes[0:8]:
    password.append(chr(b))

for b in password_bytes[8:16]:
    password.append(chr(int(b)))

for b in password_bytes[16:24]:
    password.append(chr(int(b, 8)))

for b in password_bytes[24:]:
    password.append(b)

print(''.join(password))
```

After execution of this script we got the password `jU5t_4_bUnCh_0f_bYt3s_c194f7458e`.
So, our flag is `picoCTF{jU5t_4_bUnCh_0f_bYt3s_c194f7458e}`.
