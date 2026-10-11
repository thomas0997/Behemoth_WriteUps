# Write-up: Cryptography Basics (PCI DSS / Hashes / Base64 / MD5)

**Platform:** picoCTF
**Category:** Cryptography
**Author:** Sean

---

## Steps Taken

**1. Encode "cryptographyisuseful" with Base64**
- Opened CyberChef.
- Added the **To Base64** operation.
- Entered the plaintext string `cryptographyisuseful` as input.
- Read the output as the encoded answer.

**2. Decode "dGhlIHNlY3JldCB3b3JkIGlzIDogd2F0ZXJtZWxvbg=="**
- Added the **From Base64** operation in CyberChef.
- Pasted the string as input.
- Read the decoded output to find the secret word.

**3. Find the MD5 hash of "hashes are cool"**
- Added the **MD5** operation in CyberChef.
- Entered the plaintext string `hashes are cool` as input.
- Read the resulting hash from the output.

**4. What does MD5 stand for / who created it**
- Looked this up to answer the conceptual questions (not something you compute, just background knowledge on the algorithm).

## Tools Used

- CyberChef — Base64 encode/decode, MD5 hashing

## Answers

- Base64 encode of "cryptographyisuseful": _fill in from CyberChef output_
- Decoded secret word: _fill in from CyberChef output_
- MD5 hash of "hashes are cool": _fill in from CyberChef output_
- MD5 stands for: Message Digest Algorithm 5
- MD5 was created by: Ronald Rivest (1991)

## Lesson Learned

This task clarified the core vocabulary of cryptography:

| Term | Meaning |
|---|---|
| Plaintext | The original readable data before encryption — a document, image, or any file |
| Ciphertext | The scrambled result after encryption — ideally reveals nothing except its rough size |
| Cipher | The algorithm that converts plaintext to ciphertext and back |
| Key | The secret string of bits the cipher uses. The cipher itself is usually public, but the key must stay secret |
| Encryption | Plaintext → ciphertext, using a cipher and a key |
| Decryption | Ciphertext → plaintext, using a cipher and a key |

The cipher is like a public recipe, and the key is the secret ingredient — anyone can know the recipe, but without the key they can't reproduce the result.

I also learned the difference between encoding and hashing: Base64 is just a reversible encoding (not secret, not secure), while MD5 is a one-way hash used for things like password storage and file integrity checks — you can't decrypt a hash back into the original text.
