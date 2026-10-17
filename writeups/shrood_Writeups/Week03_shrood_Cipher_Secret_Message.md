# Cipher's Secret Message

- **Platform:** TryHackMe
- **Difficulty:** Easy
- **Date completed:** 2026-10-17
- **Category:** Crypto

## Summary
A message was encrypted with a custom cipher that shifts each alphabetic character forward by its own position index in the string (non-alpha characters pass through untouched). The challenge hands you the encryption function and the ciphertext, but not the plaintext — solving it means deriving and running the *inverse* of the given function rather than the function itself.

## The encryption algorithm
```python
from secret import FLAG

def enc(plaintext):
    return "".join(
        chr((ord(c) - (base := ord('A') if c.isupper() else ord('a')) + i) % 26 + base)
        if c.isalpha() else c
        for i, c in enumerate(plaintext)
    )

with open("message.txt", "w") as f:
    f.write(enc(FLAG))
```
For each character at index `i`: if it's alphabetic, shift it forward by `i` positions (wrapping within its own case, A–Z or a–z); everything else (`_`, digits) is left alone.

## Ciphertext given
```
a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm
```

## Failed attempts / mistakes made
1. **Tried to literally run the provided script as-is**, including `from secret import FLAG` — this fails immediately since `secret.py` doesn't exist locally:
   ```
   ModuleNotFoundError: No module named 'secret'
   ```
   Attempted `pip install secret`, which also isn't the right fix (hit an `externally-managed-environment` error on top of it, and even if installed, `secret` isn't a real package containing the challenge's flag anyway). The `from secret import FLAG` line is just scaffolding describing how the challenge author generated the ciphertext — it's not something you need to reproduce or import.
2. **Ran the forward `enc()` function on guessed plaintexts** instead of decoding the actual ciphertext — this only re-encrypts a guess, it doesn't reverse anything, and produced garbage output each time (e.g. `a_ws4vx_sjslr0_qkacmuu_nk_sv4xb_iwjz_PFM_gximwg`, `a_yv4ad_ascwd0_farufop_ki_sw4af_odri_ARZ_vnzepa`, etc. — none of these were meaningful).
3. **Wrote the inverse formula correctly** (subtracting `i` instead of adding it) but **introduced a leading space** in the `FLAG` string being decoded:
   ```python
   FLAG = " a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm"  # extra space at the start
   ```
   Since the cipher depends on each character's *exact* index, that one leading space shifted every subsequent character's index by 1, corrupting the entire decode (`z_rl4kk_bqxos0_ldrrzfd_sn_rs4qs_vhsg_SGL_bqzbjr` — still garbage).

## Working solution
1. Ignored the `from secret import FLAG` import — it's irrelevant to decoding; the actual ciphertext to decode is the message given in the challenge.
2. Derived the inverse of `enc()`: since encoding adds `i` to each character's value, decoding subtracts `i` instead:
   ```python
   def dec(ciphertext):
       return "".join(
           chr((ord(c) - (base := ord('A') if c.isupper() else ord('a')) - i) % 26 + base)
           if c.isalpha() else c
           for i, c in enumerate(ciphertext)
       )

   ciphertext = "a_up4qr_kaiaf0_bujktaz_qm_su4ux_cpbq_ETZ_rhrudm"
   print(dec(ciphertext))
   ```
3. Ran it on the ciphertext exactly as given (no extra whitespace), which produced readable plaintext.
4. Wrapped the decoded message in the flag format per the challenge note.

## Tools used
- Python — implemented and ran the inverse cipher function
- `nano` — edited the decode script directly on the Kali box

## Lesson learned
When a challenge hands you an encryption function and its output, the task is almost always to invert the transformation, not to re-run it forward on guesses. Index-dependent ciphers (where the shift depends on character position) are extremely sensitive to any accidental change in string length or content — a single stray leading/trailing space shifts every index downstream and silently corrupts the entire result, with no error thrown. Also, an import in provided challenge source (like `from secret import FLAG`) is often just scaffolding showing how the author generated the ciphertext, not something that needs to be installed or reproduced to solve the challenge.

## Flag
```
THM{a_sm4ll_crypt0_message_to_st4rt_with_THM_cracks}
```