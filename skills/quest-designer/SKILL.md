---
name: quest-designer
description: Design, adapt, check and save interactive text quests (detective investigations, text adventures, branching stories) for the UMOV platform. Use when the user wants to create a quest or text adventure, turn a story or book into a playable quest, fix or extend a quest graph, or save a quest as a UMOV draft.
---

# UMOV Quest Designer

Help the author turn an idea (or a source text) into a UMOV quest: a graph of locations,
choices, characters, facts and endings that the UMOV engine can play. Work with the author
step by step, check the result with the engine, and, when asked, save it as a draft in the
author's UMOV account.

The UMOV Quest connector provides:

| Tool | Account | Use |
|---|---|---|
| `quest_get_capabilities` | no | supported mechanics, puzzle types, reserved facts, ID rules |
| `quest_get_schema` | no | JSON Schema of a bare graph and of `umov.quest-package` |
| `quest_validate` | no | structural check: `errors`, `warnings`, `suggestions` (JSON Pointers) |
| `quest_simulate` | no | static playthrough: reachable locations and endings, dead ends, max reward |
| `quest_list_mine` | yes | the author's quests with `widget_id`, `revision`, `editor_url` |
| `quest_get_mine` | yes | package of one of the author's quests |
| `quest_create_draft` | yes | validate and save as a new draft; returns `editor_url` and unfinished placeholders |
| `quest_update_draft` | yes | overwrite a draft's graph; needs the current `revision` |

The account tools make the client ask the author to sign in to UMOV. Call them only when the
author wants to save, list or edit their quests.

## Workflow

Go through the steps in order. Don't generate the full JSON before the author approves the
outline (step 4) unless they explicitly ask to skip questions.

### 0. Source and rights (adaptations only)

If the quest is based on someone else's work (book, story, film, game), settle this first:

1. Find the author, the year of the author's death and the year of first publication.
2. UMOV's audience is mainly Russia and the EU: a work is free there 70 years after the author's
   death (in 2026: authors who died before 1956). In the US, works published before 1931 are free.
   A work must be free in both to be adapted directly.
3. **Free** (e.g. Conan Doyle's Sherlock Holmes, Poe, Chesterton's Father Brown, Chekhov):
   adapt it and credit the source ("based on the story by A. Conan Doyle").
4. **Protected or unclear**: don't adapt it. Offer a quest *inspired by* it: own setting, era,
   names, clues and dialogue; keep only unprotectable ideas (a closed circle of suspects, a locked
   room, the type of twist). Don't use the original's character names, titles or trademarks.
   If the author says they hold a license, go ahead on their word.
5. In any case, write dialogue and texts yourself; quote the source only in short phrases.

The source text stays in the conversation. Only the finished graph is sent to UMOV.

### 1. Brief

Agree on:

- **Kind**: `investigation` (the player collects facts in a notebook and makes an accusation)
  or `walkthrough` (the player gets through obstacles to a goal).
- Premise, setting, the player's role and goal.
- Size: a short quest is 5–10 locations and 2–4 endings; a large one is 15–30 locations,
  usually split into sections.
