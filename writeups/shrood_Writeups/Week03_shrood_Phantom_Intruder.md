# Ph4nt0m 1ntrud3r

- **Platform:** picoCTF 2025
- **Difficulty:** Easy
- **Date completed:** 2026-10-15
- **Category:** Forensics

## Summary
A PCAP file full of TCP SYN packets, each carrying a small hidden payload instead of the normal empty SYN handshake data. The packets appear out of order by frame number, but sorting by actual timestamp reveals a sequence of Base64-encoded fragments — with a deliberate split between decoy noise and the real flag data at the `t=0` timestamp.

## Steps
1. Opened the PCAP in Wireshark and noticed every visible packet was a `[TCP Retransmission]` SYN packet with a small non-zero payload (`Len=8` or `Len=12`) — unusual, since SYN packets normally carry no data.
2. Per the challenge hints ("filter your packets," "attacks were done in a timely manner," "time is essential"), recognized the frame numbers were scrambled but the actual send order was preserved in each packet's timestamp.
3. Used `tshark` to extract every SYN packet's relative timestamp and payload, sorted correctly by time:
   ```
   tshark -r myNetworkTraffic.pcap -Y "tcp.flags.syn==1" -T fields -e frame.time_relative -e tcp.payload -E separator=, | sort -t, -k1 -n
   ```
4. Converted each hex payload to ASCII with a short Python script, and noticed every chunk looked like it ended in Base64 padding (`=` or `==`).
5. Decoded each chunk individually as Base64 (since each fragment was itself a complete, separately-padded Base64 string) and concatenated the results in timestamp order.
6. The first 14 fragments (all with negative relative timestamps, before `0.000000000`) decoded into meaningless binary noise — decoy data.
7. The last 8 fragments (starting exactly at `0.000000000` and onward) decoded cleanly into readable text, spelling out the flag.

## Tools used
- Wireshark — initial inspection of the suspicious SYN packet payloads
- `tshark` — extracted timestamps and raw TCP payload bytes in bulk, sorted by time
- Python (`bytes.fromhex`, `base64.b64decode`) — converted hex to ASCII and decoded the Base64 chunks

## Lesson learned
Data can be smuggled inside packet fields that protocols never normally use for payload (like a SYN packet's data section), and when traffic looks deliberately shuffled, the frame/packet number isn't trustworthy — the real sequence lives in the timestamp. Noise fragments mixed in with real data is also a common trick: don't assume every piece of extracted data belongs in the final answer; test for which pieces decode to something meaningful.

## Flag
```
academy{1t_w4snt_th4t_34sy_tbh_4r_a8139a41}
```