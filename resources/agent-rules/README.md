# Agent Rules — AI Agent 行為規則集合

可複製貼上的行為規則片段，用於改善 AI agent 的問題解決能力。

## 使用方式

1. 從下方選擇你需要的 rule（依類別分組）
2. 點進檔案，複製「可貼上片段」的內容，貼進你專案的 `CLAUDE.md`、`AGENTS.md` 或其他 agent 設定檔
3. 可以只用一個，也可以多個一起用（不衝突）
4. 想深入了解每個規則背後的道理？往下捲看「完整說明」

## 命名慣例

檔名格式：`{category}_{verb-object}.md`

- `problem_` → 思考模式（遇到問題怎麼想）
- `safety_` → 安全（什麼事不能做）
- `workflow_` → 工作流（怎麼做事更有效）
- `design_` → 視覺與簡報設計（排版防坑、色彩語意、簡報工程）

看到檔名就知道規則在說什麼。

---

## 問題解決 (problem)

| 檔案 | 一句話說明 |
|------|-----------|
| [problem_stop-brute-force.md](./problem_stop-brute-force.md) | 遇到問題先理解機制，不要暴力嘗試 |
| [problem_verify-api-result.md](./problem_verify-api-result.md) | 非同步回傳成功 ≠ 真的成功，要 poll 確認 |

## 安全 (safety)

| 檔案 | 一句話說明 |
|------|-----------|
| [safety_authorize-each-action.md](./safety_authorize-each-action.md) | 外部操作需逐案授權，授權不傳遞 |
| [safety_ai-output-is-draft.md](./safety_ai-output-is-draft.md) | AI 輸出只是草稿，人類確認才算數 |

## 工作流 (workflow)

| 檔案 | 一句話說明 |
|------|-----------|
| [workflow_atomic-commits.md](./workflow_atomic-commits.md) | 一個 commit 只做一件事 |
| [workflow_match-test-to-risk.md](./workflow_match-test-to-risk.md) | 風險越高，測試越嚴格 |

## 視覺與簡報設計 (design)

| 檔案 | 一句話說明 |
|------|-----------|
| [design_slide-deck-standards.md](./design_slide-deck-standards.md) | 簡報防禦性排版、死空間防護、色彩語意與去 AI 腔 |

---

## 預設組合

不知道從哪裡開始？根據你的專案類型選擇：

### 基本款（任何專案都建議）

```
problem_stop-brute-force — 思維基礎
workflow_atomic-commits — Git 紀律
```

### 有外部操作的專案（PR / Store / Deploy）

```
基本款 +
safety_authorize-each-action — 安全授權
```

### 有 AI 功能的專案

```
基本款 +
safety_ai-output-is-draft — AI 輸出不當成結果
problem_verify-api-result — API 狀態驗證
```

### 有資料正確性要求的專案

```
基本款 +
workflow_match-test-to-risk — 風險匹配測試
```

### 全包（最大保護）

```
全部 6 個都貼上
```

---

## 全域守則

所有 rule 共用以下底線：

1. **繁體中文**：rule 內容以繁體中文撰寫。
2. **解釋 Why**：用類比和原因讓 agent 理解，不只下冰冷指令。
3. **不衝突**：每個 rule 覆蓋行為的不同面向，可以自由組合。

---

**最後更新：2026-07-15**
