---
name: news-weekly-report
description: Run or continue Hannah's News All in One weekly global news intelligence report workflow. Use when the user asks to make the Friday 15:00 News All in One report, continue or complete the weekly news report flow, generate/review/render/deliver a global news intelligence report, fix News All in One formatting or delivery, update the Friday automation, or ensure Zotero, Google Drive, and Discord delivery for the news report.
---

# News Weekly Report

Use this skill for Hannah's recurring News All in One global news intelligence workflow.

## Start Here

1. Work in `/Users/hannah/Documents/News ALL in One`.
2. Read these files before acting:
   - `PROJECT_MEMORY.md`
   - `README.md`
   - `docs/WORKFLOW.md`
   - `docs/REPORT_FORMAT_REQUIREMENTS.md`
3. Trust the newest `PROJECT_MEMORY.md` state over older notes.
4. Check current state before heavy work:
   - `git status --short`
   - latest `data/service_logs/*.json` if present
   - latest Markdown report under `reports/markdown/`
   - latest Word/PDF under `reports/docx/` and `reports/pdf/`
5. Follow the full workflow in `references/workflow.md`.

## Hard Rules

- Fixed schedule: every Friday 15:00 Asia/Shanghai.
- Normal coverage window: previous Friday 15:00 inclusive to current Friday 15:00 exclusive. Always print exact absolute timestamps.
- Do not download Spotify transcripts. This project intentionally removed all episode/transcript/mark-seen steps.
- Final filename stem must be `YYMMDD-News全球新闻情报研报`.
- Visible H1 title must be a constructive thesis-style bilingual sentence that immediately communicates the issue's key conclusion, not a generic topic stack.
- Top 40 tables must use the full available body width, with the source column kept compact and smaller while analysis columns receive most of the width.
- Top 40 tier headings must not sit alone near the bottom quarter of a page. If a tier heading would land too low before its table, start that tier on a new page with the table.
- Word goes to Google Drive folder `2.国际科技情报`.
- PDF goes to Zotero collection `2.国际科技情报` with `/unread` and `/<YYMMDD>` tags.
- PDF goes to Discord `todo`, channel ID `1508163671988109393`, through Discord Studio at `/Users/hannah/.discord-studio/Discord_Studio`.
- The run is not complete until content review, format audit, visual inspection, Zotero, Google Drive, and Discord all succeed or the user explicitly skips a blocked channel.
- When using Gemini, never invent facts, dates, citations, source URLs, companies, products, or future events. Rumors must be labeled as unconfirmed.
- Respect Codex approval prompts for external uploads and local Zotero DB writes. Do not work around rejected approvals.

## Common User Triggers

- “继续 News All in One 项目”
- “每周五下午3点做新闻研报”
- “做本周 News 全球新闻情报研报”
- “完成 News 研报交付”
- “修 News All in One 的格式/分页/表格”
- “把 News 研报上传 Zotero / Google Drive / Discord”
