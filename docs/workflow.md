# Workflow — Ingest / Query / Lint 深入

## Ingest workflow（raw → wiki）

### 6 步流程

```
raw doc 下載 → 存 raw/
     ↓
   agent 讀 raw
     ↓
  與 user 對焦（關鍵步驟！）
     ↓
  判斷影響哪幾個 wiki 頁（1 份 raw 常觸發 5-15 頁更新）
     ↓
  去敏後寫進 wiki 頁，frontmatter sources 補 raw 路徑
     ↓
  更新 index.md（若新增頁 / 改 type / last_updated）
     ↓
  log.md append INGEST entry
```

### 為何「與 user 對焦」不能 skip

Karpathy 原文明確這步。實務上 agent 直接 ingest 容易：

- **錯抓重點**：把 raw 邊緣細節當核心寫進主題頁
- **過度泛化**：把一次 context-specific 的 fact 寫成通用原則
- **重複 ingest**：同 fact 進多頁，日後 drift 風險

對焦格式建議：

```
Agent: 「這份 raw 我看到 3 個核心主張：
  1. X — 影響 wiki/infra.md
  2. Y — 新建 wiki/y_topic.md (type: entity)
  3. Z — 影響 wiki/roadmap.md

漏掉什麼 / 抓錯什麼嗎？」
```

### Frontmatter 更新規則

每次 ingest 觸及某頁：

- `last_updated` 改成今天
- `sources` 追加新 raw 路徑（已有的不重複）

### Commit message

- 用具體描述：`ingest: vendor API spec → api_surface + deployment`
- 不寫 credentials / 敏感 entity name（即使 git-crypt 加密 blob，message 仍明文）

## Query workflow

### 5 步流程

```
User 問問題
     ↓
  agent 讀 index.md 判斷該查哪幾頁
     ↓
  讀對應 wiki 頁 → 綜合答案含 citation
     ↓
  (可選) 好答案升為新 wiki 頁（flywheel）
     ↓
  (可選) 重大 insight → log.md append DECISION
```

### Citation 格式

Agent 回答必須帶 citation：

```
「XXX 的運作：...（詳 `wiki/architecture.md §XXX 流程` + 原檔 `raw/source/xxx.md`）」
```

### Flywheel：好答案升為新頁

當 query 的答案：

- **結構化**（不是一兩句話的 quick reply）
- **refine 過**（跟 user 來回對焦過）
- **未來可能重複查**（其他人 / 未來你自己會再問）

→ 建檔成 `wiki/<topic>.md`，frontmatter `sources` 補「（query-derived from session YYYY-MM-DD）」

這讓 wiki **自我生長**，不是死 ingest。

## Lint workflow

### 建議節奏

- **每週**：跑一次完整 lint，catch 最新 drift
- **每 sprint / milestone**：深度 review，考慮 wiki 結構 refactor
- **Session 起手自動提醒**：log.md 最後 LINT > 14 天 → agent 主動提

### 跑 lint

```
/llm-wiki-lint [path]
```

（不帶 path 會偵測 `cwd`，若含 `SCHEMA.md` + `wiki/` 就採用）

### 報告處理

Lint 輸出分三級：

- 🔴 **Error**（必修）：frontmatter 缺欄位 / index 不一致 / source 斷鏈
- 🟡 **Warning**（review）：stale / orphan / contradiction
- 🔵 **Info**（可選）：missing cross-refs / data gaps

### Append log 即使全通過

即使 lint 結果全 🟢，**還是要 append `log.md`**：

```
2026-04-30 09:00  LINT  全通過（17 頁 / frontmatter 全完整 / 無 stale）
```

為什麼？

- **Threshold check** 需要「上次跑」的時間戳（見 [CLAUDE.md](../CLAUDE.md) §Session 起手 hygiene check）
- 沒 append 會被誤判「從未跑過」每 session 都提醒 → noise

## 邊界：哪些進 wiki / 哪些進 journal

- **主題式 / 跨時間 fact** → `wiki/`（e.g.「OpenAI 費用結構」、「MySQL schema」）
- **事件式 / 時序紀錄** → `journal/`（e.g.「今天站會 X 說」、「週四跟 Y 1:1」）
- **Meta 動作** → `log.md`（INGEST / UPDATE / LINT / DECISION）

Journal 條目可**升級**成 wiki 頁（當它沉澱成跨時間 fact），走 Ingest workflow。
