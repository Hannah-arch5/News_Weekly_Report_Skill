---
name: news-weekly-report
description: Run or continue Hannah's News All in One weekly global news intelligence report workflow. Use when the user asks to make the Friday 15:00 News All in One report, audit or backfill missing weekly issues, continue or complete the weekly news report flow, generate/review/render/deliver a global news intelligence report, fix News All in One formatting or delivery, update the Friday automation, or ensure Zotero, Google Drive, and Discord delivery for the news report.
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
   - run `python3 scripts/audit_report_schedule.py` before choosing a report window
   - `git status --short`
   - latest `data/service_logs/*.json` if present
   - latest Markdown report under `reports/markdown/`
   - latest Word/PDF under `reports/docx/` and `reports/pdf/`
5. Follow the full workflow in `references/workflow.md`.

## Hard Rules

- Fixed schedule: every Friday 15:00 Asia/Shanghai.
- Before every generation or continuation, audit all expected Friday cutoffs with `scripts/audit_report_schedule.py`. If historical issues are missing, backfill them in chronological order and complete all review and delivery gates for each issue before generating the current/latest issue. A request for the latest report never permits skipping older missing Fridays.
- Normal coverage window: previous Friday 15:00 inclusive to current Friday 15:00 exclusive. Always print exact absolute timestamps.
- Do not download Spotify transcripts. This project intentionally removed all episode/transcript/mark-seen steps.
- Final filename stem must be `YYMMDD-News全球新闻情报研报`.
- Visible H1 title must be a constructive thesis-style bilingual sentence that immediately communicates the issue's key conclusion, not a generic topic stack.
- Top 40 tables must use the full available body width, with the source column kept compact and smaller while analysis columns receive most of the width.
- Top 40 `核心事件` titles must be Chinese-first. Original English headlines may appear in parentheses after the Chinese title, but English-only event titles are not acceptable.
- Top 40 analysis cells must be deep enough for decision support, not one-line news digests. Each row must include factual context, the prior market assumption and repricing mechanism, signal/transmission-chain value, concrete monitoring indicators or actions, and business implications for A-share, U.S. equity, AI, supply-chain, sector, or geopolitical decisions where relevant. After removing Markdown and spacing, require at least 170 visible characters in each analysis column and at least 360 across both columns; treat a failure as a blocking content error. Allow the PDF to become longer if needed.
- Source cells must display compact clickable source names only. Never print full raw URLs in the table; keep them hidden in hyperlink targets.
- Final Word/PDF output must not have detached punctuation at the start of a visual line. Chinese and English punctuation should stay attached to the preceding text.
- Top 40 tier headings must not sit alone near the bottom quarter of a page. If a tier heading would land too low before its table, start that tier on a new page with the table.
- A tier heading plus only the repeated table header row is still visually orphaned. Visual inspection must confirm at least the first substantive data row appears under a tier heading; otherwise insert a page break before that tier and rerender.
- Before adding a hard page break to fix an orphan or large gap, normalize avoidable content density first: keep the Chinese executive summary near 700 visible Chinese characters and trim unusually verbose table rows only when they remain above the permanent depth gate. Part Two should flow onto the current page when the summary can be brought to standard length and rank 1 can appear with its heading. Use a hard page break only when the heading/header orphan remains after this density check.
- Never force a tier or major section onto a new page merely because the preceding table ended. Use only the conditional bottom-quarter rule confirmed by visual inspection.
- Word goes to Google Drive folder `2.国际科技情报`.
- PDF goes to Zotero collection `2.国际科技情报` with `/unread` and `/<YYMM>` tags. Keep filenames as `YYMMDD`, but Zotero date tags only go to month granularity.
- PDF goes to Discord `todo`, channel ID `1508163671988109393`, through Discord Studio at `/Users/hannah/.discord-studio/Discord_Studio`.
- The run is not complete until content review, format audit, visual inspection, Zotero, Google Drive, and Discord all succeed or the user explicitly skips a blocked channel.
- When using Gemini, never invent facts, dates, citations, source URLs, companies, products, or future events. Rumors must be labeled as unconfirmed.
- Gemini must follow the candidate-first verified-source workflow: generate candidate evidence rows, audit source URLs, then draft the final report only from a verified evidence table.
- If Gemini API quota is exhausted and Hannah explicitly authorizes using Codex/OpenAI capacity, Codex may draft from the verified source-candidate pool directly. This does not relax any delivery gate: exactly 40 ranked rows, 40 openable direct source URLs, no duplicate URLs, content review, source audit, visual inspection, and delivery verification still apply.
- Do not ask Gemini to write the final report until at least 40 verified candidate rows are available.
- For focused evidence top-ups, remove every already-used URL from the candidate table before sending it to Gemini. A prose exclusion list alone is not sufficient because smaller models may repeat the original rows.
- Use Gemini SSE streaming for the long final report and long repair batches so a valid generation is not lost to an idle long-response connection reset.
- Normalize mechanical table drift before regeneration. If Gemini merges the two analysis columns while preserving the source, split at the `情报价值：` label and restore the five-column schema.
- If only the analysis-depth gate fails, repair the failing rows only. Keep event titles, rankings, facts and source URLs locked. If only the fourth column is short, append event-specific monitoring indicators and actions there; if both analysis columns are short, append focused repricing/boundary analysis to the third column and monitoring/action guidance to the fourth. Rerun the complete content review after every pass and do not regenerate compliant rows.
- Reject simulated or hypothetical Gemini output. If candidate batches are insufficient, generate focused additional batches rather than letting Gemini fill gaps.
- Open all 40 source URLs before rendering or delivery. Replace inaccessible links, and remove any event that cannot be independently re-verified.
- Final report sources must never contain `vertexaisearch`, Google Search pages, Google grounding redirects, guessed URLs, homepage-only citations, placeholders, or 404 links.
- Respect Codex approval prompts for external uploads and local Zotero DB writes. Do not work around rejected approvals.

## Common User Triggers

- “继续 News All in One 项目”
- “每周五下午3点做新闻研报”
- “做本周 News 全球新闻情报研报”
- “完成 News 研报交付”
- “修 News All in One 的格式/分页/表格”
- “把 News 研报上传 Zotero / Google Drive / Discord”
