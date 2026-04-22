# {{ your-wiki-name }} log

Append-only ledger，最新在下。詳規則見 [SCHEMA.md](SCHEMA.md)。

格式：`YYYY-MM-DD HH:MM  <TYPE>  <摘要>`

動作類型：

- `INGEST`   — 新 raw 進來 + 觸發哪些主題頁更新
- `UPDATE`   — 修改既有主題頁（非 ingest 觸發）
- `LINT`     — run lint skill 結果摘要（即使全通過也 append，作為「已跑過」憑證）
- `DECISION` — 重大 knowledge-related 決策

---

## Entries

<!--
首次 clone 後記一筆 DECISION 作為 anchor，例如：

- 2026-04-23  DECISION  Clone kc_llm_wiki_starter template，採 Karpathy LLM Wiki 三層 pattern
-->
