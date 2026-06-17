---
name: screenshot
description: Takes a full-page screenshot of any URL using Apify and displays it visually. Use when the user wants to see a website, review a design visually, or capture a webpage. Trigger on /screenshot <url> or when user says "скриншот", "сфотай сайт", "покажи сайт визуально".
user-invocable: true
argument-hint: "<url>"
---

# Screenshot Skill

Take a full-page screenshot of a URL and display it visually.

## Requirements

- Apify MCP connected (`https://mcp.apify.com`) — needs `mcp__claude_ai_claude_mcp_apify__call-actor` and `mcp__claude_ai_claude_mcp_apify__get-dataset-items` tools available.
- Get a free Apify API key at https://apify.com and connect via Claude.ai → Settings → Integrations.

## Steps

1. Extract the URL from args or ask for it if missing.

2. Call Apify actor `leadsbrary/screenshot-html-file-from-url` via MCP tool `mcp__claude_ai_claude_mcp_apify__call-actor` with:
```json
{
  "urls": [{"url": "<URL>"}],
  "format": "png",
  "fullPage": true,
  "viewportWidth": 1440,
  "waitUntil": "networkidle",
  "scrollToBottom": true,
  "delayAfterScrollMs": 2000
}
```
Set `waitSecs: 45`.

3. Get dataset items via `mcp__claude_ai_claude_mcp_apify__get-dataset-items` using the `datasetId` from the run result. Extract `screenshotUrl`.

4. Download screenshot locally:
```bash
curl -s "<screenshotUrl>" -o /tmp/screenshot_$(date +%s).png && echo /tmp/screenshot_$(date +%s).png
```

5. Read and display the image using the Read tool on the downloaded `/tmp/screenshot_*.png` path.

6. After displaying: brief visual analysis — layout, colors, typography, key sections, overall quality. Keep concise.
