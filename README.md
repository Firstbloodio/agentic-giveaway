# Vast giveaways for AI agents

Let your AI agent run giveaways on [Vast](https://vast.gg). It can draft a giveaway, add entry actions such as following on X or joining Discord, preview it, publish it when you say so, and report entries and winners.

This repo works in Claude Code, Codex and any agent that reads [Agent Skills](https://agentskills.io):
- **`skills/agentic-giveaway/SKILL.md`**: teaches the agent how to run a giveaway and what to hand back to you.
- **`.mcp.json`** (Claude Code) and **`mcp.json`** (Codex): connect the agent to Vast's MCP server at `https://general.vast.gg/mcp`.
- **`.claude-plugin/`** and **`.codex-plugin/`**: the plugin manifests and the `vast` marketplace. vast.gg serves the same marketplace at `https://vast.gg/plugins/claude/marketplace.json` for Claude Code and `https://vast.gg/plugins/agentic-giveaway.git` for Codex, so the commands below don't change if this repo moves.
- **`server.json`**: the listing for the [MCP Registry](https://registry.modelcontextprotocol.io).

Paste a Shopify product link and the agent fills the prize from the store's product data (title, variants, price, image).

## Before you start
1. Get a Vast account. Agent keys come with paid plans today. Vast is rolling out agent keys on the free plan too (2 active keys after a one-time email confirmation; each free giveaway admits at most 500 people until the host upgrades) — the Developer page at https://vast.gg/settings/developer shows which applies to this account. Upgrading at https://vast.gg/upgrade lifts the cap.
2. Create an API key: https://vast.gg/settings/developer
3. Keep the key out of chat. Each install below says where it goes.

## Install in Claude Code
```bash
claude plugin marketplace add https://vast.gg/plugins/claude/marketplace.json
claude plugin install agentic-giveaway@vast
```
Then add your key: in Claude Code, run `/plugin configure agentic-giveaway@vast` and paste it into the "Vast API key" field. If you install from `/plugin` inside Claude Code, it asks for the key during install. Claude Code keeps the key in secure storage (the keychain on a Mac), not in a settings file.

Already added the `vast` marketplace from this repo? Keep it: it is the same marketplace, so there is nothing to change. Adding it again from the vast.gg address fails while the first one is registered.

## Upgrading from vast-giveaways
In Claude Code, run `/plugin install agentic-giveaway@vast` once, then `/plugin configure agentic-giveaway@vast` to enter your key.

## Install in Codex
Codex reads the key from the `VAST_API_KEY` environment variable. Load your key into that variable through your secret manager and export it first; keep the key itself out of shell commands and history.
Run this first on its own, then paste your key when it asks.
```bash
read -rs VAST_API_KEY && export VAST_API_KEY   # paste the key when prompted; it stays out of your shell history
```

```bash
codex plugin marketplace add https://vast.gg/plugins/agentic-giveaway.git
codex plugin add agentic-giveaway@vast
```
This adds the skill and the `vast` MCP server. To add only the MCP server:
```bash
codex mcp add vast --url https://general.vast.gg/mcp --bearer-token-env-var VAST_API_KEY
```

## Install with skills.sh (Vercel)
```bash
npx skills add https://vast.gg
```
The skills CLI sends telemetry by default; set `DISABLE_TELEMETRY=1` to opt out (for example, `DISABLE_TELEMETRY=1 npx skills add https://vast.gg`).

It asks which agents to install the skill for (Claude Code, Codex, Cursor and others). This installs the skill only, so also connect the MCP server as shown below. The skill does nothing without the `vast` MCP server at `https://general.vast.gg/mcp`, using the header `Authorization: Bearer <key>`. To skip the questions: `npx skills add https://vast.gg --skill agentic-giveaway -a codex -y`.

## Connect the MCP server manually
- URL: `https://general.vast.gg/mcp` (Streamable HTTP)
- Header on every request: `Authorization: Bearer <your key>`

Claude Code, without the plugin: load your key into `VAST_API_KEY` through your secret manager and export it first.
Run this first on its own, then paste your key when it asks.
```bash
read -rs VAST_API_KEY && export VAST_API_KEY   # paste the key when prompted; it stays out of your shell history
```

```bash
claude mcp add --transport http vast https://general.vast.gg/mcp --header "Authorization: Bearer $VAST_API_KEY"
```

Codex, by hand in `~/.codex/config.toml`:
```toml
[mcp_servers.vast]
url = "https://general.vast.gg/mcp"
bearer_token_env_var = "VAST_API_KEY"
```

claude.ai (web, Desktop and mobile): open Customize, then Connectors, and add a custom connector with the URL above. Choose **No sign-in**. Under Request headers, add `Authorization` with the value `Bearer <your key>`. Request headers are a beta available to a limited set of organizations; if the field is missing, your organization does not have access yet. On Team/Enterprise, an Owner adds the connector in Organization settings → Connectors; the header holds one key shared by the whole organization. See [Claude’s custom connector docs](https://claude.com/docs/connectors/custom/add-unlisted.md).

Other MCP clients take the same URL and header; check your client's docs for where its config lives.

## Try it
> "Run a one-week giveaway for a $100 gift card. To enter, people follow @yourbrand on X and join our Discord. Three winners."

The agent drafts the giveaway, shows you a summary and the link, and publishes only when you say so. It finishes with a short action plan: anything only you can do, such as paying a launch fee or finishing a step in the web editor.

## What it connects to
- The MCP server sends your API key and the giveaway details you give the agent to `https://general.vast.gg/mcp`.
- To add a cover image, the skill may upload an image you choose to `https://general.vast.gg/storage/upload`. Uploads accept PNG, JPEG, WebP or GIF (checked by magic bytes), at most 5 MB. Free keys must send `Content-Length`.
- The plugin has no hooks or scripts. The skills CLI sends telemetry by default unless you set `DISABLE_TELEMETRY=1`.

## Safety
- Giveaways stay drafts until you explicitly ask to publish.
- Keys are scoped and can be revoked at any time at https://vast.gg/settings/developer.
- Privacy policy: https://vast.gg/privacy-policy
- Help: https://vast.gg/help/use-vast-with-ai-agents

MIT licensed.
