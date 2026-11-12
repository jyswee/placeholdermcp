# pmcp — PlaceholderMCP CLI

Placeholder image generation from the command line, for developers and coding agents.
Wraps the PlaceholderMCP API (`api.placeholdermcp.com`). Zero dependencies.

**Works with:** Claude Code · Cursor · Cline · Windsurf · Aider · Codex · any MCP client

[![pmcp demo — login, API key, first image in 60 seconds](https://prodmedia.tyga.host/public/tyga.cloud/landing/placeholdermcp.com/demo/quickstart-demo.gif)](https://placeholdermcp.com/#demo)

*Login to first image in 60 seconds — [watch the full demo](https://placeholdermcp.com/#demo).*

## Install

```bash
npm install -g @placeholdermcp/cli
```

The npm package is [`@placeholdermcp/cli`](https://www.npmjs.com/package/@placeholdermcp/cli); the command is `pmcp`.
Upgrading from the old unscoped package? `npm rm -g placeholdermcp && npm install -g @placeholdermcp/cli`.

## Quickstart

```bash
pmcp start                       # instant: mint a key — no account, no card (48h window)
pmcp gen 400x300 --bg 6366f1 --text "Hello" -o hero.png
```

One command, zero signup. After the 48-hour card-free window, requests return 402 until you add a card ($1/mo Basic) — your key stays valid.

With an account:

```bash
pmcp init                        # setup guide + agent instructions
pmcp login you@email.com         # JWT for account ops
pmcp keys create my-key --save   # API key for image requests (saved to .pmcp/config.json)
```

## Auth model

| Credential | Header | Used for |
|---|---|---|
| JWT (from `pmcp login`) | `Authorization: Bearer <jwt>` | me, keys, usage, billing |
| API key (`key_...`) | `X-API-Key` | image generation, MCP tools |

Config priority: flags (`--key`/`--token`/`--url`) → env (`PMCP_API_KEY`/`PMCP_TOKEN`/`PMCP_URL`) → `.pmcp/config.json` (project-local).

## MCP server

[![pmcp agent demo — agent-readable schema, profile & plan from the terminal](https://prodmedia.tyga.host/public/tyga.cloud/landing/placeholdermcp.com/demo/mcp-demo.gif)](https://placeholdermcp.com/#demo)

*Built for agents — machine-readable schema, profile and plan straight from the terminal.*

Save as `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "placeholdermcp": {
      "type": "stdio",
      "command": "pmcp",
      "args": ["mcp-serve"]
    }
  }
}
```

Tools: `get_image_url`, `generate_placeholder`, `get_usage`, `list_plans`.

## Agent schema

`pmcp init --agent-schema` prints a machine-readable JSON contract of all commands and flags — load it into agent context to prevent flag hallucination.

## Commands

Run `pmcp --help` for the full reference.

## License

**PlaceholderMCP CLI. Copyright (c) 2026 Tyga.Cloud Ltd. All rights reserved.**

PLACEHOLDERMCP is a trade mark of Tyga.Cloud Ltd, asserted as an unregistered mark. Copyright in the Work subsists automatically under Article 5(2) of the Berne Convention for the Protection of Literary and Artistic Works: protection arises on creation, with no registration or formality, in every contracting party to the Convention. The software is further protected as a literary work under Article 4 of the WIPO Copyright Treaty (1996) and Article 10(1) of TRIPS. See [COPYRIGHT](./COPYRIGHT) for the full notice.

The software is licensed under the EULA in [LICENSE](./LICENSE): installing and using it via npm to work with the PlaceholderMCP platform is permitted; everything else is reserved. No further licence, express or implied, is granted by the publication of this repository, by any release attached to it, or by having been given a copy.
