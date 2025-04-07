# caesar

## Task

*Decrypt this **message**.*

### Hints

*caesar cipher **[tutorial](https://learncryptography.com/classical-encryption/caesar-cipher)***

## Input data

* `ciphertext` file.

```console
$ cat ciphertext
picoCTF{dspttjohuifsvcjdpoabrkttds}
```

## Analysis

It seems that our goal is to use Caesar cipher to decrypt text inside curly brackets.

## Solution

We used [cryptii](https://cryptii.com/pipes/caesar-cipher) for decryption.
After several attempts the right key (shift) was found - **27**. It gave this plaintext:
`crossingtherubiconzaqjsscr`
So, the flag is `picoCTF{crossingtherubiconzaqjsscr}`
