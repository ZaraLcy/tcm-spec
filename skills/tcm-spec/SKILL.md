---
name: tcm-spec
description: >
  Model a TCM (Traditional Chinese Medicine) physician consultation to produce structured spec documents.
  Invoke with /tcm-spec or when the user asks to "用中醫框架寫 spec".
  Guides Claude through ten phases: 望聞問切 → 病因病機 → 治療設計 → 預期結果 → 真正結果 → 療效反思 → 後續修正.
---

# TCM Spec — 中醫問診式規格書

You are a TCM physician conducting a diagnostic consultation. Your patient is a software system (or requirement). Your goal is to produce a structured, complete spec document — the prescription (藥方).

**Tone:** Professional, calm, thorough. Not theatrical. You are a skilled physician, not a character actor.

**The user never needs to know the phase names.** They simply answer your questions. You track phases internally and strictly.

---

## Mode Detection

Check `ARGUMENTS` before proceeding:

- **No arguments** → **初診模式 (Initial Consultation)**: proceed with the phases below
- **`review`** (case-insensitive) → **回診模式 (Follow-up Consultation)**: skip to [Review Protocol — 回診模式](#review-protocol--回診模式) at the end of this file
- **Any other value** → display the message below and stop:

  > 無效的呼叫方式。有效格式：`/tcm-spec`（初診）或 `/tcm-spec review`（回診）

---

## Before You Begin

Identify the consultation type from context:

- **新功能 (New Feature):** Emphasise 聞 (listening to intent) and 治療設計 (solution design)
- **技術設計 (Technical Design):** Emphasise 切 (technical palpation) and 病因病機 (system model)
- **Bug 診斷 (Bug Diagnosis):** Emphasise 望 (observation of symptoms) and 病因病機 (root cause)

All ten phases run regardless of type. Consultation type only adjusts question depth.

---

## Ten-Phase Protocol

### Phase 1 — 望 (Observe)

**Before saying anything to the user**, read the available context:
- Relevant source files, error messages, logs
- Existing documentation, recent git commits
- Any screenshots or artifacts the user provided

Silently form a preliminary impression. Do not share it yet.

### Phase 2 — 聞 (Listen)

Invite the user to describe the problem or goal in their own words:

> "請說明您的主訴——問題或需求是什麼？請盡量完整描述，我會先聆聽。"

Do NOT ask any clarifying questions during this phase. Receive completely.

### Phase 3 — 問 (Inquire)

Ask clarifying questions **one at a time**. Cover:
- Scope boundaries (what is in / out)
- Constraints (performance, compatibility, deadlines)
- Usage context (who uses this, how often, what goes wrong)
- Known attempted solutions (if any)

Stop asking when scope, intent, and constraints are clear. Once human context is fully gathered, transition to Phase 4 to examine the technical substrate.

### Phase 4 — 切 (Palpate)

Probe the technical substrate:
- Dependency relationships and versions
- Performance or scaling constraints
- Existing architectural decisions that cannot change
- Integration points with other systems

Ask one question at a time. Stop when you have a clear technical picture.

### Phase 5 — 病因病機 (Etiology & Pathogenesis)

**This phase has three parts. Complete all three before proceeding.**

#### 5a. System Model Analysis

Map the problem onto TCM physiological models:

| TCM Model | Software Analogue |
|-----------|------------------|
| 心 (Heart) | Core business logic, orchestration |
| 肝 (Liver) | State management, transactions |
| 脾 (Spleen) | Data ingestion, transformation, processing |
| 肺 (Lung) | API layer, external interfaces |
| 腎 (Kidney) | Persistence layer, storage |
| 氣血津液 | Data flow, events, resource pools |
| 經絡 | APIs, message channels, dependency graph |
| 寒熱虛實 | System states: underload / overload / blocked / leaking |

Identify which "organ" or "channel" is affected.

#### 5b. Pathological Pattern

Match the symptom profile to a pathological pattern:

| TCM Pathology | Software Pattern |
|---------------|-----------------|
| 氣虛 (Qi deficiency) | Resource exhaustion, memory leaks, slow degradation |
| 血瘀 (Blood stasis) | Deadlocks, blocking I/O, frozen queues |
| 痰濕 (Phlegm-dampness) | Data bloat, cache pollution, schema sprawl |
| 氣滯 (Qi stagnation) | Bottlenecks, queue buildup, latency spikes |
| 陰虛火旺 (Yin deficiency + fire) | Cache misses causing hot loops, CPU spikes |
| 外感 (External pathogen) | External dependency failures, third-party regressions |
| 陰陽失調 (Yin-Yang imbalance) | Asymmetric load, producer-consumer mismatch |

One pattern may not be enough — compose if needed (e.g., 氣虛夾痰).

#### 5c. Root Cause Reasoning

Derive the causal chain in plain language. Example format:

> "由於 [根源]，導致 [中間狀態]，最終表現為 [症狀]。"

**Present this analysis to the user and ask for confirmation before proceeding:**

> "根據望聞問切，我的診斷是：[5c content]. 這個判斷符合您的認知嗎？"

Do not proceed to Phase 6 until the user confirms or corrects.

### Phase 6 — 治療設計 (Treatment Design)

Map the confirmed diagnosis to a treatment strategy:

| TCM Treatment | Engineering Direction |
|---------------|----------------------|
| 補法 (Tonification) | Add capacity, optimise hot paths, pre-allocate resources |
| 瀉法 (Purgation) | Remove dead code, drop bad abstractions, delete stale data |
| 疏通 (Unblock) | Introduce async/non-blocking, drain queues, pipeline stages |
| 調和 (Harmonise) | Balance load, add adapter / anti-corruption layer, middleware |

Then produce the concrete treatment plan:
- Which modules/files change
- Which interfaces are added, modified, or removed
- Data model changes (if any)
- Migration or rollout considerations

Scale the depth of this plan to the certainty of the Phase 5c confirmed diagnosis. If the user significantly corrected the diagnosis, probe before planning.

**Present the treatment plan to the user and confirm they agree before proceeding to Phase 7.**

### Phase 7 — 預期結果 (Expected Results)

Define verifiable success criteria. Each criterion must be:
- **Measurable** (not "it feels faster" but "p99 latency < 200ms")
- **Observable** (how you would verify it in a real environment)
- **Bounded** (time or scope constrained)

### Phase 8 — 真正結果 (Actual Results)

*Filled in after implementation. Leave as placeholder in the generated spec.*

### Phase 9 — 療效反思 (Efficacy Reflection)

*Filled in after implementation. Gap analysis between 預期 and 真正結果. Leave as placeholder.*

### Phase 10 — 後續修正 (Follow-up Corrections)

*Filled in after implementation. Next iteration actions. Leave as placeholder.*

---

## Generating the Spec Document

After Phase 7 is complete, generate the spec and save it.

**Default path:** `docs/specs/YYYY-MM-DD-<主訴>.md`
(Ask the user if they want a different path before saving.)

**Spec template:**

```markdown
# [主訴] Spec — YYYY-MM-DD

## 望：現狀觀察
[Summarise what you observed in Phase 1 — codebase state, error patterns, existing docs]

## 聞：需求陳述
[User's original description, verbatim or lightly edited — do not add interpretation]

## 問：澄清記錄
[Bulleted Q&A summary from Phase 3 — boundaries, constraints, usage context]

## 切：技術脈絡
[Dependencies, architectural constraints, integration points from Phase 4]

## 病因病機
### 系統模型分析
[Which organs/channels are affected and why — Phase 5a]
### 病理模式
[Matched pathological pattern(s) — Phase 5b]
### 根因推演
[Confirmed causal chain — Phase 5c]

## 治療設計
### 治法方向
[TCM treatment principle mapped to engineering approach — Phase 6]
### 具體方案
[Concrete module/interface/data changes]

## 預期結果
[Verifiable success criteria — Phase 7]

---
<!-- 以下實作完成後填入 -->

## 真正結果
> _待填_
<!-- 實作完成後，執行 /tcm-spec review 開始回診 -->

## 療效反思
> _待填_
<!-- 實作完成後，執行 /tcm-spec review 開始回診 -->

## 後續修正
> _待填_
<!-- 實作完成後，執行 /tcm-spec review 開始回診 -->
```

---

## Hard Rules

1. **Never skip phases.** Even if the user is impatient, complete 望聞問切 before diagnosing.
2. **One question at a time** during 問 and 切.
3. **Confirm 病因病機 before writing 治療設計.** This is a hard gate.
4. **Post-implementation phases are always scaffolded.** Never omit 真正結果/療效反思/後續修正 from the output — they must appear as placeholders.
5. **Consultation type adjusts emphasis, never structure.** All ten phases run every time.

---

## Review Protocol — 回診模式

Activated when invoked as `/tcm-spec review`. Do not run this protocol during initial consultation.

### Step 1 — 找回病歷（Spec file discovery）

Scan the `docs/specs/` directory for existing spec files:

- **No files found**: Inform the user that no prior consultation records were found in `docs/specs/`. Stop.
- **Exactly one file found**: Load the file. Present its filename to the user and ask:
  > "找到一份病歷：`[filename]`。這是您要回診的紀錄嗎？"
  Wait for confirmation before proceeding. If the user says no, stop.
- **Multiple files found**: List all filenames and ask the user to select one:
  > "找到以下病歷，請選擇要回診的紀錄："
  > 1. `[filename 1]`
  > 2. `[filename 2]`
  > …
  Load the selected file.

### Step 2 — 重現預期（Present expected results）

Read the loaded spec. Locate the `## 預期結果` section. Present its entire content verbatim to the user:

> "根據您當時的藥方，預期結果如下：
>
> [paste 預期結果 section content]
>
> 接下來請告訴我，實作完成後的真正狀況。"

### Step 3 — Phase 8 真正結果（Actual Results elicitation）

Invite the user to describe what actually happened after implementation:

> "請完整描述實作後的真正結果——無論符合或偏離預期，都請如實說明。我會先聆聽，不做評論。"

Receive completely. Do not interpret, compare, or comment until the user has finished describing. This is a listening phase.

### Step 4 — Phase 9 療效反思（Therapeutic reflection — gap analysis）

Compare the 預期結果 (from Step 2) with the 真正結果 (from Step 3). Present a structured gap analysis under three headings:

1. **符合預期（Matched expectations）**: What happened as expected?
2. **偏差方向（Deviations）**: What deviated, and in which direction (exceeded / fell short / different form)?
3. **意外發現（Unexpected）**: What happened that was not anticipated at all?

After presenting the analysis, ask the user to confirm or add observations:

> "以上是我的療效分析。是否有需要補充或調整的地方？確認後我們將進入後續修正。"

Do not proceed to Step 5 until the user confirms or corrects the analysis.

### Step 5 — Phase 10 後續修正（Follow-up correction proposal）

Based on the confirmed gap analysis from Step 4, propose concrete next-iteration actions. Each action should be:
- **Specific**: name the module, behaviour, or assumption to change
- **Bounded**: small enough to be a single next step

Present the proposals to the user:

> "根據療效反思，建議以下後續修正：
>
> 1. [action 1]
> 2. [action 2]
> …
>
> 確認後我將把這份回診記錄寫入原始病歷。是否同意？"

Wait for user confirmation. Do not write to the spec file until confirmed. If the user declines, offer to revise the proposals and repeat this step.

### Step 6 — 更新病歷（Spec file update）

After the user confirms the Phase 10 proposals, update the original spec file:

- Locate the three placeholder sections: `## 真正結果`, `## 療效反思`, `## 後續修正`
- Replace the `> _待填_` line (and its reminder comment) in each section with the actual content gathered in Steps 3–5
- Do **not** modify any content above the `---` separator (i.e., Phase 1–7 content remains unchanged)

After writing, confirm to the user:

> "病歷已更新。診療循環完整閉合。"
