# {{ your-wiki-name }} SCHEMA

Repo 的 config / 慣例 / workflow。採 [Karpathy LLM Wiki 三層 pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)（2026-04 gist）+ journal 本地 extension。

Agent 動 `wiki/` 或 `raw/` **前先讀此檔**。目錄 / 邊界紅線見 [CLAUDE.md](CLAUDE.md)，本檔不重複。

## 三層架構

| 層 | 實體 | 進 git | 職責 |
|----|------|-------|------|
| Raw | `raw/**` + `*.local.md` | No（gitignored） | 外部 immutable copy，原樣保留，LLM 讀不寫 |
| Wiki | `wiki/<topic>.md`（含 frontmatter） | Yes | LLM 讀 raw 後整理的主題頁，cross-ref 彼此 + 回指 raw |
| Schema | `SCHEMA.md` + `log.md` + `index.md` | Yes | 結構宣告 + ledger + 內容索引 |

**本地 extension（非 Karpathy 原模型）**：

| 層 | 實體 | 進 git | 職責 |
|----|------|-------|------|
| Journal | `journal/<YYYYMMDD>_dayN.md` / meeting / checklist | Yes | 時序工作日誌（個人 context，非 knowledge base） |

## 主題頁 template（`wiki/*.md`）

每頁結構：

```markdown
---
title: <主題>
type: overview | entity | concept | comparison | synthesis
last_updated: YYYY-MM-DD
sources:
  - <來源 1>
  - <來源 2>
---

# <主題>

**TLDR**: 一句話摘要（agent query 時 loadable）

**來源**：哪幾個 raw 檔貢獻（與 frontmatter sources 對應）

## <按需 sections>

- Overview / Entities / Relationships / Cross-refs / Open questions
```

### Type 分類（Karpathy）

- **overview** — 總覽型（架構、拓撲、roadmap）
- **entity** — 單一實體（database schema、state machine、person profile）
- **concept** — 抽象 / 流程（API surface、pipeline、protocol）
- **comparison** — 比較型（vendor A vs B、framework pros/cons）
- **synthesis** — 綜合分析 / runbook / 觀察清單

### Frontmatter 欄位

| 欄位 | 意義 | Lint 用途 |
|---|---|---|
| `title` | 主題名（可中文） | index.md 顯示 |
| `type` | 上述 5 類之一 | 組 index 分類 |
| `last_updated` | 日期 | stale check |
| `sources` | 列表，指向 raw/ 或外部來源 | traceability lint |

## Ingest workflow（raw → 主題頁）

1. 新 raw doc 下載 → 放 `raw/<source>/...` 或頂層 `<topic>.local.md`
2. **與 user 對焦關鍵要點**（Karpathy 強調步驟）— agent 讀 raw 後不要直接 ingest，**先跟 user 確認這份 raw 的核心主張 + 預計影響哪些主題頁**，避免 hallucination
3. 判斷影響哪幾個 wiki 頁（1 份 raw 常觸發 5-15 頁更新）
4. 去敏後寫進 wiki 頁，frontmatter `sources` 補 raw 路徑
5. **更新 `index.md`**（若新增頁 / 改 type / last_updated 變動）
6. `log.md` append：`YYYY-MM-DD HH:MM  INGEST  <raw 來源> → <受影響主題頁>`

## Query workflow

1. Agent 先讀 `index.md` 判斷該查哪幾頁（不盲 grep）
2. 讀對應 wiki 頁 → 綜合答案 **含 citation**（回指 wiki 頁或 raw 檔）
3. 主題頁沒有 → 檢查 raw/ → 補進 wiki 頁（走 Ingest workflow）
4. **好答案可升為新 wiki 頁**（Karpathy flywheel）— 若 query 回答結構化、refine 過、未來可能重複查，檔成 `wiki/<topic>.md`
5. 重大 insight / decision → `log.md` append `DECISION`

## Lint 節奏

- **建議節奏**：每週或每 sprint 跑一次
- **Threshold**：`log.md` 最後 `LINT` entry > 14 天 → agent session 起手時**自動提醒** user（見 [CLAUDE.md](CLAUDE.md) §Session 起手 hygiene check）
- 結果 `log.md` append `LINT` 摘要（即使全通過也要 append，作為「已跑過」憑證）

Run `/llm-wiki-lint <path>`（見 [`kc_ai_skills/llm-wiki-lint`](https://github.com/KerberosClaw/kc_ai_skills/tree/main/llm-wiki-lint)）：

**檢查項**（Karpathy 完整版）：
- **頁面間矛盾**（contradictions）
- **陳舊聲明**（stale claims — 內文 vs raw 脫節，不是只看 mtime）
- **孤立頁面**（無入站 cross-ref）
- **缺失主題頁**（index 提到但不存在 / 應有但不存在的領域）
- **缺 cross-ref**（wiki 頁間 concepts 有邏輯關聯但沒連結）
- **缺 source traceability**（frontmatter `sources` 指向已消失的 raw）
- **資料缺口**（data gaps — 應該有但未收集到的 fact）
- **Frontmatter 完整性**（4 欄必備）

## 與其他系統的關係

- [`CLAUDE.md`](CLAUDE.md) — repo 全局 operating rules（寫入規則、紅線、session hygiene）
- [`index.md`](index.md) — wiki 內容索引（按類別分組）
- [`log.md`](log.md) — append-only ledger

## Journal 層說明（非 Karpathy）

`journal/` 是本 template 內建 domain extension，非 Karpathy LLM Wiki 模型的一部分。職責：

- Daily notes（`YYYYMMDD_dayN.md`）— 工作日誌 / 觀察 / 思考
- Meeting notes（`meeting_YYYYMMDD_<type>.md`）— 獨立會議紀錄
- Checklists / 問題清單 — 時序性準備工作

Journal 不進 Karpathy lint scope（非主題頁）。可按需 archive 或刪除。
