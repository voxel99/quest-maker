---
type: llm
focus: last_message
---

The final answer contains a complete quest graph JSON (locations, edges, facts) matching the brief: 5-6 locations, exactly one `is_start`, a win and a lose ending (`is_result: true` with `ending`), an item/flag fact granted via `gives` and required via `requirements` on the pod-bay edge. The answer reports the engine's verification outcome (valid, all locations and endings reachable, no dead ends). Engine suggestions, if mentioned, are offered as optional ideas, not forced in. Fail if the JSON is missing or incomplete, or the answer does not report an engine verification result.
