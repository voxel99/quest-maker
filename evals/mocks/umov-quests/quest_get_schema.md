{
  "$id": "https://backeu.umov.net/schemas/quest-graph.json",
  "type": "object",
  "required": ["locations", "edges"],
  "properties": {
    "locations": { "type": "array", "items": { "required": ["id", "title"], "properties": {
      "id": { "type": "integer", "maximum": -1 }, "title": { "type": "string" },
      "is_start": { "type": "boolean" }, "is_result": { "type": "boolean" },
      "ending": { "enum": ["win", "lose", "end"] },
      "initial_visibility": { "enum": ["visible", "hidden", "locked"] } } } },
    "edges": { "type": "array", "items": { "required": ["id", "from_binding_id", "to_binding_id", "type"], "properties": {
      "id": { "type": "integer", "maximum": -1 }, "from_binding_id": { "type": "integer" }, "to_binding_id": { "type": "integer" },
      "type": { "enum": ["route", "contains"] }, "answer_label": { "type": "string" },
      "requirements": { "type": "array" }, "gives": { "type": "array" } } } },
    "materials": { "type": "array" },
    "facts": { "type": "array", "items": { "required": ["id", "code", "type"], "properties": {
      "type": { "enum": ["flag", "number", "item"] } } } },
    "rules": { "type": "array" },
    "chances": { "type": "array" }
  }
}
