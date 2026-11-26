# Packets Primer

**Platform:** CyLab Security Academy (picoCTF 2022)
**Category:** Forensics
**Difficulty:** Medium
**Author:** LT 'syreal' Jones
**Date completed:** 2026-11-26

## Challenge
Download a packet capture and use packet analysis software to find the flag.

## Steps
1. Downloaded `network-dump.flag.pcap` and opened it in Wireshark.
2. Scrolled through packets one by one looking for anything unusual rather than searching blindly.
3. Spotted a TCP packet whose hex/ASCII payload spelled out letters spaced one per byte: `a c a d e m y { p 4 c k 3 7 _ 5 h 4 r k _ 7 f f 5 a 1 b 3 }`.
4. Right-clicked that packet → **Follow → TCP Stream** on the third TCP stream — the full flag text appeared immediately in the reassembled stream view, still space-separated.
5. Stripped the spaces to get the final flag.

## Tools used
- Wireshark (packet list view, Follow TCP Stream)

## Lesson learned
- Payloads are sometimes deliberately obfuscated with trivial spacing/formatting (one character per byte) rather than real encryption — "Follow TCP Stream" reassembles the full conversation, making a flag spread across a packet's raw bytes trivial to read once you're looking at the stream instead of a single packet's hex dump.
- Scanning packet-by-packet for anything visually odd in the hex/ASCII pane is a legitimate (if slow) way to spot a flag when you don't know which stream to check — once one anomalous packet is found, following its stream usually surfaces the whole payload at once.

## Flag
`academy{p4ck37_5h4rk_7ff5a1b3}`