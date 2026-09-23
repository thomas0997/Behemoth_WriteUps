# Lets Warm Up

* **FILE NAME FORMAT:** Week00_Username_LetsWarmUp
* **Platform:** CyLab
* **Difficulty:** Easy
* **Date completed:** 2026-09-23
* **Category:** Other / General Skills

## Summary

This challenge introduced me to hexadecimal and ASCII, including how to convert between hexadecimal values and text. The goal was to determine which ASCII character corresponds to the hexadecimal value `0x70`.

## Steps

1. I first learned what **hexadecimal** and **ASCII** are. Hexadecimal is a base-16 number system that uses the digits `0–9` and letters `A–F`, where `A–F` represent the values `10–15`. Each hexadecimal digit represents 4 bits.

2. I also learned about **ASCII**, an encoding system that assigns numerical codes from `0–127` to letters, digits, and other characters. Each ASCII character can be represented by a byte, which is 8 bits or two hexadecimal digits.

3. To convert hexadecimal to ASCII, I can split the hexadecimal code into pairs of two characters, convert each pair into its decimal value, and then look up the corresponding character in an ASCII table. For example, `4849` can be split into `48` and `49`. These correspond to decimal values `72` and `73`, which represent `H` and `I`, giving `HI`.

4. I also learned that the process can be reversed to convert text into hexadecimal. For example, `HI` corresponds to decimal values `72` and `73`, which convert to the hexadecimal values `48` and `49`, giving `4849`.

5. For the challenge, I was asked: **"If I told you a word started with 0x70 in hexadecimal, what would it start with in ASCII?"**

6. I looked up `0x70` in an online ASCII table and converter found that it corresponds to the lowercase letter **`p`**.

7. Since the challenge expected the answer in the flag format, I entered **`picoCTF{p}`**, which was marked as correct and completed the challenge.

## Tools used

* **ASCII Table** - Used to find the ASCII character represented by `0x70`.
* **Hexadecimal** - Used to represent and convert numerical values.
* **CyLab CTF** - Used to complete and submit the challenge.

## Lesson learned

I learned the basics of hexadecimal and ASCII and how they are used to represent text as numerical values. I also learned how to use an ASCII table to convert hexadecimal values into characters, which is a useful skill for solving CTF challenges involving encoded data.
