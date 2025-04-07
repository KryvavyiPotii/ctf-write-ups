# WPA-ing Out

## Input data

* `wpa-ing_out.pcap`. Pcap capture file.

## Analysis

The file contains many `802.11` and some `EAPOL` protocol packets. The password is encrypted in `WPA` 4-way handshake packets.

```
Frame 23020: 133 bytes on wire (1064 bits), 133 bytes captured (1064 bits)
IEEE 802.11 QoS Data, Flags: ......F.
Logical-Link Control
802.1X Authentication
    Version: 802.1X-2001 (1)
    Type: Key (3)
    Length: 95
    Key Descriptor Type: EAPOL RSN Key (2)
    [Message number: 1]
    Key Information: 0x0089
    Key Length: 32
    Replay Counter: 1
    WPA Key Nonce: 5706027aa040608050dc586732df808c567a92ee786f02057a9f1ae91a9f93f2
    Key IV: 00000000000000000000000000000000
    WPA Key RSC: 0000000000000000
    WPA Key ID: 0000000000000000
    WPA Key MIC: 00000000000000000000000000000000
    WPA Key Data Length: 0
```

## Solution

Luckily, there are automated tools made by `hashcat` team.

1. Convert `.pcap` file to `.hc22000` with [an online tool](https://hashcat.net/cap2hashcat/).
2. Crack password with hashcat.

```
âââ(kaliã¿kali)-[~]
ââ$  hashcat -m 22000 155043_1743065392.hc22000 /usr/share/wordlists/rockyou.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 17.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu-sandybridge-AMD Ryzen 7 5800H with Radeon Graphics, 1435/2934 MB (512 MB allocatable), 4MCU

Minimum password length supported by kernel: 8
Maximum password length supported by kernel: 63

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Single-Hash
* Single-Salt
* Slow-Hash-SIMD-LOOP

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 0 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

32989a79f86d839eadd1aaa21c03d3f9:005f674f6a1a:1cbfce17b0be:Gone_Surfing:mickeymouse
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 22000 (WPA-PBKDF2-PMKID+EAPOL)
Hash.Target......: 155043_1743065392.hc22000
Time.Started.....: Thu Mar 27 04:54:26 2025 (1 sec)
Time.Estimated...: Thu Mar 27 04:54:27 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     1803 H/s (10.15ms) @ Accel:64 Loops:1024 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 1971/14344385 (0.01%)
Rejected.........: 1459/1971 (74.02%)
Restore.Point....: 1029/14344385 (0.01%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: hollister -> trinidad
Hardware.Mon.#1..: Util: 26%

Started: Thu Mar 27 04:53:58 2025
Stopped: Thu Mar 27 04:54:28 2025
```

So the password is `mickeymouse` and our flag is `picoCTF{mickeymouse}`.

## References

Explanation of WPA 4-way handshake:
https://networklessons.com/wireless/wpa-and-wpa2-4-way-handshake

Cracking WPA:
https://hashcat.net/wiki/doku.php?id=cracking_wpawpa2
