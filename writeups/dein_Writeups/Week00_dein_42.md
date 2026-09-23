# 42

* **FILE NAME FORMAT:** Week00_dein_42
* **Platform:** CyLab
* **Difficulty:** Easy
* **Date completed:** 2026-09-23
* **Category:** Other / General Skills

## Summary

This challenge taught me the process of converting values between different number bases. The goal was to convert the decimal number `42` (Base 10) into binary (Base 2).

## Steps

1. I learned that to convert a standard Base 10 number into another base, I repeatedly divide the number by the target base. For binary, the target base is `2`.

2. To convert `42` to binary, I repeatedly divided it by `2` and recorded the remainder:

   * `42 ÷ 2 = 21` remainder `0`
   * `21 ÷ 2 = 10` remainder `1`
   * `10 ÷ 2 = 5` remainder `0`
   * `5 ÷ 2 = 2` remainder `1`
   * `2 ÷ 2 = 1` remainder `0`
   * `1 ÷ 2 = 0` remainder `1`

3. I then read the remainders from **last to first**, giving `101010`.

4. I also learned how to convert binary back into Base 10 by using the place values of powers of 2. For `101010`, the place values are:

5. The `1`s represent the values `32`, `8`, and `2`. Adding them together gives:
   `32 + 8 + 2 = 42`

6. Therefore, the final conversion is:

   * **Decimal (Base 10):** `42`
   * **Binary (Base 2):** `101010`

## Tools used

* **CyLab CTF** - Used to complete and submit the challenge.
* **Base Conversion** - Used to convert between decimal and binary.

## Lesson learned

I learned the logic behind converting Base 10 values to binary by repeatedly dividing by `2` and reading the remainders backward. I also learned how binary place values can be used to convert the result back to decimal and verify that the conversion is correct.
