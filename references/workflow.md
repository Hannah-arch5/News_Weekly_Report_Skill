# News All in One Workflow

## 1. Orient

- Work in `/Users/hannah/Documents/News ALL in One`.
- Read `PROJECT_MEMORY.md` first, then `README.md`, `docs/WORKFLOW.md`, and `docs/REPORT_FORMAT_REQUIREMENTS.md`.
- Check Git state before edits. Never revert user changes.
- If continuing a run, prefer the latest existing report artifacts instead of regenerating unless the user asks for a fresh run.

## 2. Generate Or Continue

For a scheduled Friday run, use:

```bash
python3 scripts/run_scheduled_report_service.py
```

For a manual staged run:

```bash
python3 scripts/build_weekly_prompt.py
python3 scripts/build_grounded_research_prompts.py
python3 scripts/generate_gemini_report.py --prompt data/gemini_inputs/<run_id>-research-macro/gemini-prompt.md --allow-risk-phrases
python3 scripts/generate_gemini_report.py --prompt data/gemini_inputs/<run_id>-research-ai-tech/gemini-prompt.md --allow-risk-phrases
python3 scripts/generate_gemini_report.py --prompt data/gemini_inputs/<run_id>-research-china/gemini-prompt.md --allow-risk-phrases
python3 scripts/generate_gemini_report.py --prompt data/gemini_inputs/<run_id>-research-industry/gemini-prompt.md --allow-risk-phrases
python3 scripts/collect_verified_evidence.py reports/markdown/<run_id>-research-*-gemini-report.md --output data/verified_evidence/<run_id>.md
python3 scripts/build_verified_rows_prompt.py --window-prompt data/gemini_inputs/<run_id>/gemini-prompt.md --output data/gemini_inputs/<run_id>-verified-topup1/gemini-prompt.md --exclude data/verified_evidence/<run_id>.md
python3 scripts/build_verified_final_prompt.py --prompt data/gemini_inputs/<run_id>/gemini-prompt.md --evidence data/verified_evidence/<run_id>.md
python3 scripts/generate_gemini_report.py --prompt data/gemini_inputs/<run_id>-verified-final/gemini-prompt.md
python3 scripts/check_gemini_report.py reports/markdown/<run_id>-verified-final-gemini-report.md
python3 scripts/check_report_source_links.py reports/markdown/<run_id>-verified-final-gemini-report.md
python3 scripts/render_delivery_reports.py reports/markdown/<run_id>-verified-final-gemini-report.md
python3 scripts/audit_delivery_report_format.py --docx reports/docx/<YYMMDD-News全球新闻情报研报.docx> --pdf reports/pdf/<YYMMDD-News全球新闻情报研报.pdf>
```

If Gemini Pro quota fails, note it plainly and use the project fallback only if already configured or approved by the user.

## 3. Content Contract

Gemini must produce a Chinese global news intelligence report based on real retrieval:

- Sources: X/Twitter, Yahoo Finance, Seeking Alpha, Spotify, 东方财富网快讯, 同花顺实时新闻, 财联社电报.
- Top 40 events only.
- Deduplicate same-event coverage.
- Prefer official releases, regulators, primary sources, authoritative media, and high-quality research.
- Reject any report containing simulated, hypothetical, knowledge-cutoff, or refusal-to-retrieve language.
- Use the candidate-first verified-source workflow. Gemini first produces candidate evidence rows; Codex audits source URLs; Gemini drafts the final report only from the verified evidence table.
- Do not let Gemini write the final long-form report until at least 40 verified candidate rows exist.
- Open every Top 40 source URL over the network before rendering or delivery. Replace inaccessible links, and remove any event that cannot be independently re-verified.
- Final report sources must never contain `vertexaisearch`, Google Search pages, Google grounding redirects, guessed URLs, homepage-only citations, placeholders, or 404 links.
- Exclude routine small stock moves, empty daily recaps, and pure technical indicator alerts.
- Required parts:
  - `第一部分：核心综述与摘要 (Executive Summary)`
  - `第二部分：Top 40 核心资讯精选 (Top 40 Strategic Intelligence Matrix)`
  - `第三部分：深度情报拆解 (Deep Intelligence Analysis)`
  - `第四部分：结论与策略 (Conclusion & Strategy)`
- Top 40 tier ranges:
  - 1-10: `第一梯队：战略级核心情报`
  - 11-25: `第二梯队：产业与政策深度`
  - 26-40: `第三梯队：微观信号与前兆`

One-shot final report generation is no longer the preferred production path. Split retrieval into evidence batches (macro, AI/technology, China, and industry), audit the candidate URLs, then build the final prompt from the verified evidence table. Never invent or guess replacement URLs.

## 4. Formatting Contract

- Final Word/PDF follow Spotify All in One delivery style unless News memory overrides it.
- Use Microsoft Word for final PDF export; do not silently use a lower-fidelity fallback.
- H1 title should not look like it has a blank line inside the wrap.
- H1 title must be a constructive thesis-style bilingual sentence that lets the reader understand the issue's main conclusion from the title alone, not a generic topic stack.
- Major sections flow naturally when the page still has enough space.
- Only start a new page when a heading or short structural label would land in the bottom quarter of a page with only a tiny amount of following body text.
- Do not force Part Two, Part Three, or Part Four onto new pages unless the bottom-quarter rule requires it.
- Separate major parts with one blank line, a horizontal separator, and one blank line.
- Source column in Top 40 tables is secondary and uses small text. Show only clickable source names; keep raw URLs hidden in hyperlink targets.
- Top 40 tables should use the full available body width, visually aligning with the surrounding text edges when possible.
- Top 40 tier headings must not sit alone near the bottom quarter of a page. If a tier heading would land too low before its table, start that tier on a new page with the table.
- Use clear gray-blue borders for tables.
- Split table analysis cells into separate blocks:
  - `核心摘要：` then blank line then `深度预期差分析：`
  - `情报价值：` then blank line then `业务影响建议：`
- Do not bold ordinary keywords. Bold only headings, table headers, tier headings, and short structural labels.

## 5. Delivery

After review and audit pass:

1. Stage Word and PDF under `reports/archive/pending/<YYMM>/`.
2. Archive PDF to Zotero collection `2.国际科技情报` with `/unread` and `/<YYMMDD>` tags.
3. Upload Word to Google Drive folder `2.国际科技情报`.
4. Send PDF to Discord `todo` channel ID `1508163671988109393`.
5. Verify each destination:
   - Zotero item is in `2.国际科技情报` and has both tags.
   - Google Drive folder `2.国际科技情报` contains the Word file.
   - Discord Studio notifications log shows `notification_sent`.
6. Update `PROJECT_MEMORY.md` with exact delivery status and any blockers.
7. Commit code, docs, and memory changes to Git. Keep `.env`, `data/`, and `reports/` ignored.

## 6. Common Commands

Use project runtime Python when available:

```bash
/Users/hannah/.cache/codex-runtimes/codex-primary-runtime/dependencies/python/bin/python3 -m unittest discover -s tests -v
```

Use rclone for Drive delivery:

```bash
rclone copy reports/docx/<file>.docx "gdrive:2.国际科技情报/" --checksum
rclone lsf "gdrive:2.国际科技情报/" --files-only
```

Use Discord Studio for Discord delivery:

```bash
npm run send:discord -- 1508163671988109393 "News All in One 全球新闻情报研报：<stem>" "<pdf>"
```

Use Zotero helper:

```bash
python3 scripts/archive_pdf_to_zotero.py "<pdf>" --collection "2.国际科技情报" --tag /unread --tag /<YYMMDD> --replace-existing --backup --create-collection
```
