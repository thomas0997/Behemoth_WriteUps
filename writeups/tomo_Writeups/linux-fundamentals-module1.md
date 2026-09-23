# Linux Fundamentals — Module 1

A summary of key takeaways and practical exercises completed while working through Module 1 of Linux Fundamentals.

## Overview

This module focused on core filesystem navigation, file identification, and text searching — foundational skills for any Linux-based workflow, including log analysis and incident response.

---

## 1. Recovering from Navigation Errors

**Objective:** Locate and read the contents of a target file after initial navigation attempts failed.

**What happened:**
Persisted through multiple failed attempts and recovered by listing directory contents, then reading the correct file. This demonstrated the ability to recover from errors and extract file content once the correct path was identified.

**Flow:**
- Listed directory contents to identify structure
- Made several navigation attempts
- Read the file using `cat`

**Takeaway:** When navigation fails, stop guessing — list the directory first to confirm what's actually there before trying again.

---

## 2. Searching Logs with `grep`

**Objective:** Search `access.log` for a specific pattern (`THM`).

**What happened:**
Correctly applied `grep` to search for the `THM` pattern within the `access.log` file, demonstrating basic text search competency.

**Flow:**
- Navigated to the home directory
- Executed `grep` with the target pattern against the log file

**Takeaway:** Text searching with `grep` is a foundational skill for log analysis and incident response workflows — being able to quickly isolate relevant lines in large log files is a core troubleshooting technique.

---

## 3. File vs. Directory Confusion

**Objective:** Understand the distinction between files and directories when navigating the filesystem.

**What happened:**
Attempted to `cat` directories and `cd` into files — both invalid operations. This resulted in wasted time on repeated failed commands.

**Flow:**
- Used `ls` to identify item types before acting
- Navigated into directories only
- Read files using `cat`

**Takeaway:** Always list contents first (`ls` or `ls -l`) to verify whether an item is a file or a directory before attempting to act on it. Understanding file vs. directory types prevents redundant, failed attempts and builds more efficient navigation habits.

---

## Key Skills Reinforced

- Filesystem navigation (`ls`, `cd`)
- File content extraction (`cat`)
- Pattern searching (`grep`)
- Error recovery and troubleshooting methodology
- Verifying assumptions (path, file type) before acting
