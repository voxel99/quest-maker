# UMOV Quest Maker

Design branching text quests with Claude and save them as drafts in your [UMOV](https://umov.net) account.

UMOV is a puzzle and brain-training platform with crosswords, logic puzzles, quizzes and
interactive quests. A UMOV quest is a graph: locations joined by choices, facts that open new
paths, characters and dialogues, clues, puzzles, and several endings. This plugin teaches Claude
how to build that graph from your story idea and check it with the UMOV quest engine, so nothing
reaches you with broken links, unreachable rooms or endings nobody can get to.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## What's in the plugin

- **`quest-designer` skill.** Guides the work in order: agree on the premise, the true story
  behind it, suspects or obstacles, clues, and endings; lay them out as locations, choices and
  facts; then generate the graph and verify it with the engine before handing it over.
- **UMOV Quest connector** (`.mcp.json`). A remote MCP server at `https://backeu.umov.net/mcp`
  that provides the quest schema, the validator, the simulator and, after you sign in, your drafts.

## Tools

Four tools work without an account:

| Tool | What it does |
|---|---|
| `quest_get_capabilities` | Quest kinds (investigation, walkthrough), supported mechanics and puzzles, reserved facts (`lives`, `coins`, `rating`), ID rules. |
| `quest_get_schema` | JSON Schema for a bare quest graph and for a versioned `umov.quest-package`. |
| `quest_validate` | Structural check. Returns errors and warnings with JSON Pointers, plus non-blocking suggestions (rewards, clues, extra endings, puzzles, dice rolls). |
| `quest_simulate` | Static playthrough. Reports which locations and endings are reachable, dead ends, and the maximum reward. |

Four tools act on your UMOV account and require signing in:

| Tool | What it does |
|---|---|
| `quest_list_mine` | Lists your quests: title, status, revision, editor link. |
| `quest_get_mine` | Returns one of your quests as a package, so Claude can extend it. |
| `quest_create_draft` | Validates a package and saves it as a new **draft**. Returns the editor link and the list of puzzles you still need to set up by hand. |
| `quest_update_draft` | Replaces the graph of your draft. Needs the current revision, so a stale copy can't overwrite newer edits. |

Claude never publishes anything. Drafts stay private until you open the editor link and publish
them yourself, and published quests go through the usual UMOV moderation.

## Installation

**Claude Directory.** Find **UMOV Quest Maker** in the Claude directory and add it.

**Connector only (claude.ai or Claude Desktop).** Settings → Connectors → *Add custom connector*,
URL `https://backeu.umov.net/mcp`. You get the tools without the skill.

**Claude Code.**

```bash
claude mcp add --transport http umov-quests https://backeu.umov.net/mcp
```

## Signing in

The read-only tools need no account. The first time Claude calls a tool that touches your
drafts, your client opens a UMOV sign-in page (OAuth 2.1 with PKCE). You log in or register on
umov.net and approve access. Claude receives a token limited to your quests (`quests:read`,
`quests:write`); it never sees your password. To cut access, disconnect the connector in your
client; the token itself can be revoked through the standard OAuth revoke endpoint
(`/oauth/revoke`).

## What you finish in the editor

Text content is complete when it leaves Claude: locations, choices, facts, rules, dialogues,
clues, trivia questions, tasks with text answers, and endings. Some things are left for the
UMOV editor:

- **Images.** Claude doesn't generate pictures. It can write a media plan (what each scene should
  show), which is saved with the draft as an author note.
- **Visual puzzles.** Find-the-object, spot-the-difference and jigsaw puzzles need images, so
  Claude inserts placeholders. `quest_create_draft` returns the list of placeholders to fill in.

You can also try a quest graph without an account in the sandbox at
[umov.net/test-quest](https://umov.net/test-quest).

## Example requests

- *"Let's make a detective quest: a stolen emerald in a Victorian manor, three suspects, a safe
  whose code you find in the study, a correct accusation and two false ones."*
- *"Check this quest graph and fix whatever the engine complains about."* (paste the JSON)
- *"Show my UMOV quests and add a branch to the lighthouse draft where the keeper lies."*
- *"Here's the plot of my short story, turn it into an investigation quest with a false ending."*

## Privacy and limits

- The connector receives only the quest graph and the tool arguments. Your conversation, and
  any book or file you give Claude to adapt, are not sent to UMOV.
- Signed-in calls are tied to your UMOV account. The server keeps your drafts and the OAuth
  tokens; deleting your UMOV profile removes them together with your content.
- Limits: 20 new drafts per user per day, up to 500 locations and 1000 transitions per quest,
  request body up to 2 MB.
- Privacy policy: [umov.net/privacy](https://umov.net/privacy). Connector documentation:
  [umov.net/docs/mcp](https://umov.net/docs/mcp). Contact: support@umov.net.

## Repository layout

```text
quest-maker/
├── .claude-plugin/plugin.json      # plugin manifest
├── .mcp.json                       # UMOV Quest connector
├── skills/quest-designer/SKILL.md  # quest design process
├── examples/haunted-mansion.json   # sample quest graph that passes validation
├── evals/                          # `claude plugin eval` cases with mocked MCP responses
├── README.md
└── LICENSE
```

## License

[MIT](LICENSE). Copyright (c) 2026 voxel99 / UMOV.
