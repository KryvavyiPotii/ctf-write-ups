# vault-door-5

## Task

*In the last challenge, you mastered octal (base 8), decimal (base 10), and hexadecimal (base 16) numbers, but this vault door uses a different change of base as well as URL encoding! 
The source code for this vault is here: **VaultDoor5.java***

### Hints

1. *You may find an encoder/decoder tool helpful, such as https://encoding.tools/*
2. *Read the wikipedia articles on URL encoding and base 64 encoding to understand how they work and what the results look like.*

## Input data

* `VaultDoor5.java`. Java source code.

```java
import java.net.URLDecoder;
import java.util.*;

class VaultDoor5 {
    public static void main(String args[]) {
        VaultDoor5 vaultDoor = new VaultDoor5();
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

    // Minion #7781 used base 8 and base 16, but this is base 64, which is
    // like... eight times stronger, right? Riiigghtt? Well that's what my twin
    // brother Minion #2415 says, anyway.
    //
    // -Minion #2414
    public String base64Encode(byte[] input) {
        return Base64.getEncoder().encodeToString(input);
    }

    // URL encoding is meant for web pages, so any double agent spies who steal
    // our source code will think this is a web site or something, defintely not
    // vault door! Oh wait, should I have not said that in a source code
    // comment?
    //
    // -Minion #2415
    public String urlEncode(byte[] input) {
        StringBuffer buf = new StringBuffer();
        for (int i=0; i<input.length; i++) {
            buf.append(String.format("%%%2x", input[i]));
        }
        return buf.toString();
    }

    public boolean checkPassword(String password) {
        String urlEncoded = urlEncode(password.getBytes());
        String base64Encoded = base64Encode(urlEncoded.getBytes());
        String expected = "JTYzJTMwJTZlJTc2JTMzJTcyJTc0JTMxJTZlJTY3JTVm"
                        + "JTY2JTcyJTMwJTZkJTVmJTYyJTYxJTM1JTY1JTVmJTM2"
                        + "JTM0JTVmJTMwJTYyJTM5JTM1JTM3JTYzJTM0JTY2";
        return base64Encoded.equals(expected);
    }
}
```

## Analysis

The source code has 4 functions: `main`, `base64Encode`, `urlEncode` and `checkPassword`.
`main` receives some password from user and then checks if it is correct by calling `checkPassword`.
There the entered password is URL-encoded with `urlEncode` and then Base64-encoded with `base64Encode`.
After that the encoded user password is compared with the `expected` string.
So, to get the password we need to decode the `expected` string.

## Solution

We used [CyberChef](https://cyberchef.org/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)URL_Decode()&input=SlRZekpUTXdKVFpsSlRjMkpUTXpKVGN5SlRjMEpUTXhKVFpsSlRZM0pUVm1KVFkySlRjeUpUTXdKVFprSlRWbUpUWXlKVFl4SlRNMUpUWTFKVFZtSlRNMkpUTTBKVFZtSlRNd0pUWXlKVE01SlRNMUpUTTNKVFl6SlRNMEpUWTI) with the `From Base64` and `URL Decode` operations to decode the `expected` string.
That way we got the password `c0nv3rt1ng_fr0m_ba5e_64_0b957c4f`.
So, our flag is `picoCTF{c0nv3rt1ng_fr0m_ba5e_64_0b957c4f}`.
