# tcm-spec

A Claude Code skill that structures spec-writing using Traditional Chinese Medicine (TCM) consultation as its framework.

一個以中醫問診為框架的 Claude Code 技能，將軟體規格撰寫轉化為十個診療階段，幫助你在動手實作之前，徹底釐清問題本質。

---

## What it does / 這個技能做什麼

The skill has two modes:

- **`/tcm-spec`** — Initial consultation (初診). Claude acts as a TCM physician, conducting a ten-phase diagnostic on your software problem and producing a structured spec document (the prescription / 藥方).
- **`/tcm-spec review`** — Follow-up consultation (回診). After implementation, return to complete Phases 8–10: actual results, efficacy reflection, and follow-up corrections.

此技能有兩種模式：

- **`/tcm-spec`** — 初診。Claude 化身中醫師，針對你的問題進行十個階段的問診，產出結構化規格文件（藥方）。
- **`/tcm-spec review`** — 回診。實作完成後，回來填寫第 8–10 階段：真正結果、療效反思、後續修正。

The skill forces thorough problem understanding before any solution is written. You cannot jump straight to implementation — just as a physician must diagnose before prescribing. And the cycle only closes when you return for follow-up.

這個技能強迫你在寫任何解法之前，先完整理解問題。就像醫師必須先診斷，才能開藥——而診療循環，要等到回診後才真正閉合。

**Ten phases / 十個階段：**

| Phase | 階段 | What happens / 說明 |
|-------|------|---------------------|
| 1 | 望（望診） | Claude silently reads codebase context, logs, and artifacts / Claude 靜默閱讀程式碼、日誌與現有文件 |
| 2 | 聞（聆聽） | You describe the problem freely — Claude only listens / 你自由描述問題，Claude 只聆聽不打斷 |
| 3 | 問（問診） | Claude asks clarifying questions one at a time (scope, constraints, context) / Claude 逐一提問，釐清範圍、限制與使用情境 |
| 4 | 切（切診） | Claude probes the technical substrate — dependencies, architecture, integration points / Claude 深入探查技術底層：依賴、架構決策、整合點 |
| 5 | 病因病機 | Root cause analysis using TCM organ/pathology metaphors mapped to software patterns; **user confirms diagnosis before proceeding** / 以中醫臟腑與病理模型進行根因分析；**使用者確認診斷後才繼續** |
| 6 | 治療設計 | Concrete treatment plan: which files/modules/interfaces change; **user confirms plan before proceeding** / 具體治療方案：哪些檔案、模組、介面需要變動；**使用者確認後才繼續** |
| 7 | 預期結果 | Verifiable, measurable success criteria / 可量測、可驗證的成功標準 |
| 8 | 真正結果 | *(filled in via `/tcm-spec review` / 執行回診後填入)* — what actually happened after implementation |
| 9 | 療效反思 | *(filled in via `/tcm-spec review` / 執行回診後填入)* — gap analysis between expected and actual / 預期與實際的落差分析 |
| 10 | 後續修正 | *(filled in via `/tcm-spec review` / 執行回診後填入)* — next iteration actions / 下一輪改善行動 |

**Works for / 適用場景：**

- New feature design / 新功能設計
- Technical architecture decisions / 技術架構決策
- Bug root cause investigation / Bug 根因調查

---

## Output / 輸出

**Initial consultation** produces a spec saved to `docs/specs/YYYY-MM-DD-<主訴>.md`.  
Phases 8–10 are scaffolded as placeholders. Each placeholder includes a reminder:

```
## 真正結果
> _待填_
<!-- 實作完成後，執行 /tcm-spec review 開始回診 -->
```

**Follow-up consultation** (`/tcm-spec review`) loads the existing spec, guides you through Phases 8–10 interactively, and writes the completed content back into the file.

**初診**產出規格文件，儲存於 `docs/specs/YYYY-MM-DD-<主訴>.md`。  
第 8–10 階段預留為空白，並附上 `/tcm-spec review` 的提示註解。

**回診**（`/tcm-spec review`）會載入現有的規格文件，逐步引導你完成第 8–10 階段，並將內容寫回原始文件，閉合診療循環。

---

## Installation / 安裝

```bash
mkdir -p ~/.claude/skills/tcm-spec
cp skills/tcm-spec/SKILL.md ~/.claude/skills/tcm-spec/SKILL.md
```

Then in any Claude Code session, invoke with:  
安裝完成後，在任意 Claude Code session 中輸入：

```
/tcm-spec
```

---

## Usage / 使用方式

### Initial consultation / 初診

```
/tcm-spec
```

Claude will open the consultation. You describe your problem when prompted.  
You do **not** need to know the phase names — Claude tracks them internally.

Claude 會開啟問診。當 Claude 提示時，描述你的問題即可。  
你**不需要**知道各階段的名稱，Claude 會在內部追蹤進度。

### Follow-up consultation / 回診

```
/tcm-spec review
```

Run this after you finish implementing the prescription. Claude will:
1. Find your spec file(s) in `docs/specs/`
2. Show you the expected results from Phase 7
3. Ask you to describe what actually happened (Phase 8)
4. Present a gap analysis (Phase 9)
5. Propose next-iteration actions (Phase 10)
6. Write everything back into the original spec file — closing the loop

實作完成後執行此指令。Claude 會找出 `docs/specs/` 中的規格文件，引導你逐步完成第 8–10 階段，並將結果寫回原始文件，讓診療循環真正閉合。

---

## Repo structure / 專案結構

```
skills/tcm-spec/SKILL.md   ← the skill (install this) / 技能本體（安裝這個）
docs/specs/                ← example spec produced by the skill / 技能產出的範例規格書
docs/plans/                ← implementation plan for the skill itself / 技能本身的實作計畫
```

---

## Changelog

### 2026-04-26 — Follow-up consultation mode (回診機制)

- **New**: `/tcm-spec review` — a dedicated follow-up mode that guides you through Phases 8–10 after implementation
- **New**: Mode detection at skill start — routes to initial or follow-up consultation based on `ARGUMENTS`
- **Changed**: Initial consultation spec output now includes `/tcm-spec review` reminder comments below each Phase 8–10 placeholder

The diagnostic cycle can now fully close. Phase 8–10 are no longer permanent placeholders.

---

## License / 授權

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)

Copyright (c) 2026 ZaraLcy

本作品採用**創用 CC 姓名標示—非商業性 4.0 國際授權**。

你可以自由使用、修改與散布本作品，條件是：
- **署名**：保留原作者名稱（ZaraLcy）
- **非商業**：不得用於商業目的

This work is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License**.  
You are free to use, modify, and share this work, provided that you:
- **Attribute** the original author (ZaraLcy)
- **Do not use it for commercial purposes**
