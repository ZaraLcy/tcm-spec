# tcm-spec

A Claude Code skill that structures spec-writing using Traditional Chinese Medicine (TCM) consultation as its framework.

## What it does

Invoke `/tcm-spec` and Claude acts as a TCM physician — conducting a ten-phase diagnostic consultation on your software problem or requirement, then producing a structured spec document (the prescription).

**Ten phases:**
望 → 聞 → 問 → 切 → 病因病機 → 治療設計 → 預期結果 → 真正結果 → 療效反思 → 後續修正

Works for: new features, technical design, bug diagnosis.

## Installation

```bash
mkdir -p ~/.claude/skills/tcm-spec
cp skills/tcm-spec/SKILL.md ~/.claude/skills/tcm-spec/SKILL.md
```

Then in any Claude Code session, invoke with `/tcm-spec`.

## Repo structure

```
skills/tcm-spec/SKILL.md   ← the skill (install this)
docs/specs/                ← design document
docs/plans/                ← implementation plan
```
