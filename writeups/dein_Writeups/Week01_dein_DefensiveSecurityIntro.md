# Defensive Security Intro

* **FILE NAME FORMAT:** Week01_dein_DefensiveSecurityIntro
* **Platform:** TryHackMe
* **Difficulty:** Easy
* **Date completed:** 2026-09-30
* **Category:** Defensive Security / Blue Team

## Summary

This room introduced me to defensive security and the role of defenders in detecting, investigating, and responding to attacks. I practiced identifying suspicious activity, investigating an attack, and containing it by blocking the attacker's IP address.

## Steps

### Task 1: Think Like a Defender

1. I learned that **defensive security** is the process of defending and securing devices and systems.

2. Unlike offensive security, defensive security does not involve attacking systems. Instead, defenders monitor systems, detect suspicious activity, investigate attacks, and respond before damage occurs.

3. The question asked for the main goal of defensive security. The correct answer was **Detect and respond to attacks**, because defenders need to monitor, respond to, and protect systems when they are attacked.

### Task 2: Detect Suspicious Activity

1. I opened the monitoring dashboard in the simulated lab machine and reviewed the recent security events.

2. I identified an alert for a **Web Discovery Attack**, which involved automated directory enumeration on admin endpoints.

3. The suspicious source IP address shown in the event management page was **32.122.195.63**.

### Task 3: Identify the Attack

1. I investigated the attack by opening the **URL Discovery Attempts** section of the monitoring dashboard.

2. I reviewed the latest entry to determine what the attacker was trying to access.

3. The attacker had made multiple attempts to discover URLs, with **31 URL login attempts** and **10 block requests** over approximately 16 minutes.

4. The latest suspicious URL identified was:

`https://fakebank.com/admin`

5. This showed that the attacker was attempting to discover and access the site's admin page without proper authorization.

### Task 4: Stop the Attack

1. After identifying the attacker and the activity, I accessed the firewall manager to contain the attack.

2. I entered the attacker's IP address:

`32.122.195.63`

3. I selected **BLOCK** from the firewall rule dropdown and applied the rule.

4. Blocking the IP address prevented the attacker's device from accessing the system, completing the final task and successfully stopping the simulated attack.

## Tools used

* **TryHackMe Virtual Machine** - Used to access the simulated defensive security environment.
* **Monitoring Dashboard** - Used to review alerts and investigate suspicious activity.
* **Event Management** - Used to identify the suspicious source IP and attack type.
* **URL Discovery Attempts** - Used to investigate the URLs the attacker was attempting to access.
* **Firewall Manager** - Used to block the attacker's IP address.

## Lesson learned

I learned that defensive security involves more than simply identifying an attack. Defenders need to monitor activity, investigate suspicious behavior, identify the attacker and their target, and quickly contain the threat to protect the system.
