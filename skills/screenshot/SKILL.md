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

7. Give design feedback — cover all of these in order:
   - **Первое впечатление** — общее ощущение, стиль, аудитория
   - **Layout** — структура секций, иерархия, пропорции
   - **Цвета** — палитра, контрасты, акценты
   - **Типографика** — шрифты, размеры, читаемость
   - **Слабые места** — что мешает, что теряется, что режет глаз
   - **Топ-3 улучшения** — конкретные, приоритизированные

   Keep feedback caveman-compressed if caveman mode is active.
