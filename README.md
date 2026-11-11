# Vast giveaways for AI agents

Let your AI agent run giveaways on [Vast](https://vast.gg). It can draft a giveaway, add entry actions such as following on X or joining Discord, preview it, publish it when you say so, and report entries and winners.

This repo works in Claude Code, Codex and any agent that reads [Agent Skills](https://agentskills.io):
- **`skills/agentic-giveaway/SKILL.md`**: teaches the agent how to run a giveaway and what to hand back to you.
- **`.mcp.json`** (Claude Code) and **`mcp.json`** (Codex): connect the agent to Vast's MCP server at `https://general.vast.gg/mcp`.
- **`.claude-plugin/`** and **`.codex-plugin/`**: the plugin manifests and the `vast` marketplace.
- **`server.json`**: the listing for the [MCP Registry](https://registry.modelcontextprotocol.io).

## Before you start
1. Get a Vast account with a paid plan: https://vast.gg/upgrade
2. Create an API key: https://vast.gg/settings/developer
3. Keep the key out of chat. Each install below says where it goes.

## Install in Claude Code
```bash
claude plugin marketplace add Firstbloodio/agentic-giveaway
claude plugin install agentic-giveaway@vast
```
Then add your key: in Claude Code, run `/plugin configure agentic-giveaway@vast` and paste it into the "Vast API key" field. If you install from `/plugin` inside Claude Code, it asks for the key during install. Claude Code keeps the key in secure storage (the keychain on a Mac), not in a settings file.

## Install in Codex
Codex reads the key from the `VAST_API_KEY` environment variable.
```bash
export VAST_API_KEY="<your key>"
codex plugin marketplace add Firstbloodio/agentic-giveaway
codex plugin add agentic-giveaway@vast
```
This adds the skill and the `vast` MCP server. To add only the MCP server:
```bash
codex mcp add vast --url https://general.vast.gg/mcp --bearer-token-env-var VAST_API_KEY
```

## Install with skills.sh (Vercel)
```bash
npx skills add Firstbloodio/agentic-giveaway
```
It asks which agents to install the skill for (Claude Code, Codex, Cursor and others). This installs the skill only, so also connect the MCP server as shown below. To skip the questions: `npx skills add Firstbloodio/agentic-giveaway --skill agentic-giveaway -a codex -y`.

## Connect the MCP server manually
- URL: `https://general.vast.gg/mcp` (Streamable HTTP)
- Header on every request: `Authorization: Bearer <your key>`

Claude Code, without the plugin:
```bash
claude mcp add --transport http vast https://general.vast.gg/mcp --header "Authorization: Bearer <your key>"
```

Codex, by hand in `~/.codex/config.toml`:
```toml
[mcp_servers.vast]
url = "https://general.vast.gg/mcp"
bearer_token_env_var = "VAST_API_KEY"
```

claude.ai (web, Desktop and mobile): open Customize, then Connectors, and add a custom connector with the URL above. Under Request headers, add `Authorization` with the value `Bearer <your key>`.

Other MCP clients take the same URL and header; check your client's docs for where its config lives.

## Try it
> "Run a one-week giveaway for a $100 gift card. To enter, people follow @yourbrand on X and join our Discord. Three winners."

The agent drafts the giveaway, shows you a summary and the link, and publishes only when you say so. It finishes with a short action plan: anything only you can do, such as paying a launch fee or finishing a step in the web editor.

## What it connects to
- The MCP server sends your API key and the giveaway details you give the agent to `https://general.vast.gg/mcp`.
- To add a cover image, the skill may upload an image you choose to `https://general.vast.gg/storage/upload`.
- Nothing else is sent anywhere. The plugin has no hooks or scripts.

## Safety
- Giveaways stay drafts until you explicitly ask to publish.
- Keys are scoped and can be revoked at any time at https://vast.gg/settings/developer.
- Privacy policy: https://vast.gg/privacy-policy
- Help: https://vast.gg/help/use-vast-with-ai-agents

MIT licensed.
