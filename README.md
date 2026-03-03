# agents

集中管理通用 Agent 設定與技能資產的 repository，提供一致、可追蹤、可重現的工作基準。

## 這個 repo 在做什麼

- 管理 Agent 行為規範（`AGENTS.md`）
- 管理 Skills 來源與雜湊鎖定（`skills-lock.json`）
- 管理可重複使用的 skills 內容（`.agents/skills/`）

> 此 repo 只負責 **設定與規範**，不承載特定產品功能程式碼。

## 目錄結構

```text
.
├── .agents/
│   └── skills/
├── AGENTS.md
├── LICENSE
└── skills-lock.json
```

## 使用方式

### 1) 新增或更新技能（Skills）

1. 在 `.agents/skills/` 新增或更新對應 skill。
2. 確認 skill 說明包含：用途、觸發情境、前置需求（若有外部工具）。
3. 同步更新 `skills-lock.json`，確保來源與 hash 可追溯且可重現。

### 2) 調整 Agent 規範

- 直接更新 `AGENTS.md`，並維持內容清楚描述：目的、限制、操作方式。
- 若變更屬於破壞性調整，請明確標示並提供遷移說明。

### 3) 變更提交規範

本專案採用 [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)。

範例：

- `feat(skills): add pdf merge workflow`
- `fix(memory): avoid duplicate index entries`
- `docs(agents): clarify maintainer responsibilities`

## 維護原則（MVP）

- 以最小可行變更為優先，避免無關重構。
- 文件、skills、鎖定資訊需保持一致。
- 優先採用可追溯來源與固定雜湊。

## 參考文件

- 規範總覽：`AGENTS.md`
- 技能鎖定：`skills-lock.json`
- 授權資訊：`LICENSE`

## License

本專案授權條款請見 `LICENSE`。
