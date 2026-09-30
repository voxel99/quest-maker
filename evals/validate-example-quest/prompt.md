---
max_turns: 15
timeout_seconds: 300
allowed_tools: [Skill]
---

Here is the graph of my UMOV quest "The Mystery of Blackwood Manor" (the repo's examples/haunted-mansion.json). Check it with the UMOV engine: is the quest structurally valid, are all locations and endings reachable, and is it ready to publish? Give me a short verdict.

```json
{
  "locations": [
    {"id": -1, "title": "Вход в поместье", "is_start": true},
    {"id": -2, "title": "Тёмный коридор"},
    {"id": -3, "title": "Старинная библиотека"},
    {"id": -4, "title": "Тайная сокровищница", "is_result": true, "ending": "win"},
    {"id": -5, "title": "Ловушка с шипами", "is_result": true, "ending": "lose"},
    {"id": -6, "title": "Бегство через ворота", "is_result": true, "ending": "end"}
  ],
  "edges": [
    {"id": -10, "from_binding_id": -1, "to_binding_id": -2, "type": "route", "answer_label": "Толкнуть массивную дубовую дверь и войти в дом"},
    {"id": -11, "from_binding_id": -1, "to_binding_id": -6, "type": "route", "answer_label": "Испугаться грозы и убежать через ворота"},
    {"id": -12, "from_binding_id": -2, "to_binding_id": -3, "type": "route", "answer_label": "Осторожно пройти по коридору в библиотеку"},
    {"id": -13, "from_binding_id": -2, "to_binding_id": -5, "type": "route", "answer_label": "Шагнуть в тёмную неосвещенную нишу"},
    {"id": -14, "from_binding_id": -3, "to_binding_id": -4, "type": "route", "answer_label": "Потянуть за корешок синей книги с гербом"}
  ],
  "materials": [
    {"id": -20, "title": "У входа", "location_binding_id": -1, "upload_type": "text"},
    {"id": -21, "title": "В коридоре", "location_binding_id": -2, "upload_type": "text"},
    {"id": -22, "title": "В библиотеке", "location_binding_id": -3, "upload_type": "text"},
    {"id": -23, "title": "Триумф", "location_binding_id": -4, "upload_type": "text"},
    {"id": -24, "title": "Провал", "location_binding_id": -5, "upload_type": "text"},
    {"id": -25, "title": "Отступление", "location_binding_id": -6, "upload_type": "text"}
  ]
}
```
