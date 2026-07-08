# Knowledge Graph Schema (v2)

One JSON file per topic at `~/.claude/accelerated-learning/<topic-slug>/graph.json` (created automatically; never stored under the plugin/skill directory).

**Slug rule:** before creating anything, list existing `~/.claude/accelerated-learning/*/graph.json` files and match the named topic against each `topic` field **by meaning, across languages** ("分布式共识" matches "Distributed Systems Consensus"). Reuse on match. Only when nothing matches, mint a slug: short kebab-case ASCII English translation of the topic (never slug non-ASCII text directly).

## Fields

- `schema_version` (int) — currently `2`. Files with an older/missing version are upgraded in place (add missing fields with empty or derived values), never discarded.
- `topic` (string) — human-readable topic name, in the learner's language
- `created` / `updated` (ISO date)
- `mode` — `"depth-first"` or `"breadth-first"`, last mode the learner chose
- `current_node` — id of the node currently in the Socratic loop, or `null` between nodes
- `nodes[]` — `{ id, label, status, confidence_log, notes }`
  - `status`: `locked` | `visible-unexplored` | `gap` | `in_progress` | `mastered`
  - `confidence_log`: array of `{ estimate: 0-100, correct: bool, date: "YYYY-MM-DD", probe: "<one-line question summary>" }` — every probe on this node, diagnostic and exit alike. `probe` exists so the same question is never asked twice; `date` powers the staleness check and the recency-windowed calibration line.
  - `notes`: free text — specific misconceptions surfaced, analogies that worked, open questions. Written to be fed back to the learner (session close-out, resume), not just archived.
- `edges[]` — `{ from, to, type }`, `type`: `prerequisite` | `parallel` | `application`

## Status Transitions

```
locked ──(surfaced by probe / deepen / learner question)──► visible-unexplored
visible-unexplored ──(learner picks it)──► in_progress
visible-unexplored ──(diagnostic probe missed)──► gap
gap ──(learner picks it)──► in_progress
in_progress ──(mastery gate: "no doubts" + exit probe passed)──► mastered
mastered ──(later probe missed, or learner voices a doubt)──► in_progress
mastered ──(stale re-probe failed)──► gap
```

No other transitions. In particular there is no path into `mastered` that skips the exit probe, and `mastered` is not a terminal state.

## Example

```json
{
  "schema_version": 2,
  "topic": "Distributed Systems Consensus",
  "created": "2026-06-02",
  "updated": "2026-07-02",
  "mode": "depth-first",
  "current_node": "raft",
  "nodes": [
    {
      "id": "cap-theorem",
      "label": "CAP Theorem",
      "status": "mastered",
      "confidence_log": [
        {"estimate": 60, "correct": true, "date": "2026-06-02", "probe": "What does CAP force you to give up during a partition?"},
        {"estimate": 70, "correct": true, "date": "2026-06-02", "probe": "Exit: classify a geo-replicated bank ledger as CP or AP and justify"}
      ],
      "notes": "Initially conflated 'partition tolerance' with 'network reliability'; resolved with the banking split-brain example."
    },
    {
      "id": "raft",
      "label": "Raft Consensus",
      "status": "in_progress",
      "confidence_log": [
        {"estimate": 40, "correct": false, "date": "2026-07-02", "probe": "What is the role of the term number in Raft?"}
      ],
      "notes": "Confused leader election term numbers with log indices."
    },
    {
      "id": "paxos",
      "label": "Paxos",
      "status": "visible-unexplored",
      "confidence_log": [],
      "notes": ""
    },
    {
      "id": "leader-election",
      "label": "Leader Election in Practice",
      "status": "gap",
      "confidence_log": [
        {"estimate": 85, "correct": false, "date": "2026-07-02", "probe": "Why can't two nodes both win an election in the same term?"}
      ],
      "notes": "Overconfident here — flagged by calibration gap (85% confidence, wrong answer)."
    },
    {
      "id": "multi-paxos",
      "label": "Multi-Paxos",
      "status": "locked",
      "confidence_log": [],
      "notes": "Pre-planted; surfaces as visible-unexplored once paxos is explored."
    }
  ],
  "edges": [
    {"from": "cap-theorem", "to": "raft", "type": "prerequisite"},
    {"from": "raft", "to": "paxos", "type": "parallel"},
    {"from": "raft", "to": "leader-election", "type": "application"}
  ]
}
```

## Rendering the Map to the Learner

Render as a short nested list grouped by status, not a raw graph dump — in the learner's language:

```
已掌握: CAP 定理 ⏳可能已生疏 (上次答对已是 30 天前, 要不要来一道快速复检?)
正在学: Raft 共识 ← 当前节点
待补缺口: Leader Election 实践应用 (校准偏差: 85%置信度但答错)
已知但未探索: Paxos (与 Raft 平行的另一种共识协议)

校准情况(最近10次探测): 平均置信度 62%, 平均正确率 33% — 存在明显高估
```

- **Calibration line** uses the 10 most recent probes by date across the map — not all-time, so improved calibration actually shows up.
- **Stale marker:** a `mastered` node whose most recent *correct* probe is older than ~30 days gets the "可能已生疏" marker and an offered (never forced) quick re-probe. Failing that re-probe moves the node to `gap`.
- **Locked nodes** are stored in the file but never rendered — flip them to `visible-unexplored` when a diagnostic probe, a "deepen" choice, or a learner question surfaces them.
