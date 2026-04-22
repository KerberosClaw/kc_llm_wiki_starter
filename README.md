# kc_llm_wiki_starter

Clone 回去、餵資料，你就有自己的 **LLM-maintained wiki**。

基於 [Karpathy 2026-04 LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 的 production-ready template — 三層架構（raw sources / wiki pages / schema config）+ journal 本地 extension + frontmatter convention + lint hygiene。

## 為何用這個而不是 Obsidian / Notion / Confluence

- **LLM 維護成本近零**：LLM 不會忘記更新 cross-ref、不會厭倦做 bookkeeping
- **三層明確分工**：raw = 真實來源、wiki = LLM 整理、schema = 結構規則
- **Git native**：版本控制、diff、可 git-crypt 加密
- **無 vendor lock-in**：純 markdown，明天換 LLM / 換 tool 都帶走

## 快速開始

```bash
git clone https://github.com/YOUR/kc_llm_wiki_starter.git my-wiki
cd my-wiki
```

1. 改 `CLAUDE.md` / `README.md` 的 `{{ your-wiki-name }}` placeholder
2. 看 `wiki/_example_entity.md` 跟 `wiki/_example_overview.md` 學 frontmatter 格式
3. 砍 example 或保留當 template reference（底線前綴標示 non-real）
4. 開 Claude session，對 agent 說「ingest raw/xxx.md 到 wiki」→ 詳 [docs/getting-started.md](docs/getting-started.md)

## 核心概念（30 秒版）

```
raw/     ← 外部 dump（gitignored，不進 git）
wiki/    ← LLM 整理的主題頁（frontmatter + body）
journal/ ← 時序工作日誌（daily / meeting / checklist）
SCHEMA.md, log.md, index.md  ← meta 層
```

**三個 workflow**：
1. **Ingest**：新 raw doc → agent 跟你對焦關鍵 → 去敏後寫進 wiki → 更新 index + log
2. **Query**：問 agent → 他讀 index 決定查哪頁 → 讀 wiki → 回答含 citation
3. **Lint**：定期跑 `/llm-wiki-lint` → 抓 stale / orphan / missing / data gap

詳見 [docs/workflow.md](docs/workflow.md)。

## 搭配 Skill

推薦搭配 [`kc_ai_skills/llm-wiki-lint`](https://github.com/KerberosClaw/kc_ai_skills/tree/main/llm-wiki-lint)：

**為何需要**：wiki 超過 ~15 頁之後，stale claims（內文 vs raw 脫節）、orphan cross-refs（頁面孤立）、missing topic pages（應有但缺）、data gaps（`sources` 指向消失的 raw）會**默默腐爛**。LLM 自己不會發現，你讀到才會炸。llm-wiki-lint 每週跑一次抓 drift，讓 LLM 不會哪天自信地搬錯 fact 打臉你。

**Install**：
```bash
# 參考 kc_ai_skills README
cp -r kc_ai_skills/llm-wiki-lint ~/.claude/skills/
```

## Documentation

- [docs/getting-started.md](docs/getting-started.md) — 5 分鐘第一次 ingest
- [docs/workflow.md](docs/workflow.md) — Ingest / Query / Lint 詳細流程
- [docs/extensions.md](docs/extensions.md) — 客製化方向（journal layer、frontmatter 擴展、工具整合）

## License

MIT — 見 [LICENSE](LICENSE)
