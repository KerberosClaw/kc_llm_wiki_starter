---
title: Example Entity Page
type: entity
last_updated: 2026-04-23
sources:
  - raw/source1/example_doc.md
---

# Example Entity Page

**TLDR**: Entity 型主題頁範例 — 單一實體的完整 profile（database schema、state machine、person、system component）。

## 何時用 `type: entity`

當你要寫**單一 identifiable thing**的完整資料：

- Database schema（表結構、index、migration 歷史）
- State machine（狀態 + transition）
- Person profile（人員 / 聯絡 / 職責 / context）
- System component（一個特定 service 的 I/O + 依賴）

## 內文結構建議

### Sections

1. **Definition** — 這個 entity 是什麼、為何存在
2. **Attributes / schema** — 所有欄位 / 屬性 / 狀態
3. **Behavior** — 如何運作（規則、constraint）
4. **Relationships** — 與其他 entity 的關聯
5. **History / changes** — 重大變動歷史（optional）
6. **Open questions** — 尚未確認 / 待追蹤

## Cross-ref 範例

相關 overview 見 [_example_overview.md](_example_overview.md)。原檔 `../raw/source1/example_doc.md`。

## 何時砍此檔

Clone 後如果不需要 example：`git rm wiki/_example_*.md`

---

**[刪除此段 + 改寫為你自己的 entity 頁]**
