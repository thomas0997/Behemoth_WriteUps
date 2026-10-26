# What's a Net Cat?

- **FILE NAME FORMAT:** Week05_dein_WhatsANetCat
- **Platform:** CyLab
- **Difficulty:** Easy
- **Date completed:** 2026-10-26
- **Category:** Networking / General Skills

## Summary

Completed the *What's a Net Cat?* challenge on CyLab, where I learned how to use Netcat (`nc`) to connect directly to a remote server through a specified port. By establishing a connection, I received the server's response and retrieved the flag.

## Steps

1. **Check Netcat Availability**
   - Opened my Kali Linux terminal and ran `nc -h` to check whether Netcat was installed and view its available options.
   - Learned that Netcat is a command-line networking tool that can establish connections to remote hosts through specific ports.

2. **Connect to the Remote Server**
   - Used the provided hostname and port to connect to the challenge server:
   ```bash
   nc xebec.cylabacademy.net 27759
   ```
   - The command established a connection to the remote service, allowing me to communicate with it directly through the terminal.

3. **Retrieve the Flag**
   - After connecting, the server returned the following message:
   ```
   You're on your way to becoming the net cat master
   academy{nEtCat_Mast3ry_AFf1FE8E}
   ```
   - Successfully retrieved the flag:
   `academy{nEtCat_Mast3ry_AFf1FE8E}`

## Tools Used

- **Kali Linux Terminal:** Used to execute commands and interact with the remote challenge server.
- **Netcat (`nc`):** Established a direct network connection to the provided hostname and port.

## Lesson Learned

I learned that Netcat is a useful networking tool for connecting to remote services and receiving their responses directly through the terminal. Unlike a web browser, which is designed to communicate using HTTP, Netcat can establish basic TCP connections without automatically requiring a specific application protocol. This challenge helped me understand how hostname and port combinations are used to access network services and how simple command-line tools can be useful when solving CTF challenges.