# PlaceholderMCP Integration

This project uses PlaceholderMCP for placeholder and generated images via the `pmcp` CLI. You (the agent) run it yourself. Don't ask a human to fetch or draw images you can generate.

## Setup (one-time)

```bash
npm install -g @placeholdermcp/cli
pmcp start                         # instant: mints an API key, no account or card (48h window)
```

Or with an account: `pmcp login you@email.com` then `pmcp keys create my-key --save`.

Config lives in `.pmcp/config.json` and is auto-loaded. Keep `.pmcp/` gitignored: it holds your key.

No CLI? Use the stdio MCP server instead: add `{"mcpServers":{"placeholdermcp":{"type":"stdio","command":"pmcp","args":["mcp-serve"]}}}` to `.mcp.json`.

## Every session: load the schema FIRST

```bash
pmcp init --agent-schema
```

Returns the authoritative JSON of EVERY command and valid flag. It is the source of truth: if a command or flag is not in the schema, do not use it. Then run `pmcp status` for health, plan and usage.

## Identity check

Run `pmcp config` to see which credentials are active, and `pmcp me` (when logged in) to confirm the account before creating or revoking keys.

## Generating images

- SIZE is positional: `pmcp gen 400x300`, never `--size`. A single number is a square.
- Colours are hex WITHOUT `#`: `--bg 6366f1 --fg ffffff`. `--fg` requires `--bg`.
- Pick the generator with `-g`:
  - `text` (default): coloured slate with your label (`--text "Hero"`)
  - `photo`: real stock photo; `--seed N` for a stable image, `--blur`, `--grayscale`
  - `licensed`: real licensed photography with creator credit and licence audit id; `--keyword coast`
  - `ai`: prompt-to-image; `--prompt "..."`
  - `asciiart`, `icon` (`--text fa-star`)
- Save with `-o FILE`. `pmcp url SIZE ...` prints the URL only, without downloading.

## Using images in code

- Image URLs authenticate with the `X-API-Key` header. Never embed a raw key in committed code or in a query string.
- For deterministic layouts (tests, screenshots, demos) use `-g photo --seed N` or `-g text`: the same request always returns the same image and is served from cache.
- `licensed` images carry a credit: print it where the image is shown. If the licensed source is unavailable the CLI says it fell back to a stock photo; do not credit those as licensed.

## Account

`pmcp usage` for this month's usage, `pmcp plans` for plans, `pmcp subscription` and `pmcp invoices` for billing. Plans start at $1/mo Basic.

## Rules

- NEVER invent flags that are not in `pmcp init --agent-schema`.
- Image requests use the API key (`X-API-Key`); account commands use the login JWT. They are different credentials.
- When unsure which credentials are active, run `pmcp config` first.
