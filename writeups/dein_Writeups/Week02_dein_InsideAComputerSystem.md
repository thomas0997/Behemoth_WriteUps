# Inside a Computer System

* **FILE NAME FORMAT:** Week02_dein_InsideAComputerSystem
* **Platform:** TryHackMe
* **Difficulty:** Easy
* **Date completed:** 2026-10-09
* **Category:** Computer Fundamentals

## Summary

Completed the *Named Inside a Computer System* room on TryHackMe, learning about the main components of a computer, their functions and positions, and the sequence of steps involved in booting up a computer. The room introduced essential computer fundamentals that will help me understand the systems I will eventually learn to secure in cybersecurity.

## Steps

1. **Task 1: Identify Computer Components**

   * Answered questions about different PC components and their respective functions.
   * Correctly answered all questions on my first try and received the flag.
   * Learned that understanding a computer's components and their roles is essential before attempting to secure the system.

2. **Task 2: Explore PC Components**

   * Used the interactive static site to move and place the PC components in their correct positions inside the computer.
   * Completed the activity after two tries and received the flag.
   * Learned to recognize the locations of components such as the RAM, motherboard, and SSD/HDD, along with external components such as the network adapter and power supply unit (PSU).

3. **Task 3: Understand the Boot Sequence**

   * Rearranged the computer startup processes into their correct order using the interactive static site.
   * Completed the activity after a few tries and received the flag.
   * Learned the sequence a computer follows when starting up:

     1. **Press the Power Button:** Signals the PSU to supply power to the system.
     2. **Firmware Starts:** The UEFI initializes and manages the startup process. BIOS is its older predecessor.
     3. **Power-On Self-Test (POST):** Checks whether essential components are present and functioning correctly.
     4. **Select Boot Device:** The firmware checks the configured boot-device order to locate the operating system.
     5. **Initiate Bootloader:** The bootloader loads the operating system into memory, after which the operating system takes control of the system's components.
   * Understood how these steps work together to bring the computer to a usable state.

4. **Task 4: Room Conclusion**

   * Completed the concluding task and received the free flag.
   * Reinforced the importance of understanding computer components and the boot process before studying more advanced cybersecurity topics.

## Tools Used

* **TryHackMe:** Used the learning platform to study computer fundamentals and complete the room's tasks.
* **Interactive Static Sites:** Used the drag-and-drop activities to identify PC components, arrange their positions, and organize the boot sequence.

## Lesson Learned

I learned that a computer relies on multiple components working together, from the hardware that provides processing, memory, and storage to the firmware and bootloader that help start the operating system. The interactive activities helped me understand these concepts more clearly through practice. Most importantly, I realized that knowing how a computer operates is a necessary foundation for cybersecurity because protecting a system requires understanding its components and how they interact. The boot process is also worth remembering because it can become a target in certain types of cyberattacks.
