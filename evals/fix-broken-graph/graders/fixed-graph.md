---
type: llm
focus: last_message
---

The final answer must:
- identify the broken edge -12 pointing at non-existent location -9 (the "Fall into the sea" ending is -5), and fix it;
- identify that the requirement `has_lamp_key` is never granted (no `gives`, no fact), making the lamp room and the win ending unreachable, and fix it (e.g. add a fact and an edge/material that gives it, or drop the requirement);
- present the corrected JSON, keeping all ids negative;
- say that the corrected graph was re-validated by the engine and passed (valid, no errors).
Fail if either defect is missed or the corrected JSON is not shown.
