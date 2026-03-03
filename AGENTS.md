# AGENTS

## Repo 目標

此 repository 用來集中管理通用的 Agent 設定與資產，包含但不限於：

- Agent 行為規範（例如本檔）
- Skills 清單與來源鎖定（`skills-lock.json`）
- 可重複使用的技能內容（`.agents/skills/`）

目標是提供一致、可追蹤、可重現的 Agent 工作基準，讓不同專案可直接套用。

## 範圍

本 repo **只負責設定與規範**，不承載特定產品功能程式碼。

- 允許：新增/更新 skills、調整 Agent 規範文件、維護鎖定檔
- 不建議：放入與 Agent 設定無關的應用程式實作

## 目錄慣例

- `.agents/skills/`：已安裝的 skills 內容
- `MEMORY.md`：專案記憶索引（前 200 行為高優先摘要）
- `skills-lock.json`：skills 來源與雜湊鎖定，確保可重現
- `AGENTS.md`：Agent 通用工作規範（單一事實來源）

## MEMORY 模式

本 repo 採用 `MEMORY.md` 作為 Agent 的持久化記憶入口，行為如下：

1. 啟動載入：每次新任務開始時，Agent 應先讀取 `MEMORY.md` 前 200 行。
2. 最小讀取：預設僅載入索引與必要片段，避免一次載入過多歷史。
3. 按需載入：只有當任務需要時，才讀取對應主題記憶檔，避免上下文噪音。
4. 任務後更新：若產生可重用知識（指令、慣例、踩雷、決策），應回寫記憶。
5. 明確覆寫：若新規則與舊記憶衝突，以最新且明確的決策為準，並同步修訂索引。

### 記憶檔寫作規範

- 精簡可驗證：使用短句與條列，避免模糊描述。
- 可追溯：重要決策附上日期與原因。
- 避免敏感資訊：不得寫入密碼、token、個資或受限制金鑰。
- 控制大小：`MEMORY.md` 建議維持在 200 行內，超出時拆分到主題檔。

### 建議目錄

```text
.
├── AGENTS.md
├── MEMORY.md
└── .agents/
	├── skills/
```

### 更新觸發條件（MVP）

- 使用者明確要求「記住這件事」。
- 同一問題重複出現兩次以上，且已有穩定解法。
- 新增或變更固定工作流程（build/test/release/review）。
- 發生一次高成本錯誤，且已確認預防方式。

## Skills 管理原則

1. 任何 skill 調整都應同步更新 `skills-lock.json`。
2. 優先使用可追溯來源（例如 GitHub repo）與固定雜湊。
3. 新增 skill 時需附上用途與觸發情境，避免重複或功能重疊。
4. 若 skill 涉及外部工具，需在文件註明前置需求。

## Agent 協作流程（MVP）

1. 釐清需求：先確認目標是「新增 skill」、「調整規範」或「更新鎖定」。
2. 實作變更：只修改必要檔案，保持差異最小。
3. 一致性檢查：確認文件、技能內容、鎖定資訊互相一致。
4. 交付說明：提供變更摘要、影響範圍、後續建議。

## Git 規範（Conventional Commits）

- 採用 Conventional Commits `v1.0.0`：https://www.conventionalcommits.org/en/v1.0.0/
- Commit 標題格式：`<type>[optional scope][!]: <description>`
- 可選擇加入內文與 footer，且需與標題間保留一個空行。
- `type` 至少應支援：`feat`、`fix`、`docs`、`style`、`refactor`、`perf`、`test`、`build`、`ci`、`chore`、`revert`。

### Breaking Change 規則

- 若為破壞性變更，需使用以下任一方式：
	- 在 type/scope 後加上 `!`（例如：`feat(api)!: remove v1 endpoints`）
	- 在 footer 加上 `BREAKING CHANGE: <description>`

### 建議範例

- `feat(skills): add pdf merge workflow`
- `fix(memory): avoid duplicate index entries`
- `docs(agents): clarify maintainer responsibilities`
- `refactor(skills)!: redesign lock schema`
- `BREAKING CHANGE: skills-lock.json schema updated; old keys are no longer supported.`

## Agent 角色矩陣（MVP）

### Builder

- 職責：實作技能與規範變更，確保最小且可追蹤的差異。
- 可做：新增/更新 `.agents/skills/`、調整 `AGENTS.md`、同步 `skills-lock.json`。
- 不可做：引入與 Agent 設定無關的應用程式程式碼。

### Reviewer

- 職責：檢查一致性與可維護性，確認變更符合 repo 範圍。
- 可做：驗證 skills 來源與鎖定資訊、檢查文件是否完整清楚。
- 不可做：在審查階段擴張需求或加入未對齊目標的功能。

### Maintainer

- 職責：維護規範品質與演進方向，管理破壞性變更。
- 可做：定義/更新流程準則、規劃版本化與發布策略。
- 不可做：在未公告遷移方式前直接變更既有規範行為。

## 變更準則

- 以最小可行變更為優先，避免無關重構。
- 不在未說明的情況下引入破壞性變更。
- 文件應優先清楚描述「目的、限制、操作方式」。

## 後續可擴充項目

- 加入「PR 驗收清單」與「版本發布流程」。
- 加入「Skill 品質評估指標」（覆蓋率、可維護性、重用性）。
