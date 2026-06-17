# screenshot-skill

A Claude Code skill that takes full-page screenshots of any URL and displays them visually — powered by [Apify](https://apify.com).

## What it does

Type `/screenshot https://example.com` and Claude will:
1. Capture a full-page 1440px screenshot via Apify
2. Display it inline in your conversation
3. Give a brief visual analysis of the design

## Requirements

- **Apify account** — free tier works. Get API key at [apify.com](https://apify.com)
- **Apify MCP** connected in Claude.ai → Settings → Integrations → add `https://mcp.apify.com`

## Install

```bash
npx skills add your-github-username/screenshot-skill
```

Or manually copy `skills/screenshot/SKILL.md` to `~/.claude/skills/screenshot/SKILL.md`.

## Usage

```
/screenshot https://pology.vercel.app/
```

Or natural language: "сфотай сайт", "покажи дизайн этого сайта", "take a screenshot of..."

## Cost

Uses Apify actor `leadsbrary/screenshot-html-file-from-url` — ~$0.002 per screenshot on free tier.
