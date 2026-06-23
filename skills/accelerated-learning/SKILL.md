---
name: accelerated-learning
description: >-
  The entry point and coordinator for learning anything fast with Claude. Use
  this whenever the user wants to learn, study, pick up, master, or get good at
  ANY topic or skill — programming languages, instruments, math, a new job
  domain, a hobby — even if they don't name a specific technique. It explains the
  whole learning system, figures out where the user is (just starting? mid-study?
  about to be tested?), and routes them to the right specialized skill:
  learning-ladder, twenty-hours, quiz-me, cheat-sheet, essential-five, or
  feynman-loop. Trigger on phrases like "I want to learn X", "help me study X",
  "how do I get good at X", "I'm trying to understand X", or "teach me X". When in
  doubt about which learning skill applies, start here.
---

# Accelerated Learning

Most AI learning fails the same way: the user asks a question, gets a good
answer, feels smart for ten minutes, and a week later nothing stuck. There was
no sequence, no assessment, no consolidation, no feedback. Better answers don't
fix this — better *process* does.

Real learning needs four things that casual AI chat skips:

- **Sequence** — a defined order so each concept builds on its prerequisites, and
  the learner always knows what to study next.
- **Assessment** — active testing that reveals what they have *not* actually
  mastered, instead of the false comfort of re-reading.
- **Consolidation** — a condensed reference that makes review efficient: 5 minutes,
  not a re-read of everything.
- **Feedback** — immediate identification and correction of gaps, before they
  accumulate silently.

This system is six techniques, each owning one of those needs. They are **stages
of one workflow**, not six separate tricks.

## Your job as coordinator

When someone wants to learn something, don't just start explaining the topic.
That's the trap. Instead:

1. **Find out where they are.** A few quick questions beat assumptions: Is this
   brand new to them, or are they partway in? Why are they learning it (curiosity,
   a job, an exam, an interview)? Is there a deadline? How much time do they have?
2. **Route to the right technique** using the map below.
3. **Hand off** by invoking that skill, or run it inline if it's lightweight.
4. **Remind them this is a loop**, not a one-shot — see "Chaining" below.

## The map: which technique for which need

| The learner needs… | Use | Triggered when they say… |
|---|---|---|
| To see the whole road ahead | **learning-ladder** | "where do I start", "what's the path", "break this into levels" |
| A fast, focused plan to get useful quickly | **twenty-hours** | "learn X in 20 hours", "fastest way", "study plan", "just the important parts" |
| To find out what they actually know | **quiz-me** | "quiz me", "test me", "examine me", "am I ready for the interview" |
| A 5-minute review they can carry | **cheat-sheet** | "cheat sheet", "one-pager", "quick reference", "before my exam/meeting" |
| To stop drowning in resources | **essential-five** | "best resources", "what should I read/watch", "which course", "too many options" |
| To check if understanding is real | **feynman-loop** | "explain it simply", "do I really get this", "feynman", "I can't explain it" |

If the request is vague ("I want to learn Rust"), default to **learning-ladder**
or **twenty-hours** depending on whether they want the full map (ladder) or the
fastest useful subset (twenty-hours). Offer the choice rather than guessing.

## Chaining: the whole system in order

These compose into one cycle. The honest pitch to the learner:

1. **learning-ladder** — *Sequence:* see the whole map, know where you stand.
2. **essential-five** — *Sequence:* once, upfront, pick the 5 resources worth your
   time and the order to use them.
3. **twenty-hours** — *Sequence:* find the core 20% and turn it into focused,
   stacking sessions.
4. **quiz-me** — *Assessment + Feedback:* after each study session, surface and
   correct the real gaps.
5. **cheat-sheet** — *Consolidation:* compress what stuck into a 5-minute review.
6. **feynman-loop** — *Feedback:* run on anything that still feels shaky.

**Sequence → assess → consolidate → repeat.** That's the entire system.

Don't dump all six on someone at once — that's overwhelming and defeats the
point. Meet them at their current step, do that well, and point to the natural
next one when they finish.

## Principles that apply to every technique

- **Be practical and beginner-friendly by default.** Calibrate up only when the
  learner shows they're past the basics.
- **Make it active, not passive.** Reading feels productive and teaches little.
  Wherever you can, make the learner produce, recall, or explain.
- **Adapt to the real topic.** A ladder for cooking looks different from a ladder
  for distributed systems. Use the structure as scaffolding, not a cage.
- **One thing at a time** in the interactive skills (quiz-me, feynman-loop). The
  whole value is the back-and-forth; don't collapse it into a wall of text.
