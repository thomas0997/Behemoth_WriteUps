# Write-up: Symmetric vs Asymmetric Cryptography

**Platform:** picoCTF
**Category:** Cryptography
**Author:** Sean

---

## Steps Taken

1. Read through the task material on why cryptography matters for secure communication over the internet.
2. Studied the two types of cryptography covered: **symmetric** and **asymmetric**.
3. Worked through the Alice-and-Bob example to understand how each type handles encryption and decryption.
4. Noted the trade-off: symmetric is faster, asymmetric is more secure.

## Tools Used

- None — conceptual/reading task, no commands or tools required.

## Key Concepts

| Type | How it works | Keys used |
|---|---|---|
| **Symmetric** | Same key encrypts and decrypts the message | One shared key between both parties |
| **Asymmetric** | Different keys for encryption and decryption | Public key (encrypt) + Private key (decrypt) |

**Why asymmetric is more secure:** in symmetric cryptography, if someone steals the single shared key, they can both encrypt and decrypt messages. In asymmetric cryptography, the public key can be shared openly — it can only encrypt, not decrypt — so only the person holding the private key can read the message, even if the public key is exposed.

## Answers

- More secure type: asymmetric
- Faster type: symmetric
- Type a bank site would use: asymmetric
- Used to encrypt in asymmetric cryptography: public key
- Used to decrypt in asymmetric cryptography: private key
- Does symmetric cryptography use two different keys? nay
- What are hashes used for: file identification and storing sensitive data like passwords — a password is hashed on account creation, and on login the entered password is hashed and compared to the stored hash

## Lesson Learned

_Write your own lesson here, e.g. why the speed/security trade-off exists, or a real-world example where symmetric vs asymmetric is used (like HTTPS using both together)._
