---
title: Example Overview Page
type: overview
last_updated: 2026-04-23
sources:
  - raw/source1/
  - raw/source2/
---

# Example Overview Page

**TLDR**: Overview 型主題頁範例 — 總覽型 content（系統架構、拓撲、roadmap、產品線全圖）。

## 何時用 `type: overview`

當你要寫一份「全局 view」主題 — 跨多個 entity / concept 的整合視角。適用例：

- System architecture（分層 / stack / 服務對照）
- Topology（節點 / 網路 / 機房）
- Product roadmap（多產品線 / 客戶分布）
- Team structure（組織圖 / 人員分工）
- Deployment（環境演進 / phase 切換）

## 5 種 type 對照

- **overview** — 總覽型（本頁）
- **entity** — 單一實體（database、state machine、person profile）— 見 [_example_entity.md](_example_entity.md)
- **concept** — 抽象 / 流程（pipeline、protocol、convention）
- **comparison** — 比較（vendor A vs B、framework pros/cons）
- **synthesis** — 綜合分析（runbook、風險清單、觀察總結）

## 內文結構建議

### Sections

1. **Overview / scope** — 這頁涵蓋什麼
2. **Entities / components** — 列出主要 entity，每個一段
3. **Relationships** — entity 之間的關係（可用 mermaid diagram）
4. **Cross-refs** — 指向其他主題頁 / raw 檔
5. **Open questions** — 尚未確認 / 待追蹤

### 內嵌 cross-ref 範例

entity 詳細見 [_example_entity.md](_example_entity.md)。
原文在 `../raw/source1/example_doc.md`（gitignored，local 才有）。

## 何時砍此檔

Clone 後如果不需要 example：`git rm wiki/_example_*.md`
保留當 reference：維持 `_` 前綴表示「template 保留，不屬於真 wiki」。

---

**[刪除此段 + 改寫為你自己的 overview 頁]**
