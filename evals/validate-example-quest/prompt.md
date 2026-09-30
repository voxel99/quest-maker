---
max_turns: 15
timeout_seconds: 300
allowed_tools: [Skill]
---

Here is the graph of my UMOV quest "The Mystery of Blackwood Manor" (the repo's examples/haunted-mansion.json). Check it with the UMOV engine: is the quest structurally valid, are all locations and endings reachable, and is it ready to publish? Give me a short verdict.

```json
{
  "locations": [
    {"id": -1, "title": "Manor Entrance", "is_start": true},
    {"id": -2, "title": "Dark Corridor"},
    {"id": -3, "title": "Old Library"},
    {"id": -4, "title": "Secret Treasury", "is_result": true, "ending": "win"},
    {"id": -5, "title": "Spike Trap", "is_result": true, "ending": "lose"},
    {"id": -6, "title": "Escape Through the Gates", "is_result": true, "ending": "end"}
  ],
  "edges": [
    {"id": -10, "from_binding_id": -1, "to_binding_id": -2, "type": "route", "answer_label": "Push the heavy oak door and enter the house"},
    {"id": -11, "from_binding_id": -1, "to_binding_id": -6, "type": "route", "answer_label": "Take fright at the storm and flee through the gates"},
    {"id": -12, "from_binding_id": -2, "to_binding_id": -3, "type": "route", "answer_label": "Carefully walk down the corridor to the library"},
    {"id": -13, "from_binding_id": -2, "to_binding_id": -5, "type": "route", "answer_label": "Step into the dark, unlit alcove"},
    {"id": -14, "from_binding_id": -3, "to_binding_id": -4, "type": "route", "answer_label": "Pull the spine of the blue book with the coat of arms"}
  ],
  "materials": [
    {"id": -20, "title": "At the Entrance", "location_binding_id": -1, "upload_type": "text"},
    {"id": -21, "title": "In the Corridor", "location_binding_id": -2, "upload_type": "text"},
    {"id": -22, "title": "In the Library", "location_binding_id": -3, "upload_type": "text"},
    {"id": -23, "title": "Triumph", "location_binding_id": -4, "upload_type": "text"},
    {"id": -24, "title": "Failure", "location_binding_id": -5, "upload_type": "text"},
    {"id": -25, "title": "Retreat", "location_binding_id": -6, "upload_type": "text"}
  ]
}
```
