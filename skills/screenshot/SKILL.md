---
name: screenshot
description: Takes a full-page screenshot of any URL using Apify and displays it visually. Use when the user wants to see a website, review a design visually, or capture a webpage. Trigger on /screenshot <url> or when user says "скриншот", "сфотай сайт", "покажи сайт визуально".
user-invocable: true
argument-hint: "<url>"
---

# Screenshot Skill

Take a full-page screenshot of a URL, display it inline, and give design feedback.

## Requirements

- Apify MCP connected (`https://mcp.apify.com`)

## Steps

1. Extract the URL from args or ask for it if missing.

2. Call Apify actor `leadsbrary/screenshot-html-file-from-url` via `mcp__claude_ai_claude_mcp_apify__call-actor`:
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

3. Get dataset items via `mcp__claude_ai_claude_mcp_apify__get-dataset-items` using `datasetId` from the run result. Extract `screenshotUrl`.

4. Download screenshot — use exact timestamp to avoid collisions:
```bash
TS=$(date +%s) && curl -s "<screenshotUrl>" -o /tmp/screenshot_$TS.png && echo "/tmp/screenshot_$TS.png"
```
Save the returned path.

5. Display the image inline using the Read tool on the saved path.

6. After the image, output this block exactly (fill in the path and domain):
```
📁 **Файл:** `/tmp/screenshot_<TS>.png`
🖥️ **Открыть:** `open /tmp/screenshot_<TS>.png`
```

7. Give design feedback in the user's language. Cover:
   - **First impression** — overall feel, style, target audience
   - **Layout** — section structure, hierarchy, proportions
   - **Colors** — palette, contrast, accents
   - **Typography** — fonts, sizes, readability
   - **Weak points** — what hurts, what gets lost, what feels off
   - **Top 3 improvements** — concrete, prioritized

   Keep feedback caveman-compressed if caveman mode is active.
