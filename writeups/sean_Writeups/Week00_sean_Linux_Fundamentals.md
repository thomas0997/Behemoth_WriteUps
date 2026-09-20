# Write-up: Linux Fundamentals Part 1

**Platform:** TryHackMe
**Room:** Linux Fundamentals Part 1
**Author:** Sean

---

## Steps Taken

I worked through the room's tasks in the AttackBox and practiced each command in the terminal.

| Command | What it does | Example |
|---|---|---|
| `whoami` | Shows which user you are logged in as | `whoami` |
| `ls` | Lists the files and folders in the current directory | `ls` |
| `cd` | Moves you into a folder | `cd Documents` |
| `cat` | Shows the contents of a file | `cat notes.txt` |
| `echo` | Prints the text you give it | `echo "hello"` |
| `pwd` | Shows where you are (the current directory path) | `pwd` |
| `&` | Runs the first command in the background, so both run at the same time | `cmd1 & cmd2` |
| `&&` | Runs the second command only if the first one succeeds | `cmd1 && cmd2` |
| `>` | Sends output into a file. Creates the file if it doesn't exist and **overwrites** it if it does | `echo "hi" > file.txt` |
| `>>` | Sends output to the **end** of a file. Creates the file if it doesn't exist | `echo "hi" >> file.txt` |

---

## Tools Used

- TryHackMe AttackBox (browser terminal)
- Linux terminal commands: `whoami`, `ls`, `cd`, `cat`, `echo`, `pwd`, `&`, `&&`, `>`, `>>`

---

## Lesson Learned

_Write your own lesson here in 1-2 sentences, for example the mistake you made or the thing that finally clicked for you._