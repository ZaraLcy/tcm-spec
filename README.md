# tcm-spec

A Claude Code skill that structures spec-writing using Traditional Chinese Medicine (TCM) consultation as its framework.

## What it does

Invoke `/tcm-spec` and Claude acts as a TCM physician — conducting a ten-phase diagnostic consultation on your software problem or requirement, then producing a structured spec document (the prescription / 藥方).

The skill forces thorough problem understanding before any solution is written. You cannot jump straight to implementation — just as a physician must diagnose before prescribing.

**Ten phases:**

| Phase | Chinese | What happens |
|-------|---------|-------------|
| 1 | 望（望診） | Claude silently reads codebase context, logs, and artifacts |
| 2 | 聞（聆聽） | You describe the problem freely — Claude only listens |
| 3 | 問（問診） | Claude asks clarifying questions one at a time (scope, constraints, context) |
| 4 | 切（切診） | Claude probes the technical substrate — dependencies, architecture, integration points |
| 5 | 病因病機 | Root cause analysis using TCM organ/pathology metaphors mapped to software patterns; **user confirms diagnosis before proceeding** |
| 6 | 治療設計 | Concrete treatment plan: which files/modules/interfaces change; **user confirms plan before proceeding** |
| 7 | 預期結果 | Verifiable, measurable success criteria |
| 8 | 真正結果 | *(filled in after implementation)* |
| 9 | 療效反思 | *(filled in after implementation)* — gap analysis |
| 10 | 後續修正 | *(filled in after implementation)* — next iteration |

**Works for:**

- New feature design
- Technical architecture decisions
- Bug root cause investigation

## Output

A spec document saved to `docs/specs/YYYY-MM-DD-<主訴>.md`, structured according to all ten phases.  
Phases 8–10 are scaffolded as placeholders and filled in after implementation.

## Installation

```bash
mkdir -p ~/.claude/skills/tcm-spec
cp skills/tcm-spec/SKILL.md ~/.claude/skills/tcm-spec/SKILL.md
```

Then in any Claude Code session, invoke with:

```
/tcm-spec
```

## Usage

```
/tcm-spec
```

Claude will open the consultation. You describe your problem when prompted.  
You do **not** need to know the phase names — Claude tracks them internally.

## Repo structure

```
skills/tcm-spec/SKILL.md   ← the skill (install this)
docs/specs/                ← example spec produced by the skill
docs/plans/                ← implementation plan for the skill itself
```

## License

MIT License

Copyright (c) 2026 ZaraLcy

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
