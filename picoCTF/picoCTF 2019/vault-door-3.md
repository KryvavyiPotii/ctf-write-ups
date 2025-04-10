# vault-door-3

## Task

*This vault uses for-loops and byte arrays. The source code for this vault is here: **VaultDoor3.java***

### Hints

*Make a table that contains each value of the loop variables and the corresponding buffer index that it writes to.*

## Input data

* `VaultDoor3.java`. Java source code.

```java
import java.util.*;

class VaultDoor3 {
    public static void main(String args[]) {
        VaultDoor3 vaultDoor = new VaultDoor3();
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

    // Our security monitoring team has noticed some intrusions on some of the
    // less secure doors. Dr. Evil has asked me specifically to build a stronger
    // vault door to protect his Doomsday plans. I just *know* this door will
    // keep all of those nosy agents out of our business. Mwa ha!
    //
    // -Minion #2671
    public boolean checkPassword(String password) {
        if (password.length() != 32) {
            return false;
        }
        char[] buffer = new char[32];
        int i;
        for (i=0; i<8; i++) {
            buffer[i] = password.charAt(i);
        }
        for (; i<16; i++) {
            buffer[i] = password.charAt(23-i);
        }
        for (; i<32; i+=2) {
            buffer[i] = password.charAt(46-i);
        }
        for (i=31; i>=17; i-=2) {
            buffer[i] = password.charAt(i);
        }
        String s = new String(buffer);
        return s.equals("jU5t_a_sna_3lpm18gb41_u_4_mfr340");
    }
}
```

## Analysis

The source code has 2 functions: `main` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
There the entered password is checked by length and slices in loops with `charAt` method that works as an array index.
In these loops the order of checks is quite chaotic that makes static analysis harder.
Finally, the equality between scrambled user input and the `jU5t_a_sna_3lpm18gb41_u_4_mfr340` string is checked.
So, to get the password we need to revert scrambling of the `jU5t_a_sna_3lpm18gb41_u_4_mfr340` string.

## Solution

By replacing parts of source code in `vim` a Python script was made to recover the password.

```python
scrambled_password = list('jU5t_a_sna_3lpm18gb41_u_4_mfr340')
password = [''] * 32

i = 0

while i < 8:
    password[i] = scrambled_password[i]
    i += 1

while i < 16:
    password[23 - i] = scrambled_password[i]
    i += 1

while i < 32:
    password[46 - i] = scrambled_password[i]
    i += 2

i = 31
while i >= 17:
    password[i] = scrambled_password[i]
    i -= 2

print(''.join(password))
```

After execution of this script we got the password `jU5t_a_s1mpl3_an4gr4m_4_u_1fb380`.
So, our flag is `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_1fb380}`.
