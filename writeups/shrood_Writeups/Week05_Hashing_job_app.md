# HashingJobApp

- **Platform:** picoCTF (picoMini 2022)
- **Difficulty:** Easy / Beginner
- **Date completed:** 2026-10-31
- **Category:** General Skills

## Summary
A netcat service acts as a quiz: it gives a plaintext string and asks for its MD5 hash, back-to-back for several rounds. Answer each round correctly to get the flag at the end.

## Steps
1. Connected to the challenge with `nc chatelaine.cylabacademy.net 20789`.
2. Service presented the first prompt: hash the text between quotes (excluding the quotes) — `'a locker room'`.
3. Ran it through an MD5 hash tool, submitted the result (`9be86a0fa0b3b24d0d18d78dc45808f1`) — confirmed `Correct.`
4. Repeated for the next two rounds (`'money'`, `'killer whales'`), submitting each MD5 hash as given.
5. After the third correct answer, the service returned the flag directly.

## Tools used
- `nc` — connected to and interacted with the challenge service
- MD5 hashing tool (CLI or web-based) — computed each round's answer

## Lesson learned
Straightforward challenge once you notice the pattern: read the exact string between the quotes (no quotes, no extra whitespace), hash it with the exact algorithm named, and submit. These interactive "answer N rounds correctly" netcat services are common in picoCTF's General Skills category — the real skill being tested is just reading the prompt carefully and automating the hash step if there are more rounds than feels worth doing by hand.

## Flag
```
academy{4ppl1c4710n_r3c31v3d_8c3fe27c}
```