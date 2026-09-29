# Offensive Security Intro

* **FILE NAME FORMAT:** Week01_dein_OffensiveSecurityIntro
* **Platform:** TryHackMe
* **Difficulty:** Easy
* **Date completed:** 2026-09-30
* **Category:** Introduction / Web / Other

## Summary

This room introduced me to offensive security and how ethical hackers simulate real attacker behavior to identify weaknesses. I also practiced using a virtual machine, finding hidden web pages, and exploiting a vulnerable page in a safe and legal environment.

## Steps

### Task 1: Introduction to Offensive Security

1. I learned that **Offensive Security** involves thinking like an attacker to find weaknesses before real hackers can exploit them.

2. The question asked which term describes simulating a hacker's actions to find weaknesses. The correct answer was **Offensive Security**, because ethical hackers need to simulate real attack scenarios to identify vulnerabilities in the systems they are defending.

### Task 2: Exploring FakeBank

1. I opened the virtual machine provided by TryHackMe, which simulated a real system with the FakeBank web application.

2. I accessed the FakeBank application and looked for the account information required by the task.

3. The bank account number shown in the application was **8881**.

### Task 3: Finding Hidden Pages

1. I opened the terminal within the virtual machine and used **Dirb** to scan the FakeBank website for hidden pages.

2. I ran the command:

```bash
dirb http://fakebank.thm
```

3. I checked the results for URLs marked with a `+`, which indicated pages that Dirb had discovered.

4. Dirb found two hidden URLs. One was `/images`, while the other was:

`http://fakebank.thm/bank-transfer`

5. I accessed the hidden `/bank-transfer` page, which revealed a vulnerable page that allowed money to be added to my account.

### Task 4: Exploiting the Vulnerability

1. I added `/bank-transfer` to the FakeBank URL to access the hidden bank transfer page.

2. I entered my account number, **8881**, and deposited **$2000** into the account.

3. After returning to the account page, the balance became positive, confirming that the simulated attack was successful.

4. A pop-up appeared with the green text **BANK-HACKED**, which was the answer required to complete the task and finish the room.

## Tools used

* **TryHackMe Virtual Machine** - Used to interact with the simulated FakeBank environment.
* **Dirb** - Used to scan the website for hidden directories and pages.
* **Terminal** - Used to run the Dirb command and perform the web directory scan.
* **Web Browser** - Used to access FakeBank and the discovered hidden page.

## Lesson learned

I learned how offensive security involves thinking like an attacker to identify vulnerabilities. I also learned how directory scanning can reveal hidden pages and how an exposed vulnerable page can be exploited in a controlled environment.
