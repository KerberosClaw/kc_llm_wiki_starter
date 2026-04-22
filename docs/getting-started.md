# Getting Started — 5 分鐘第一次 ingest

## 0. Clone

```bash
git clone https://github.com/YOUR/kc_llm_wiki_starter.git my-wiki
cd my-wiki
```

## 1. 客製化 placeholder

找 `{{ your-wiki-name }}` 替換成你的 wiki 名稱：

```bash
grep -rl '{{ your-wiki-name }}' . --include='*.md'
# Edit each one: CLAUDE.md / SCHEMA.md / README.md / index.md / log.md
```

## 2. 調整寫入規則

打開 `CLAUDE.md` §寫入規則，按你 repo 是否 **public** / **private** / **git-crypt** 決定：

- **Public repo**：敏感資訊全部抽象化或刪除
- **Private repo 無加密**：不放 credentials / 公司 source code，其他注意
- **Private + git-crypt**：可留真名，但 commit message 仍去敏

## 3. 處理 example（可選）

```bash
# 如果不需要 reference
git rm wiki/_example_*.md
# 更新 index.md 砍 example 段

# 或留著當 template
# （底線 _ 前綴已 signal 這是 template reference）
```

## 4. 第一次 ingest

### 準備 raw

把要整理的 doc 放進 `raw/<source>/`：

```bash
mkdir -p raw/my-docs
cp ~/Downloads/some-spec.md raw/my-docs/
```

### 開 session

對 Claude 講：

> 「Ingest `raw/my-docs/some-spec.md` 到 wiki」

Agent 會：

1. **讀 raw**（`Read raw/my-docs/some-spec.md`）
2. **跟你對焦關鍵要點**：
   > 「這份 spec 的核心主張我看到 X、Y、Z — 預計影響：新建 `wiki/some_topic.md`（type: entity）+ 更新 `wiki/architecture.md`（type: overview）。對嗎？」
3. 去敏後**寫進 wiki 頁**，frontmatter `sources` 補 raw 路徑
4. **更新 `index.md`**（新頁 + last_updated）
5. **`log.md` append**：`2026-04-23 14:20  INGEST  raw/my-docs/some-spec.md → wiki/some_topic.md, wiki/architecture.md`

## 5. 第一次 query

對 Claude 講：

> 「XXX 是怎麼運作的？」

Agent 會：

1. **讀 `index.md`** 判斷該查哪幾頁
2. **讀對應 wiki 頁**
3. **回答含 citation**（回指 wiki 頁或 raw 檔）
4. **好答案升為新 wiki 頁**（若 query 結構化、可能重複查）

## 6. 一週後跑 lint

Install 需要的 skill（見 [README.md](../README.md) §搭配 Skill）：

```bash
cp -r /path/to/kc_ai_skills/llm-wiki-lint ~/.claude/skills/
```

跑：

```
/llm-wiki-lint
```

（不帶 path 會偵測 cwd，含 `SCHEMA.md` + `wiki/` 就採用）

報告會列：

- 🔴 error：frontmatter 缺 / index 不一致 / source traceability 斷鏈
- 🟡 warning：stale claims / orphan pages / contradictions
- 🔵 info：可能 missing cross-refs / 資料缺口

**跑完記得 append `log.md`**：`2026-04-30 09:00  LINT  5 warnings (3 stale / 2 orphan), 0 errors`

## 7. 定期 review

- **每週五**看 `log.md` 最後 N entry → 本週 knowledge base 變化 summary
- **每月 / 每 sprint** 看 `wiki/` 結構 → 是否需要 refactor type 分類 / 合併頁面

## 下一步

- [workflow.md](workflow.md) — Ingest / Query / Lint 深入
- [extensions.md](extensions.md) — 客製化方向
