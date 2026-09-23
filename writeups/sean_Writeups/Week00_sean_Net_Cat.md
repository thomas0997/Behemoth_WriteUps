# Write-up: nc (netcat) Basics

**Platform:** picoCTF
**Category:** General Skills
**Author:** Sean

---

## Steps Taken

1. Got the connection details from the challenge: host `fickle-tempest.picoctf.net`, port `58040`.
2. Connected using netcat:
   ```bash
   nc fickle-tempest.picoctf.net 58040
   ```
3. Once connected, the server sent back a message/output directly in the terminal, which contained the flag.

## Tools Used

- `nc` (netcat) — connect directly to a service running on a remote host and port

## Answer

- Flag: _fill in after you run the command_

## Lesson Learned

_Write your own lesson here, e.g. the difference between connecting to a hostname/IP vs using a placeholder, or what nc actually showed you once connected._