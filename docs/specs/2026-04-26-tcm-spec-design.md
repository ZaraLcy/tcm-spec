# TCM Spec Skill — Design Doc
*Date: 2026-04-26*

## Overview

A Claude Code skill that structures spec-writing using Traditional Chinese Medicine (TCM) consultation as its framework. Claude adopts the role of a TCM physician, conducting a natural diagnostic dialogue, then produces a structured spec document (the "prescription").

**Skill name:** `tcm-spec`
**Trigger:** User invokes `/tcm-spec` or requests "用中醫框架來寫 spec"

---

## Role & Persona

Claude acts as a TCM physician — professional in tone, not theatrical. The user experiences a natural consultation dialogue. Internally, Claude strictly tracks all ten phases. The user never needs to know the phase names; they simply answer questions.

At the start, Claude briefly identifies the consultation type (new feature / technical design / bug diagnosis) and adjusts question weighting accordingly, while keeping the full ten-phase framework intact.

---

## Ten-Phase Framework

### Phase 1 — 望 (Observe)
*What Claude does:* Read codebase, existing documentation, git history, error messages, and any relevant context before speaking.

### Phase 2 — 聞 (Listen)
*What Claude does:* Invite the user to describe the problem or goal in their own words. Do not interrupt. Receive fully before asking anything.

### Phase 3 — 問 (Inquire)
*What Claude does:* Ask clarifying questions one at a time. Establish scope boundaries, constraints, and usage contexts.

### Phase 4 — 切 (Palpate)
*What Claude does:* Probe technical context — dependencies, performance limits, prior architectural decisions, integration points.

### Phase 5 — 病因病機 (Etiology & Pathogenesis)

**System Model Analysis** — Map the problem onto TCM physiological analogies:

| TCM Model | Software Analogue |
|-----------|------------------|
| 臟腑 (Organs: 心肝脾肺腎) | Core modules: logic, state, data processing, interface, persistence |
| 氣血津液 (Qi, Blood, Fluids) | Data flow, events, resources |
| 經絡 (Meridians) | APIs, message channels, dependency graph |
| 寒熱虛實 (Cold/Hot, Deficiency/Excess) | System states: underload / overload / blocked / leaking |

**Pathological Patterns** — Identify the matching pathology:

| TCM Pathology | Software Pattern |
|---------------|-----------------|
| 氣虛 (Qi deficiency) | Resource exhaustion, memory leaks |
| 血瘀 (Blood stasis) | Deadlocks, blocking I/O |
| 痰濕 (Phlegm-dampness) | Data bloat, cache pollution |
| 氣滯 (Qi stagnation) | Bottlenecks, queue buildup |
| 陰虛火旺 (Yin deficiency with fire) | Cache misses causing hot loops |
| 外感 (External pathogen) | External dependency failures |

**Root Cause Reasoning** — Derive the causal chain. Present to user for confirmation before proceeding.

### Phase 6 — 治療設計 (Treatment Design)

Map solution direction to TCM therapeutic principles:

| TCM Treatment | Spec Direction |
|---------------|---------------|
| 補法 (Tonification) | Add resources, optimise |
| 瀉法 (Purgation) | Remove, clean up, refactor |
| 疏通 (Unblock) | Introduce async, eliminate bottlenecks |
| 調和 (Harmonise) | Balance opposing forces, add adapter / middleware |

Produce concrete technical solution directions: module changes, interface redesigns, data model adjustments.

### Phase 7 — 預期結果 (Expected Results)
*Verifiable success criteria.* Define what "healed" looks like in measurable terms.

### Phase 8 — 真正結果 (Actual Results)
*Filled in post-implementation.* Initially a blank placeholder.

### Phase 9 — 療效反思 (Efficacy Reflection)
*Filled in post-implementation.* Gap analysis between expected and actual.

### Phase 10 — 後續修正 (Follow-up Corrections)
*Filled in post-implementation.* Next iteration actions.

---

## Output Format

Saved to `docs/specs/YYYY-MM-DD-<主訴>.md` by default (user may override path).

```markdown
# [主訴] Spec — YYYY-MM-DD

## 望：現狀觀察
> 系統現況、相關程式碼、既有文件摘要

## 聞：需求陳述
> 使用者原始描述，不加工

## 問：澄清記錄
> Q&A 摘要，邊界與限制

## 切：技術脈絡
> 依賴關係、既有設計決策、效能限制

## 病因病機
### 系統模型分析
> 以臟腑/氣血/經絡模型定位問題所在的「臟」
### 病理模式
> 對應的病理（氣虛、血瘀、痰濕…）
### 根因推演
> 為什麼會這樣，因果鏈

## 治療設計
### 治法方向
> 補/瀉/疏通/調和，以及理由
### 具體方案
> 技術實作方向，模組/介面變更

## 預期結果
> 可驗證的成功 criteria

---
<!-- 以下實作完成後填入 -->

## 真正結果
> _待填_

## 療效反思
> _待填_

## 後續修正
> _待填_
```

---

## Design Principles

- **Holistic before analytical:** 望聞問切 must complete before diagnosis. Claude does not jump to solutions.
- **One question at a time:** During 問 phase, ask one question per message.
- **Confirm 病因病機 before proceeding:** Present root cause reasoning and get user acknowledgement before writing 治療設計.
- **Post-implementation phases are scaffolded, not skipped:** The last three phases always appear in the output as blank placeholders, ready to be filled.
- **Consultation type adaptation:** Claude identifies spec type early and adjusts question emphasis, but never skips phases.
