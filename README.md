# FlowLink — Pitch & Agent-Native Proof

Pitch-deck artifacts, comparison slides, and real agent-testing logs for **FlowLink** —
the compliance-first payment layer for the agent economy on HashKey Chain.

**Companion code:** [Akasxh/flowlink](https://github.com/Akasxh/flowlink) — the clean agent-native rewrite.

## What's in here

| Path | What |
|---|---|
| `index.html` | Original 16:9 agent-native pitch slide. "Markdown is the API." |
| `comparison.html` | Side-by-side replay slide: agent with vs without FlowLink's `.md` skill surface. Numbers from real runs. |
| `FlowLink_The_Payment_Layer_for_the_Agent_Economy.pdf` | Full pitch deck. |
| `agent-logs/round{1..5}/*.json` | Transcripts from 22 real Claude agent probes across 5 iterations. Each JSON has tool_calls, bytes, final_state, observations. |
| `flowlink-agent-native-slide.png`, `slide-*.png`, `flowlink-comparison-full.png` | Rendered slide images. |

## The comparison headline

| Metric | Agent + `.md` skills | Agent + Playwright only |
|---|---|---|
| Avg tool calls | 6 – 10 | 15 – 30 |
| Avg context tokens | 39 – 51K | 51 – 58K |
| Avg duration | 31 – 147 s | 106 – 188 s |
| Task completion | **7 / 7** | **1 / 4** (cheated to read `.md` anyway) |

## The bug real agents caught

One Playwright agent flagged a Problem+JSON contract violation on
`POST /v1/compliance/check` (custom 403 schema instead of RFC 9457). Another round caught a P0 —
`POST /v1/auth/siwe/nonce` returning HTTP 500 on every call because the SIWE default statement
contained a Unicode em-dash (U+2014) that the EIP-4361 ABNF parser rejects. Both fixed and
committed in [Akasxh/flowlink@811b45c](https://github.com/Akasxh/flowlink/commit/811b45c).

## Viewing the comparison

```bash
python3 -m http.server 8080
# then open http://localhost:8080/comparison.html
```

Click "Replay real agent run" to watch both logs animate side-by-side. The counters (tool calls,
context tokens) grow in real time.
