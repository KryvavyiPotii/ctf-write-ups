# Easy1

## Task

*The one time pad can be cryptographically secure, but not when you know the key.
Can you solve this? We've given you the encrypted flag, key, and a table to help **UFJKXQZQUNB** with the key of **SOLVECRYPTO**. 
Can you use this table to solve it?*

### Hints

1. *Submit your answer in our flag format. For example, if your answer was 'hello', you would submit 'picoCTF{HELLO}' as the flag.*
2. *Please use all caps for the message.*

## Input data

* `table.txt` text file

```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z 
   +----------------------------------------------------
A | A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
B | B C D E F G H I J K L M N O P Q R S T U V W X Y Z A
C | C D E F G H I J K L M N O P Q R S T U V W X Y Z A B
D | D E F G H I J K L M N O P Q R S T U V W X Y Z A B C
E | E F G H I J K L M N O P Q R S T U V W X Y Z A B C D
F | F G H I J K L M N O P Q R S T U V W X Y Z A B C D E
G | G H I J K L M N O P Q R S T U V W X Y Z A B C D E F
H | H I J K L M N O P Q R S T U V W X Y Z A B C D E F G
I | I J K L M N O P Q R S T U V W X Y Z A B C D E F G H
J | J K L M N O P Q R S T U V W X Y Z A B C D E F G H I
K | K L M N O P Q R S T U V W X Y Z A B C D E F G H I J
L | L M N O P Q R S T U V W X Y Z A B C D E F G H I J K
M | M N O P Q R S T U V W X Y Z A B C D E F G H I J K L
N | N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
O | O P Q R S T U V W X Y Z A B C D E F G H I J K L M N
P | P Q R S T U V W X Y Z A B C D E F G H I J K L M N O
Q | Q R S T U V W X Y Z A B C D E F G H I J K L M N O P
R | R S T U V W X Y Z A B C D E F G H I J K L M N O P Q
S | S T U V W X Y Z A B C D E F G H I J K L M N O P Q R
T | T U V W X Y Z A B C D E F G H I J K L M N O P Q R S
U | U V W X Y Z A B C D E F G H I J K L M N O P Q R S T
V | V W X Y Z A B C D E F G H I J K L M N O P Q R S T U
W | W X Y Z A B C D E F G H I J K L M N O P Q R S T U V
X | X Y Z A B C D E F G H I J K L M N O P Q R S T U V W
Y | Y Z A B C D E F G H I J K L M N O P Q R S T U V W X
Z | Z A B C D E F G H I J K L M N O P Q R S T U V W X Y
```

## Analysis

After a small research with Google, we found out that this table is used in [Vigenere cipher](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher).
This table is a so called 'tabula recta'.
It is necessary to decrypt our flag.

## Solution

A simple Python 3 script was written to decrypt the flag with formula:

$$
M_i=D_K(C_i)=C_i - K_{i\mod len(K)}\mod26
$$
where $M_i$ - i-th letter of a plaintext, $C_i$ - i-th letter of a ciphertext, $K_x$ - x-th letter of a key.

```python
ALPHABET_LEN = 26
CHAR_OFFSET = 65


def vigenere_decrypt(key, ciphertext):
    plaintext = ''

    for i, char in enumerate(ciphertext):
        key_char = key[i % len(key)]
    
        plaintext += chr((ord(char) - ord(key_char)) % ALPHABET_LEN + CHAR_OFFSET)

    return plaintext


plaintext = vigenere_decrypt('SOLVECRYPTO', 'UFJKXQZQUNB')

print(f'picoCTF{{{plaintext}}}')
```

So, the flag is `picoCTF{CRYPTOISFUN}`.
