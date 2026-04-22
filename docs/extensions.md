# Extensions — 客製化方向

## Journal layer（本 template 內建）

Karpathy 原模型只有三層（raw / wiki / schema），沒 journal。本 template 加 `journal/` 是**本地 extension**，為了 cover 兩個 raw / wiki 沒有的場景：

- **時序工作日誌**：daily notes（你每天做了什麼、想了什麼）
- **Meeting notes**：會議紀錄，不屬主題頁也不是外部 raw
- **Checklists**：時序性準備工作（e.g. onboarding、週五 retro）

**與 Karpathy 原模型的衝突**：原模型假設「所有內容最終沉澱成 wiki 主題頁」。Journal 違反這個 — 它是**持續時序性** context，不一定沉澱。

**妥協方案**：journal 條目**可升級**成 wiki 頁（走 Ingest workflow），當它沉澱成跨時間 fact。例如「今天發現 X 系統的 Y 行為」→ 先進 journal（時序紀錄）→ 三天後確認是穩定 fact → 升為 `wiki/system_x.md`。

### 如果你不要 journal

`rm -rf journal/` + 從 `SCHEMA.md` / `CLAUDE.md` 刪 journal mention。純 Karpathy 三層就好。

## Frontmatter 欄位擴展

template 預設 4 欄（title / type / last_updated / sources）是最小集。可加：

- `tags` — 按主題標籤（適合 Obsidian Dataview）
- `status` — `draft` / `active` / `archived`
- `depth` — 深度 level（參考 [cablate/llm-atomic-wiki](https://github.com/cablate/llm-atomic-wiki) 的 atom 概念）
- `owner` — 維護者（多人 repo 有用）
- `review_cycle` — 建議 review 頻率（e.g. `quarterly`）

**注意**：每加一欄 = maintenance burden + lint 要 cover。加之前問：真的用到嗎？lint / query 會 reference 嗎？

## Type 分類擴展

Karpathy 預設 5 類：overview / entity / concept / comparison / synthesis。可加：

- `runbook` — 故障排查 / SOP（類似 synthesis 但更 actionable）
- `playbook` — 回應特定 scenario 的決策樹
- `meeting_notes` — 會議結果（如果你不用 journal 層）
- `glossary` — 術語表（一個 entity 短條目）

加 type 記得：

1. 更新 `SCHEMA.md` §Type 分類
2. 更新 `index.md` 可能新增分類區塊
3. `llm-wiki-lint` 的 type 檢查要同步（若 skill 有 hardcoded 5 類）

## 工具整合

本 template 只要求 `SCHEMA.md` + `wiki/` + `index.md` + `log.md`，其他 agnostic。可整合：

- **[Obsidian](https://obsidian.md/)** — wiki/ 當 vault，graph view 看 cross-ref
- **[Obsidian Dataview](https://blacksmithgu.github.io/obsidian-dataview/)** — 靠 frontmatter 查詢，加 `tags` / `status` 欄很有用
- **[qmd](https://github.com/seraph-ai/qmd)** — 本地 markdown 搜尋，BM25 + vector
- **[Marp](https://marp.app/)** — markdown slides，query 答案可產出簡報
- **[Hugo](https://gohugo.io/) / [Jekyll](https://jekyllrb.com/)** — 靜態網站產出（若 wiki 公開）

## Multi-wiki pattern

當你有**多個 domain** 的 wiki（e.g. 公司工作 + 個人研究 + 某 OSS project），可：

**Option A**：每 domain 一個 repo（推薦）
- 各自 `SCHEMA.md` / `CLAUDE.md` 可客製
- Private / public 各自決定
- 跨 wiki query 靠 agent 手動切 cwd

**Option B**：單 repo 多 `wiki-<domain>/` 子目錄
- 共用 `SCHEMA.md` / meta
- 不推薦：違反 Karpathy「單一 knowledge base」假設

## 整合 memory-lint

[kc_ai_skills/memory-lint](https://github.com/KerberosClaw/kc_ai_skills/tree/main/memory-lint) 針對 `~/.claude/memory/`（agent 個人 memory），不針對本 wiki。兩者可互補：

- `memory-lint` → Agent 個人 memory（feedback / user profile / projects）
- `llm-wiki-lint` → 本 wiki repo

Full-stack AI knowledge hygiene。

## 貢獻 / fork

本 template 是 MIT。Fork 之後：

1. 刪 `LICENSE` 上 KerberosClaw 版權，加自己的
2. 改 `README.md` 指向你的 repo
3. 視需要加 custom section 到 `SCHEMA.md`（標記「本地 extension」避免 upstream merge 衝突）
