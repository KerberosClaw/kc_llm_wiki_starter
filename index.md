# {{ your-wiki-name }} index

按類別分組的 wiki 主題頁索引。**每次 ingest / 新頁 / 改 frontmatter，agent 要更新此檔**（[SCHEMA.md](SCHEMA.md) §Ingest workflow 第 5 步）。

詳細 sources 見各檔 frontmatter。

---

<!-- 
自訂你的分類區塊。建議分類（依 repo 性質調整）：
- Infrastructure & Architecture — 架構 / 拓撲 / 部署
- Operations — runbook / security / SRE
- Application Layer — API / data model / pipeline
- Product & Team — roadmap / 組織觀察
- Tooling & Meta — 工具 / 速查 / onboarding
-->

## Examples（砍此段，真實 wiki 用）

| 檔 | Type | Last updated | 一句話 |
|---|---|---|---|
| [_example_overview.md](wiki/_example_overview.md) | overview | 2026-04-23 | Overview 型主題頁範例（架構 / 拓撲 / roadmap） |
| [_example_entity.md](wiki/_example_entity.md) | entity | 2026-04-23 | Entity 型主題頁範例（單一實體 / person / schema） |

---

## Raw 層

原檔在 [raw/](raw/)（gitignored）。建議按 source 分子目錄（見 [raw/README.md](raw/README.md)）。

## Journal 層（時序，非 Karpathy 原模型）

工作日誌 / meeting / checklist 在 [journal/](journal/)。與 wiki 層分工：

- `wiki/` = 主題頁（跨時間沉澱）
- `journal/` = 時序日誌（每日）
- `log.md` = knowledge 操作 ledger（INGEST / UPDATE / LINT / DECISION）
