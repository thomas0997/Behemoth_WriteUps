# Assistant or Agent?

- **Platform:** CyLab Security Academy
- **Difficulty:** Easy
- **Date completed:** 2026-11-08
- **Category:** Artificial Intelligence

## Summary
An interactive-fiction style challenge teaching the practical difference between "assistant mode" (AI that only advises/drafts in chat) and "agent mode" (AI that can act with tools, memory, and permissions in real systems) through three in-story scenarios. Choosing the right mode — and the right guardrails — for each scenario's risk level reveals the flag at the end.

## Steps
1. Connected via `nc xebec.cylabacademy.net 15266` and read through the setup: Ren, an unofficial tech helper at a school, gets help from an AI assistant named ARIA to prepare a community cleanup event.
2. ARIA explained the two modes up front: assistant mode just answers/drafts in chat, agent mode can plan steps, use tools, and act in connected systems if granted permission.
3. **Scene 1 — The Schedule Switcheroo:** Needed a volunteer schedule fast, with edge cases (conflicting availability, allergies). Picked option **C** — use ARIA as an agent, but require a human approval checkpoint before any messages send. ARIA drafted the full schedule, flagged three edge cases, Ren reviewed/edited them, then approved — messages went out clean with no mistakes.
4. **Scene 2 — The Source Scavenger Hunt:** Needed three trustworthy facts (litter impact, disposal guidance, safety tips) for a poster. Picked option **B** — agent-mode ARIA browsing approved sites, collecting citations, and attaching a verification bundle. ARIA returned a mini evidence packet with confidence notes (high/medium/needs review) per claim, rather than just handing over unsourced text.
5. **Scene 3 — The Snack Budget Boss Fight:** A fixed $120 budget that couldn't be allowed to overspend. Picked option **A** — assistant-mode ARIA only (a shopping list suggestion), with Ren placing the actual order manually. Landed at $117, a clean win with zero autonomous spending risk.
6. At the ending, ARIA asked Ren (the player) to explain the core distinction back: "Assistant: gives me content in chat. Agent: can take actions with tools and memory in real systems." Confirmed correctly, followed by "And?" — answered that more autonomy means more leverage and more responsibility, so permissions/guardrails/human approval are used where impact is high.
7. Correct answers to both follow-up questions revealed the flag directly.

## Tools used
- `nc` — connected to and played through the entire interactive fiction challenge

## Lesson learned
The challenge's real lesson isn't technical exploitation but a decision framework: match the AI's autonomy level to the task's blast radius. Low-risk/reversible tasks (drafting a shopping list) are fine for assistant mode or ungated agent action; higher-impact or harder-to-reverse tasks (sending messages to real people, spending real money) warrant either a human approval checkpoint or keeping the AI in assistant-only mode so a human retains final action authority. "More autonomy = more leverage and more responsibility" is the one-line summary worth remembering for any AI-agent design decision, not just this challenge.

## Flag
```
academy{4551574n7_0r_463n7_64307e7a}
```