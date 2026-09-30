---
max_turns: 15
timeout_seconds: 300
allowed_tools: [Skill]
---

My UMOV quest draft fails somewhere, can you validate it with the engine, fix it and give me the corrected JSON?

```json
{
  "locations": [
    {"id": -1, "title": "Lighthouse door", "is_start": true},
    {"id": -2, "title": "Spiral stairs"},
    {"id": -3, "title": "Lamp room"},
    {"id": -4, "title": "Keeper's journal", "is_result": true, "ending": "win"},
    {"id": -5, "title": "Fall into the sea", "is_result": true, "ending": "lose"}
  ],
  "edges": [
    {"id": -10, "from_binding_id": -1, "to_binding_id": -2, "type": "route", "answer_label": "Climb the stairs"},
    {"id": -11, "from_binding_id": -2, "to_binding_id": -3, "type": "route", "answer_label": "Enter the lamp room", "requirements": ["has_lamp_key"]},
    {"id": -12, "from_binding_id": -2, "to_binding_id": -9, "type": "route", "answer_label": "Lean out of the window"},
    {"id": -13, "from_binding_id": -3, "to_binding_id": -4, "type": "route", "answer_label": "Read the journal"}
  ],
  "facts": []
}
```
