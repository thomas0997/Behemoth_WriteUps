# Wave a Flag

* **FILE NAME FORMAT:** Week00_dein_WaveAFlag
* **Platform:** CyLab
* **Difficulty:** Easy
* **Date completed:** 2026-10-01
* **Category:** Other / General Skills

## Summary

This challenge involved identifying and running a compiled binary to find its helpful information and flag. I initially found the flag by inspecting the file, but then learned the proper way to execute the binary through Kali Linux.

## Steps

1. I downloaded the `warm` file and initially opened it using **Notepad** and **VS Code**. The contents appeared as unreadable binary data, but I was still able to spot the flag within the output.

2. I did not immediately submit it because I recognized that the flag appeared in the wrong context. I decided to solve the challenge properly by running the binary.

3. I used the Kali Linux WSL terminal and ran:

```bash
cat warm
```

This produced garbled output because `warm` was a compiled binary, although the flag could still be seen within the output.

4. I confirmed the file type using:

```bash
file warm
```

The result identified it as an **ELF executable**.

5. I tried making the file executable using:

```bash
chmod +x warm
```

This failed because the file was located on the Windows drive under `/mnt/c/`, which does not support Linux file permissions in the same way as Kali's filesystem.

6. I copied the file into Kali's own filesystem:

```bash
mkdir -p ~/ctf/warm
cp warm ~/ctf/warm/
```

7. I then made the file executable and ran it:

```bash
chmod +x warm
./warm
```

8. The program displayed its helpful information, allowing me to complete the challenge correctly. The flag was:

`academy{b1scu1ts_4nd_gr4vy_57d8c79}`

## Tools used

* **Kali Linux WSL** - Used to inspect and execute the binary.
* **`file`** - Used to identify the file as an ELF executable.
* **`chmod`** - Used to give the binary execute permissions.
* **`cp`** - Used to move the file into Kali's filesystem.
* **`./warm`** - Used to execute the binary.
* **`cat`** - Used to inspect the binary's contents.

## Lesson learned

I learned that compiled binaries should be properly identified and executed rather than treated like normal text files. I also learned how Windows-mounted files in WSL can have permission limitations and how copying them into Kali's filesystem can resolve those issues.
