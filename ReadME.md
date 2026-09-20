# Neeks CTF Challenge

A 12-week Capture-the-Flag learning challenge by four HAU cybersecurity students, running alongside our normal school load. Two rooms a week, a write-up for every room, and a CyLab final on **December 16, 2026**.

**Challenge period:** September 21 – December 16, 2026

## Players

| Name | GitHub |
|---|---|
| Thomas Franco | @shrood |
| Hansdrew Satumbaga | @tomo |
| Daniel Rodriguez | @dein |
| Sean Deniel Dayrit | @sean |

## Repository structure

```
.
├── README.md
├── templates/
│   └── WRITEUP_TEMPLATE.md
└── writeups/
    ├── thomas-franco/
    │   ├── week-01/
    │   │   └── tryhackme-room-name.md
    │   └── week-02/
    ├── hansdrew-satumbaga/
    ├── daniel-rodriguez/
    ├── sean-deniel-dayrit/
    └── final-comp/
```

- One folder per player under `writeups/`, and one subfolder per week (`week-01` to `week-12`).
- One file per room, named `platform-room-name.md` in lowercase with hyphens (for example `tryhackme-linux-fundamentals.md`).
- Only edit your own folder.

## How to submit a write-up

1. Create a branch: `git checkout -b yourname/week-01`
2. Copy `templates/WRITEUP_TEMPLATE.md` into your week folder and rename it.
3. Fill in every section (below), then commit and push.
4. Open a pull request into `main` before the deadline.
5. The Administrator reviews and merges it. The merge time is the timestamp that counts.

## Write-up template

```markdown
# Room Name

- **Platform:** TryHackMe / picoCTF / OverTheWire / HackTheBox / CyLab
- **Difficulty:** Easy / Medium / Hard
- **Date completed:** YYYY-MM-DD
- **Category:** Web / Crypto / Forensics / OSINT / Reverse / Pwn / Linux / Other

## Summary
One or two sentences on what the room was about.

## Steps
1. What you did first, and why.
2. Next step, with the commands or payloads you used.
3. ...

## Tools used
- Tool, and what you used it for

## Lesson learned
One thing you learned or would do differently.
```

**Every write-up must include the steps, the tools, and one lesson learned.** A low-effort write-up earns a yellow card.

## The rules at a glance

- **Quota:** exactly 2 new rooms per week, no repeats.
- **Deadline:** every **Friday, 11:59 PM PHT**.
- **Exam weeks:** Week 4 (Oct 11–15) and Week 12 (Dec 7–12). No quota and no penalty.
- **Platforms:** TryHackMe, picoCTF, HackTheBox starter, OverTheWire, CyLab.
- **Integrity:** no copying write-ups or flags. Hints are fine, spoilers are not.
- **Scope:** only attack CTF platforms and your own lab. Never touch school or real-world systems without written permission.

## Scoring

| Event | Points |
|---|---|
| Write-up uploaded (per room) | +5 |
| Missing write-up (per room) | −10 |
| Bonus room (beyond 2 in a week, or any room in an exam week) | +5 |
| Every 2 yellow cards | −10 (count resets) |
| Red card | −10 |
| Final comp placement bonus | 1st +50 · 2nd +40 · 3rd +30 · last +20 |

Scores are kept in the tracker workbook. The full terms are in the signed agreement.

## Weekly schedule

| Week | Starts | Deadline (Fri) | Note |
|---|---|---|---|
| 1 | Sep 21 | Sep 25 | |
| 2 | Sep 28 | Oct 2 | |
| 3 | Oct 5 | Oct 9 | |
| 4 | Oct 12 | Oct 16 | Exam week |
| 5 | Oct 19 | Oct 23 | |
| 6 | Oct 26 | Oct 30 | |
| 7 | Nov 2 | Nov 6 | |
| 8 | Nov 9 | Nov 13 | |
| 9 | Nov 16 | Nov 20 | |
| 10 | Nov 23 | Nov 27 | |
| 11 | Nov 30 | Dec 4 | |
| 12 | Dec 7 | Dec 11 | Exam week |
| Final | Dec 16 | | CyLab final comp |

## Final competition (Dec 16)

- 20 CyLab rooms. Each player submits 2 Easy, 2 Medium, and 1 Hard room that nobody has solved yet.
- Everyone solves all 20 within a time limit (to be agreed).
- Placement is decided by Final Competition Points.
- Write-ups for the final go in `writeups/final-comp/`.

## A note on spoilers

Some platforms, like TryHackMe, ask people not to publish full solutions for their rooms and challenges. Keep flags, passwords, and answers out of your write-ups. Describe the method and what you learned instead.

## License

Personal learning project. Please don't copy write-ups for your own submissions.