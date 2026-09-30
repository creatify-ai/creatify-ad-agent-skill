# Creatify Ad Agent Skill

Connects Codex or Claude Code to Creatify's Ad Agent MCP. The agent directs it: MiniMax-H3 footage, keyframes, music and voice lines are generated into a persistent project, the agent judges every take from contact sheets, assembles one HTML stage, and ships a gated MP4.

## Install (humans)

Paste this into a new conversation in the ChatGPT/Codex desktop app or Claude Code:

```text
Read github.com/creatify-ai/creatify-ad-agent-skill to install the Creatify Ad Agent plugin and make my first video ad.
```

The agent reads the guide below and does the rest, including sign-in.

---

## Agent install guide

If you are an agent reading this for a user: read the whole guide, then complete every step in order. Do not stop after install or login; the final step is starting the user's first video.

### 0. Host environment gate

If you are running in a web app or an isolated remote workspace, stop: you cannot modify the desktop app's local plugin configuration. Tell the user to open the desktop app (or a local terminal for Claude Code), start a new conversation there, and paste the install prompt above.

### 1. Install the plugin

**Codex.** Use the Codex CLI bundled with the desktop app, not one from npm, Homebrew or a random `codex` on `PATH`:

```text
"<BUNDLED_CODEX>" plugin marketplace add https://github.com/creatify-ai/creatify-ad-agent-skill.git --ref main
"<BUNDLED_CODEX>" plugin add creatify-video-composer@creatify-video-composer
```

If no usable `git` is available, find the Git binary the machine already has and prepend its directory to `PATH`, then rerun.

**Claude Code:**

```text
/plugin marketplace add creatify-ai/creatify-ad-agent-skill
/plugin install creatify-video-composer@creatify-video-composer
```

### 2. Authenticate

- Codex: `codex mcp login creatify-composer`
- Claude Code: `/mcp`, select `creatify-composer`, then Authenticate

Both open the Creatify OAuth page in the browser. No tokens are stored in this repository.

### 3. Verify

- The `creatify-composer` MCP server is registered and logged in (`codex mcp list`, or `/mcp` in Claude Code).
- `composer_*` tools are visible in a NEW conversation. The install thread has already captured its tool list, so don't test there.

### 4. Required final step: start the user's first video

Don't end at "installed". In the new conversation, ask the user what they want to promote and whether they have reference footage or images, then:

- call `composer_billing_state` to check credits and whether takes are watermarked;
- create a project with `composer_project_create` and import their references with `composer_import_asset`;
- follow the `video-composer` skill from there.

## What is included

- `video-composer/`: the plugin package
  - `.codex-plugin/plugin.json` + `codex.mcp.json`: Codex metadata and connector
  - `.claude-plugin/plugin.json` + `.mcp.json`: Claude Code metadata and connector
  - `skills/video-composer/`: the workflow skill and its references
- `.agents/plugins/marketplace.json`: Codex marketplace
- `.claude-plugin/marketplace.json`: Claude Code marketplace

The hosted endpoint is `https://api.creatify.ai/ad_agent/mcp` (OAuth 2.1 + PKCE, handled by the host).

Claude Desktop / Cowork: add `https://api.creatify.ai/ad_agent/mcp` as a custom connector, then upload `video-composer/skills/video-composer` as a skill.

## Requirements

- A Creatify account.
- Codex with plugin support, or Claude Code.