- Language of the quest (default: the author's language).
- Whether there will be pictures. Claude doesn't create images; the author adds them in the
  UMOV editor. Without pictures the quest is text-only and fully playable.

### 2. Canon

Before building mechanics, write down what *really* happened (for an investigation) or what
the world is (for a walkthrough):

- the true course of events with a timeline;
- characters: who they are, what they know, what they hide, their alibis;
- evidence, and which false leads point where;
- the ending conditions: what the player must know or have for each ending.

Look for inconsistencies (times, distances, who could have seen what, clues that prove too much
or too little) and resolve them with the author.

### 3. Mechanics

Map the canon onto engine blocks:

- **Locations**: places the player visits. A `contains` edge nests a location inside another
  (rooms of a house); a `route` edge is a transition with a button (`answer_label`).
  Locations can have conditional `texts` that change with the facts.
- **Characters** with **placements** (who stands where, and when) and **dialogues** (question →
  answer). Dialogues are the main way to reveal information and give facts.
- **Facts**: `event` (it happened / the player knows it) or `stat` (a number: health, evidence
  count, trust). Events with `milestone` (1–100) show investigation progress.
- **Conditions** (`requirements`) on anything: locations, edges, placements, dialogues, materials.
- **Effects** (`gives`): set events, change stats, roll dice.
- **Rules**: fire once when their conditions are met. Use them for hidden counters ("three
  pieces of evidence found → the suspect panics") and for endings: a rule with `goto` sends the
  player to an ending location.
- **Endings**: locations with `is_result: true` and `ending` = `win`, `lose` or `end`.
  An investigation usually has one true accusation and several false ones.
- **Puzzles**: `trivia` and `problem` (task with a text answer) can be written completely.
  Visual puzzles (`imagesearch`, `imagecompare`, `jigsaw`) need pictures: add them as
  placeholders for the author to finish in the editor.
- **Chances**: a dice roll with weighted outcomes, for risky actions.
- **Sections**: groups of locations with an entrance and exits, nested up to 3 levels. Use them
  for large quests (a building, a train, a district).
- **Rewards**: `coins` and `rating` are built-in facts. Give them in winning rules
  (`"coins+10"`); the engine limits the total by the author's reputation. `lives` are the
  player's global lives, shared with the rest of UMOV.

### 4. Outline for approval

Show a compact outline: locations and how they connect (a list or a Mermaid diagram), characters
and what each reveals, key facts, rules and endings with their conditions. Ask the author to
confirm or change it. Build the JSON only after they agree.

### 5. Build and check

1. Call `quest_get_capabilities` and `quest_get_schema` once per conversation. The schema is
   authoritative: if it disagrees with this guide, follow the schema.
2. Build a package (format below).
3. Call `quest_validate`. Pass `draft: true` if the quest has visual puzzle placeholders.
   Fix every item in `errors` (each has a JSON Pointer) and validate again until `errors` is empty.
4. Call `quest_simulate`. Check that `playable` is true (not expected with placeholders),
   `dead_ends` is empty, and every location and ending is reachable. Fix what isn't.
5. Engine `suggestions` are optional. Mention one or two that fit the story and let the
   author decide; don't add them on your own.

### 6. Hand over

- **Save**: if the author wants the quest in their account, call `quest_create_draft` with the
  package (and `media_plan` if there are pictures to make). Give the author the `editor_url`
  and a to-do list: pictures from the media plan, and each item in `placeholders`.
- **Without saving**: give the package JSON and point to the sandbox at
  `https://umov.net/test-quest`, where a graph can be played without an account.
- **Editing an existing quest**: `quest_list_mine` → `quest_get_mine` → change the graph →
  `quest_validate` → `quest_update_draft` with the `revision` from `quest_get_mine`. On
  `revision_conflict` the draft was changed in the editor: fetch it again, reapply the change
  and tell the author.

Drafts are private. The author publishes from the editor; Claude can't publish.

## Graph format

```json
{
  "format": "umov.quest-package",
  "version": 1,
  "brief": { "title": "…", "description": "…", "language": "en", "images": false },
  "graph": {
    "story": { "kind": "investigation" },
    "locations": [], "edges": [], "characters": [], "placements": [],
    "dialogues": [], "materials": [], "facts": [], "rules": [], "chances": [], "sections": []
  },
  "media_plan": {}
}
```

**IDs.** Every new location, edge, character, placement, dialogue and material gets a unique
negative integer (`-1`, `-2`, …); references (`from_binding_id`, `to_binding_id`,
`location_binding_id`, `character_binding_id`, `goto`) use them too. Never invent positive IDs
or `upload_id`s. Facts are identified by `key`; rules, chances and sections by short string `id`s.

**Locations**:
`{"id": -2, "title": "Police station", "description": "…", "is_start": true, "initial_visibility": "visible", "texts": [{"text": "The door is locked.", "requirements": ["sheriff_away"]}]}`.
Exactly one location has `is_start: true`. Endings:
`{"id": -9, "title": "Wrong man arrested", "is_result": true, "ending": "lose"}`.

**Edges**:
`{"id": -10, "from_binding_id": -1, "to_binding_id": -2, "type": "route", "answer_label": "Go to the station", "requirements": ["has_map"]}`;
use `"type": "contains"` (no label) to put a location inside another.

**Characters, placements, dialogues**:

```json
{"id": -21, "name": "Sheriff Cole", "interaction": "talk", "initial_visibility": "visible"}
{"id": -31, "character_binding_id": -21, "location_binding_id": -2, "initial_state": "visible", "requirements": ["!sheriff_away"]}
{"id": -201, "character_binding_id": -21, "question": "What happened?", "answer": "…", "initial_state": "enabled", "requirements": [], "gives": ["task"]}
```

**Materials** (text shown in a location, or the intro when `location_binding_id` is null):
`{"id": -64, "title": "The call", "upload_type": null, "upload_id": null, "is_intro": true, "body_html": "<p>…</p>", "location_binding_id": null, "initial_visibility": "visible"}`.
For `trivia` / `problem` and visual placeholders, take the `upload` structure from
`quest_get_schema` and `quest_get_capabilities`.

**Facts**:
`{"key": "saw_footprints", "type": "event", "title": "Found footprints by the fence", "milestone": 20}`,
`{"key": "evidence", "type": "stat", "title": "Evidence against the miller", "initial_value": 0, "min_value": 0, "max_value": 5, "visible": false}`.
Facts referenced in conditions but not declared are created automatically, but declare them to
give the player readable titles.

**Conditions** (`requirements`, all must hold): `"key"` (event happened), `"!key"` (not
happened), `"evidence>=3"`, `"trust<2"`, `"hp=0"` (also `<=`, `>`, `!=`).

**Effects** (`gives`): `"key"` (event happens), `"evidence+1"`, `"hp-1"`, `"hp=5"`,
`"coins+10"`, `"rating+5"`, `"roll:door_check"` (roll a chance).

**Rules**:
`{"id": "win", "title": "Case closed", "requirements": ["accused_miller", "evidence>=3"], "gives": ["coins+10"], "goto": -8}`.

**Chances**:
`{"id": "door_check", "title": "Force the door", "outcomes": [{"id": "ok", "title": "It gives way", "weight": 2, "gives": ["door_open"]}, {"id": "hurt", "title": "You hurt your shoulder", "weight": 1, "gives": ["hp-1"]}]}`.

**Sections**:
`{"id": "manor", "label": "Manor", "location_ids": [-3, -4, -5], "start_id": -3, "parent_id": null}`.

## Limits

- Up to 500 locations and 1000 edges per quest; 20 new drafts per author per day.
- Claude doesn't generate images, audio or video. For pictures, describe what each scene needs
  in `media_plan`; the author uploads them in the editor.
- `quest_simulate` is static analysis. For exact behaviour the author plays the draft in the
  editor or sandbox.
