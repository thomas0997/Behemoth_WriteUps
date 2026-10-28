# Random Number

- **FILE NAME FORMAT:** `Week05_dein_RandomNumber`
- **Platform:** CyLab
- **Difficulty:** Easy
- **Date completed:** 2026-10-28
- **Category:** Reverse Engineering / Python

## Summary
The challenge involved analyzing a Python file to identify a vulnerability in its code. I discovered the `win` function, triggered it through the program's input, and used CyberChef to decode the resulting hexadecimal data to obtain the flag.

## Steps
1. **Downloaded the Python file.** I examined the source code to understand how the program worked.
2. **Analyzed the code.** I searched for a vulnerability and discovered the `win` function.
3. **Triggered the function.** When the program prompted me for input, I entered `win`, which caused it to return hexadecimal data.
4. **Decoded the hexadecimal data.** I used CyberChef to convert the provided hexadecimal data into readable text.
5. **Retrieved the flag.** After decoding the output, I obtained the flag:

   `academy{4_d14m0nd_1n_7h3_r0ugh_33984188}`

## Tools Used
- **Python source code:** Inspected the program's logic and identified the vulnerable function.
- **Python interpreter:** Ran the program and tested the input.
- **CyberChef:** Decoded the hexadecimal output into readable text.

## Lesson Learned
I learned that analyzing source code can reveal vulnerabilities in how a program handles user input. Triggering the `win` function and decoding its hexadecimal output with CyberChef helped me practice basic reverse engineering and data decoding.