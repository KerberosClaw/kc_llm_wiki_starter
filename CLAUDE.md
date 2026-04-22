# Claude 操作指引 — {{ your-wiki-name }}

本 repo 採 [Karpathy LLM Wiki 三層 pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) + journal 本地 extension。

**Agent 動 `wiki/` 或 `raw/` 前先讀 [`SCHEMA.md`](SCHEMA.md)**。

## 結構

- `wiki/` — 主題頁（進 git，有 frontmatter）
- `raw/` — 外部 dump（gitignored，不進 git）
- `journal/` — 時序日誌（daily notes / meeting / checklist，本地 extension）
- `SCHEMA.md` / `log.md` / `index.md` — meta 層（結構宣告 / ledger / 索引）

Wiki 頁必有 frontmatter 4 欄（`title` / `type` / `last_updated` / `sources`）。Type 5 類：`overview` / `entity` / `concept` / `comparison` / `synthesis`。

## Ingest 紀律

- Ingest 前**與 user 對焦關鍵要點**（SCHEMA §Ingest workflow 第 2 步），避免 hallucination
- 去敏按本檔 §寫入規則
- 更新 `index.md` + `log.md`（SCHEMA §Ingest workflow 第 5-6 步）

## Session 起手 hygiene check

- Session 開始時 check `log.md` 最後 `LINT` entry 時間戳
- **Threshold**：> 14 天或從未跑過 LINT → 正事前提醒 user：
  > 「`log.md` 顯示上次 lint ≥ 14 天（或從未跑過），建議跑 `/llm-wiki-lint`」
- User 回「之後再跑」→ acknowledge，不糾纏
- Lint 跑完記得 append `LINT` entry 到 `log.md`

## 寫入規則（commit 前檢查）

**以下按你 repo 的敏感度調整**（template 預設保守）：

- [ ] Credentials / token / API key → 絕不進 repo（放 `.env.local` gitignored）
- [ ] 未經授權的 source code → 絕不進 repo
- [ ] 客戶名 / 真實 IP / 內部 schema — 視你 repo 是否 private + git-crypt 決定：
  - **Public repo**：抽象化或刪除
  - **Private + git-crypt**：可留真名（但 commit message 仍需去敏，因 git-crypt 不加密 message）

## Commit message

- 即使 git-crypt 加密檔內容，**commit message 不加密** → GitHub API 明文可取
- Message 用抽象描述 / ticket 編號，不寫敏感細節

## 客製化

Clone 後：
1. 改本檔 `{{ your-wiki-name }}` placeholder
2. 按你 repo 敏感度調整 §寫入規則
3. 砍 `wiki/_example_*.md` 兩份 example（或保留當 template reference）
4. 第一次 ingest 詳 [docs/getting-started.md](docs/getting-started.md)
