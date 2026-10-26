# Tab, Tab, Attack

- **FILE NAME FORMAT:** Week05_dein_TabTabAttack
- **Platform:** CyLab
- **Difficulty:** Easy
- **Date completed:** 2026-10-26
- **Category:** Forensics / General Skills

## Summary

Completed the *Tab, Tab, Attack* challenge from picoCTF 2021 through CyLab. I downloaded and extracted a ZIP archive in WSL, navigated through its nested directories, and inspected a compiled executable to uncover the hidden flag. This challenge helped me understand how tab completion simplifies navigation and how readable strings can be extracted from binary files.

## Steps

1. **Download and Extract the ZIP File**
   - Downloaded `Addadshashanammu.zip` from the challenge and extracted it in WSL.
   - Learned that ZIP archives can contain deeply nested directory structures, making navigation more difficult when done manually.

2. **Navigate Through the Directories**
   - Used `cd ..` repeatedly to move between directories until I reached the directory containing the challenge file.
   - Learned that terminal navigation can become time-consuming when files are stored several folders deep.
   - The challenge also introduced the usefulness of **tab completion**, which allows the terminal to complete directory and file names automatically instead of requiring me to type long paths.

3. **Inspect the Challenge File**
   - Located the file named `fang-of-haynekhtnamet`.
   - Used the `cat` command to inspect its contents:
   ```bash
   cat fang-of-haynekhtnamet
   ```
   - The output appeared as mostly unreadable characters because the file was a compiled binary rather than a normal text file.
   - However, I noticed that readable text was embedded within the output, including a string that resembled the flag with its missing beginning.

4. **Make the File Executable**
   - Used `chmod +x` to grant the executable permission:
   ```bash
   chmod +x fang-of-haynekhtnamet
   ```
   - Learned that Linux may require executable permission before a program can be run directly.

5. **Execute the Program and Retrieve the Flag**
   - Ran the executable from its current directory:
   ```bash
   ./fang-of-haynekhtnamet
   ```
   - The program displayed the response `*ZAP!*` along with the complete flag.
   - Successfully retrieved:
   ```
   academy{l3v3l_up!_t4k3_4_r35t!_9d928112}
   ```

## Tools Used

- **WSL / Kali Linux Terminal:** Used to extract the archive, navigate directories, and execute commands.
- **`cd ..`:** Moved up one directory level at a time.
- **`cat`:** Displayed the file contents, revealing readable text embedded in the binary, although this was not the ideal tool for inspecting a compiled executable.
- **`chmod +x`:** Granted executable permission to the program.
- **`./fang-of-haynekhtnamet`:** Executed the compiled program to retrieve the flag.
- **Tab Completion:** Helped illustrate how pressing Tab can automatically complete directory and file names.

## Lesson Learned

I learned that navigating through nested directories can be much easier with tab completion instead of repeatedly typing long paths. I also discovered that compiled programs can contain readable strings even when most of their contents appear unreadable in the terminal. Although `cat` helped me notice part of the flag, it is not the recommended way to inspect binary files. Commands such as `file` and `strings` are more suitable for identifying file types and extracting readable text. Finally, this challenge helped me understand how executable permissions work in Linux and how running a compiled program can reveal information that is not immediately obvious from its contents.