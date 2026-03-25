# 🎯 Sprint Contract

> Multi-agent development workflow with Sprint Contracts and independent QA evaluation.

Inspired by [Anthropic's harness design for long-running apps](https://www.anthropic.com/engineering/harness-design-long-running-apps) — separate the agent doing the work from the agent judging it.

## Why

LLMs reliably praise their own output. When you ask a coding agent to evaluate its own work, it says "looks great!" — even when there are obvious bugs. This skill solves that by:

1. **Sprint Contracts** — Every task gets explicit, testable completion criteria *before* work begins
2. **Independent Evaluation** — A separate agent grades the work against those criteria, prompted to be skeptical

## Install

```bash
clawhub install sprint-contract
```

Or clone directly:

```bash
git clone https://github.com/Mrpixelraf/sprint-contract.git
cp -r sprint-contract ~/.openclaw/skills/
```

## How It Works

```
You (Planner) → Write BRIEF.md with Sprint Contract
                         ↓
              Generator (sub-agent) builds it
                         ↓
              Evaluator (independent sub-agent) tests it
                         ↓
              Pass? → Ship it
              Fail? → Generator fixes → Evaluator re-tests
```

### Step 1: Write a BRIEF.md

Include a **Sprint Contract** with specific, testable criteria:

```markdown
## Sprint Contract (Completion Criteria)
- [ ] Page loads without console errors
- [ ] Mobile responsive at 375px width
- [ ] CTA button links to /signup
- [ ] Lighthouse performance score > 90
```

### Step 2: Spawn a Generator

Give it the BRIEF.md. It builds against the Sprint Contract and writes a HANDOFF.md when done.

### Step 3: Spawn an Independent Evaluator

The evaluator tests each criterion and scores on 4 dimensions:
1. **Functional completeness** — does each criterion pass?
2. **User experience** — is the flow intuitive?
3. **Visual quality** — professional layout and styling?
4. **Code quality** — no errors, clean logic?

Key evaluator prompt: *"Your job is to find problems, not to praise. If everything looks fine, you probably didn't test carefully enough."*

### Step 4: Ship or Fix

- All criteria pass → done
- Criteria fail → feed evaluator report back to generator
- Architecture issues → escalate to human

## When to Use

| Complexity | Generator | Evaluator |
|-----------|-----------|-----------|
| Simple (typo, config) | Sub-agent | Self-evaluate, mark "⚠️ untested" |
| Medium (feature, bug fix) | Sub-agent | **Independent sub-agent** |
| Complex (architecture) | Claude Code / ACP | **Independent sub-agent + human** |

## Contract Templates

Included in `references/contract-examples.md`:
- 🖥️ Web App (Frontend)
- 🎬 Video Script
- 📊 Pine Script / Trading Indicator
- 🎨 Design / Brand Asset
- ⚙️ API / Backend
- 🤖 Discord Bot / Cron Task

## Key Principles

1. **Never let the builder grade their own work** on complex tasks
2. **Criteria must be specific and testable** — not "make it good" but "Lighthouse > 90"
3. **Evaluator must be tuned skeptical** — LLMs default to being too generous
4. **File-based communication** — BRIEF.md in, HANDOFF.md out
5. **One feature per sprint** — don't bundle unrelated work

## Credits

Based on research by Prithvi Rajasekaran at [Anthropic Labs](https://www.anthropic.com/engineering/harness-design-long-running-apps).

## License

MIT
