# Vast giveaways for AI agents

Let your AI agent run giveaways on [Vast](https://vast.gg). It can draft a giveaway, add entry actions such as following on X or joining Discord, preview it, publish it when you say so, and report entries and winners.

This repo contains:
- **`skills/agentic-giveaway/SKILL.md`**: teaches the agent how to run a giveaway and what to hand back to you.
- **`.mcp.json`**: connects the agent to Vast's MCP server at `https://general.vast.gg/mcp`.

## Before you start
1. Get a Vast account with a paid plan: https://vast.gg/upgrade
2. Create an API key: https://vast.gg/settings/developer
3. Put the key in your environment, never in chat:
   ```bash
   export VAST_API_KEY="vast_sk_…"
   ```

## Install

**Claude Code**
```bash
claude plugin marketplace add Firstbloodio/agentic-giveaway
claude plugin install agentic-giveaway@vast
```

**Claude Code, Cursor, Codex and other agents (the skill only)**
```bash
npx skills add Firstbloodio/agentic-giveaway
```
Then connect the MCP server yourself, for example in Claude Code:
```bash
claude mcp add --transport http vast https://general.vast.gg/mcp --header "Authorization: Bearer $VAST_API_KEY"
```

## Try it
> "Run a one-week giveaway for a $100 gift card. To enter, people follow @yourbrand on X and join our Discord. Three winners."

The agent drafts the giveaway, shows you a summary and the link, and publishes only when you say so. It finishes with a short action plan: anything only you can do, such as paying a launch fee or finishing a step in the web editor.

## Safety
- Giveaways stay drafts until you explicitly ask to publish.
- Keys are scoped and can be revoked at any time at https://vast.gg/settings/developer.
- Help: https://vast.gg/help/use-vast-with-ai-agents

MIT licensed.
