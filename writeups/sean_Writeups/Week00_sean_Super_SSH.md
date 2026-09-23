# Write-up: Secure Shell (SSH)

**Platform:** picoCTF
**Category:** General Skills
**Author:** Sean

---

## Steps Taken

1. Clicked **Launch Instance** to get the connection details (username, password, and port for this challenge). These are unique per session, so I can't reuse someone else's.
2. Connected using the SSH command format:
   ```bash
   ssh <user>@titan.picoctf.net -p <port>
   ```
   - `<user>` — the username shown in the instance details
   - `-p <port>` — specifies the port, since picoCTF doesn't use the default port 22
3. Entered the password when prompted. Note: the terminal won't show any characters or a cursor movement while typing the password — that's normal, not a bug.
4. Once logged in, looked around the filesystem (`ls`, `cat`) to find the flag.

## Tools Used

- `ssh` — connect to a remote machine securely over an encrypted connection

## Answer

- Flag: picoCTF{s3cur3_c0nn3ct10n_8306c99d}

## Lesson Learned

_Write your own lesson here, e.g. why SSH needs a port specified here, or what confused you about the hidden password input._