# Write-up: c_wily_courier

**Platform:** picoCTF
**Category:** General Skills
**Author:** Sean

---

## Steps Taken

1. Copied the file link from the challenge's details section.
2. Downloaded the file into my working directory with `wget`:
   ```bash
   wget https://challenge-files.picoctf.net/c_wily_courier/eaf5dbd32acd6d3318c402247156552fc789474a5328dc436404e97932ab34c3/flag
   ```
3. Read the file's contents with `cat`:
   ```bash
   cat flag
   ```
4. The flag was in plain text in the file, no decoding needed.

## Tools Used

- `wget` — download the file from the challenge server
- `cat` — print the file's contents directly to the terminal

## Answer

- Flag: `picoCTF{s4n1ty_v3r1f13d_9b8fa0bc}`

## Lesson Learned

_Write your own lesson here, e.g. what "in the clear" means in CTF terms, or why `wget` + `cat` is a common first move for file-based challenges._