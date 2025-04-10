# john_pollard

## Task

*Sometimes RSA **[certificates](https://jupiter.challenges.picoctf.org/static/c882787a19ed5d627eea50f318d87ac5/cert)** are breakable*

## Hints

1. *The flag is in the format picoCTF{p,q}*
2. *Try swapping p and q if it does not work*

## Input data

* `cert` file.

## Analysis

### `file`

The file is a PEM certificate.

```console
$ file cert
cert: PEM certificate
```

### [SSLShopper](https://www.sslshopper.com/certificate-decoder.html).

The tool recognized that the embedded key size is only 53 bit.

```
Key Size: 53 bit. This key size is too low and may be vulnerable. You should generate a new CSR with a key size of at least 2048 bit and get a new certificate.
```

It is small enough to allow us to brute-force the RSA private key.
For that we need to factorize the modulus which is a public parameter.

## Solution

We obtained the modulus with `openssl`.

```console
$ openssl x509 -in cert -noout -text | grep Modulus
                Modulus: 4966306421059967 (0x11a4d45212b17f)
```

As we can see from the output, it is 4966306421059967.
Its factoring was done with Python. 

```python
>>> modulus = 4966306421059967
>>> p = 2
>>> while modulus % p != 0:
...     p += 1
...     
>>> print(p)
67867967
>>> print(modulus / p)
73176001.0
```

The whole process took less than 10 seconds.
Our factors are 67867967 and 73176001 so the flag is `picoCTF{73176001,67867967}`.
