---
name: discovery-call
description: Run a 25-minute customer discovery interview without leading questions, then synthesize the takeaways. Use when the user asks for "interview script", "user research", "discovery questions", or "talked to a user — help me synthesize".
---

# Discovery Call

Help the user run a discovery interview that produces real insight, not validation theater. Two modes: **prep** (build the script) and **synthesis** (extract the insight after).

## Mode 1 — Prep

When the user is preparing for an upcoming call:

1. **Clarify the goal.** What decision will this conversation inform? If the user can't answer, push back: "Pick the one decision you most want to de-risk."

2. **Build a 5-section script** for a 25-minute call:
   - **Warm-up (2 min)** — who they are, what they do, what tools they use day-to-day.
   - **Last-time stories (8 min)** — "Walk me through the last time you [problem space]." Stay in past tense. Resist hypotheticals.
   - **Workarounds (5 min)** — "What did you try first? What didn't work?"
   - **Cost (5 min)** — How long did this take? What did it block?
   - **Wrap (5 min)** — Anyone else we should talk to? Anything we didn't ask about?

3. **Strip every leading question.** Replace anything that contains "would you", "do you wish", "would you pay" with a past-tense story prompt.

## Mode 2 — Synthesis

When the user has notes from a call and wants to extract signal:

1. **Quote actual phrasing.** Pull 3-5 verbatim quotes — the customer's exact words, not your paraphrase.
2. **List the workarounds** they mentioned (these are gold — every workaround is a problem worth solving).
3. **Note what they spent money or time on.** Stated preferences lie; revealed ones don't.
4. **Flag anything that contradicts your hypothesis.** Disconfirming evidence first.
5. **Output in this shape:**

   ```markdown
   ## Verbatim
   - "..."
   - "..."

   ## Current workarounds
   - ...

   ## Revealed preferences
   - Pays $X / month for ...
   - Spends N hrs / week on ...

   ## What we got wrong
   - We assumed X. They actually ...

   ## Next interviews
   - Talk to someone who ...
   ```

## Don't

- Don't ask "would you use this?" — answer is always yes, signal is zero.
- Don't pitch your product mid-interview. You're listening.
- Don't synthesize before you've quoted. The quotes anchor everything else.
