# Write-up: Caesar Cipher & Symmetric vs Asymmetric Review

**Platform:** TryHackme
**Category:** Cryptography Concepts
**Date:** October 14, 2026
**Author:** Sean

---

## Steps Taken

**1. Core vocabulary review**
Went through the basic cryptography terms before starting the cipher exercises:
- Plaintext — a readable message (e.g. `HELLO`)
- Ciphertext — the scrambled version (e.g. `KHOOR`)
- Key — the secret value that controls the scrambling/unscrambling
- Algorithm — the public set of steps (the Caesar cipher itself is public knowledge; the key is what stays secret)

**2. Secret Message Rescue game**
Completed all levels of the in-browser Caesar cipher game to get the flag.

**3. Caesar cipher — encode with a known key**
Encoded `CYBER` using a shift key of 5. Shifted each letter forward 5 positions in the alphabet (C→H, Y→D wrapping around, B→G, E→J, R→W).

**4. Caesar cipher — decode with unknown key (brute-force by hand)**
Given `FVZCYR PNRFNE PVCURE`, tried shifting back by different key values using the cipher tool until the output read as plain English.

**5. Decrypt an intercepted message (known key)**
Given ciphertext `DWWDFN WRPRUURZ` and shift key 3, used the cipher tool to shift each letter back by 3.

**6. Encrypt a password (known key)**
Given plaintext `SECRET` and shift key 5, used the cipher tool to shift each letter forward by 5.

**7. Decrypt with unknown key (brute-force)**
Given `ESP DJDEPX TD LE CTDV`, tried all 26 possible shifts in the cipher tool until one produced a readable English sentence.

**8. Final decrypt with unknown key (brute-force)**
Given `XLMW MW XLI JMREP GSHI`, again tried all 26 shifts since the key wasn't provided.

**9. Asymmetric encryption review**
Answered conceptual questions on how asymmetric encryption works (public/private key roles) and how HTTPS combines both encryption types.

## Tools Used

- TryHackMe/CyLab in-browser Caesar cipher tool — encode/decode with a given or brute-forced shift key
- Manual alphabet-shifting logic to understand what the tool was doing under the hood

## Answers

| # | Question | Answer |
|---|---|---|
| 1 | Flag after completing Secret Message Rescue game | `THM{CAESAR_CIPHER_MASTER_2026}` |
| 2 | CYBER encoded with key 5 | `HDGJW` |
| 3 | Decode `FVZCYR PNRFNE PVCURE`, find the key | `SIMPLE CAESAR CIPHER` |
| 4 | Decrypt `DWWDFN WRPRUURZ` (shift 3) | `ATTACK TOMORROW` |
| 5 | Encrypt `SECRET` (shift 5) | `XJHWJY` |
| 6 | Decrypt `ESP DJDEPX TD LE CTDV` (unknown key) | `THE SYSTEM IS AT RISK` |
| 7 | Decrypt `XLMW MW XLI JMREP GSHI` (unknown key) | `THIS IS THE FINAL CODE` |
| 8 | In asymmetric encryption, which key stays secret? | Private key |
| 9 | Alice encrypts with Bob's public key, only Bob's private key decrypts — Yay or Nay? | Yay |
| 10 | What problem does asymmetric solve that symmetric cannot? | Key distribution |
| 11 | After the initial asymmetric exchange in HTTPS, what encryption type handles bulk data? | Symmetric |

## Lesson Learned

_Write your own lesson here, e.g. what made brute-forcing the unknown-key messages manageable (only 26 possible shifts), or how this connects to the earlier symmetric/asymmetric write-up — Caesar cipher is a simple symmetric cipher, while HTTPS uses asymmetric encryption just to exchange a symmetric key, then switches to symmetric for speed._
