# WonderWorks AI plugin

Connects Claude to the [WonderWorks AI](https://studio.wonderworksai.com) MCP
server for generating images, video, music, sound effects and storyboards, and
adds a skill that teaches Claude how to use those tools well.

## Install

This repository is also its own plugin marketplace:

```
/plugin marketplace add WonderWorksAI/wonderworksai-plugin
/plugin install wonderworks@wonderworksai
```

In Cowork, go to Customize → Plugins → Add marketplace and paste the repository
URL. On first use Claude asks you to sign in to WonderWorks — the plugin stores
no credentials.

## What it adds

**MCP server** — `wonderworks`, a remote HTTP server at
`https://mcp.wonderworksai.com` (OAuth), exposing:

| Tool | Purpose |
|---|---|
| `generate-image` | Still image from a prompt, optionally guided by up to 4 reference images |
| `generate-video` | 5s or 10s clip from a prompt, optionally animating a source image |
| `generate-storyboard` | A continuous sequence of shots with consistent characters, locations and style |
| `generate-music` | Music track, 30–360 seconds |
| `generate-sound` | Sound effect, Foley or ambience, 1–30 seconds |
| `get-asset` | Status and URLs for an existing asset |
| `render-asset-widget` | Display an existing asset in the media player |

**Skill** — `wonderworks-media`: tool selection, prompt structure per medium,
chaining assets by id (image → video first frame, storyboard → shots), and the
rule that generate tools already render their own player.

## Endpoint

The server URL lives in `.mcp.json`. If your deployment mounts the transport on
a path, change the `url` there (for example to
`https://mcp.wonderworksai.com/mcp`).

## Layout

```
.claude-plugin/plugin.json    manifest
.mcp.json                     MCP server definition
skills/wonderworks-media/     usage skill
```
