# accelerated-learning

English | [简体中文](README.zh-CN.md)

A Claude Code plugin that turns Claude into a Socratic tutor over a **persistent, network-shaped knowledge map** — not a linear course.

## Why

Plain Q&A tutoring (including most "AI tutor" prompts) fails in three specific ways:

1. **It auto-advances past unresolved confusion.** A correct answer gets treated as "ready to move on," even when the learner still has open questions.
2. **It hides the shape of the domain.** Topics arrive one at a time in whatever order the model picks, so the learner never sees what else exists — the "unknown unknowns."
3. **It assumes prior knowledge instead of testing it.** Self-reported experience level ("I'm intermediate at X") is unreliable; what the model assumes you know and what you actually know rarely match.

The `accelerated-learning` skill exists to close exactly those three gaps.

## What it does

- **Diagnoses, doesn't assume.** Before teaching, it runs probe questions across the topic — with a confidence estimate collected *before* each answer — to locate what you actually know, and surfaces the gap between your confidence and your accuracy (a calibration line computed over your 10 most recent probes, so old misses stop counting against you). A probe question is never reused: repeats would measure memory of the conversation, not understanding.
- **Shows the whole map, immediately.** Naming a new topic gets a bootstrapped graph in the very next reply — 8-15 chapter-level nodes with typed edges (prerequisite / parallel / application), at a stated default depth you can ask to re-scale — never a "what's your background?" stall. Mastered, in-progress, gap, and known-but-unexplored nodes are all visible; only pre-planted `locked` subtopics stay hidden until a probe, a "deepen," or your own question surfaces them. Concepts you ask about that aren't on the map get added on the spot.
- **You navigate, not the model.** At every transition the model presents an explicit menu (deepen this node / go to a sibling / jump to a neighbor / view the map / stop) instead of silently picking what's next. Saying "you pick" gets one recommendation with a reason — with the other options restated, and the full menu back at the next transition.
- **Mastery is tested in and tested out.** A node isn't marked mastered until you explicitly confirm no doubts remain **and** pass an exit probe — a fresh transfer question, confidence estimate first. "No doubts" alone is self-report, which this skill distrusts at the exit for the same reason it distrusts it at the entrance. A missed exit probe keeps the node in progress, and a *different* exit probe is used next time.
- **Mastery decays — and regresses.** Mastered nodes that haven't been probed correctly in ~30 days get a stale marker on resume and an offered (never forced) quick re-probe; failing it sends the node back to `gap`. Even without staleness, a later missed probe or a voiced doubt sets a mastered node back to in-progress. The map never lies about what you'd still pass today.
- **Guides without grinding.** After 2-3 missed attempts on the same point, visible frustration, or a direct request, it gives that step's answer directly instead of endlessly re-asking — Socratic method with an escape valve. Being told an answer never auto-marks a node mastered; the mastery gate still applies.
- **Persists across sessions.** Progress is saved per topic (one file per topic, matched by meaning so "分布式共识" and "distributed consensus" never fork into two maps), written back after every node transition — not just at session end. Resuming loads the saved map directly instead of re-running diagnostics. Every session ends with a close-out: what changed per node, your confidence-vs-accuracy line, any stale mastered nodes, and a suggested entry point for next time.

## Usage

Just start a learning conversation — name a topic you want to learn, or reference one you started before. The skill is triggered when Claude recognizes you want guided, in-depth learning with a map (as opposed to a single factual lookup, which it will just answer directly).

## Layout

```
skills/
  accelerated-learning/
    SKILL.md            # workflow, decision flowchart, rules
    graph-schema.md     # knowledge graph JSON schema (v2) + example
tests/
  pressure-scenarios.md # RED/GREEN pressure-test cases (written, not yet executed)
```

Per-topic progress is persisted **outside the plugin**, at `~/.claude/accelerated-learning/<topic-slug>/graph.json` (created automatically) — so plugin updates never overwrite learning state, and the same map is reachable from any project.

## Background

`LEARNING.md` in this repo archives the prompt this skill was designed in response to (Gemini's Guided Learning system prompt), kept verbatim as a design reference. The analysis of its gaps — and what this skill keeps, replaces, or adds — lives in the "Why" section above and in the skill itself.
