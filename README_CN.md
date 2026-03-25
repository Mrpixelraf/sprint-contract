# 🎯 Sprint Contract（冲刺合同）

> 多智能体开发工作流：通过冲刺合同 + 独立 QA 评估确保交付质量。

灵感来自 [Anthropic 的长时运行应用 harness 设计](https://www.anthropic.com/engineering/harness-design-long-running-apps) —— 让做事的人和评判的人分开。

## 为什么需要这个？

大语言模型在自我评估时总是倾向于夸奖自己。你让一个编程 agent 评价自己的代码，它会说"看起来不错！"——哪怕有明显的 bug。这个 skill 通过两个机制解决这个问题：

1. **冲刺合同** —— 每个任务在开工前就列出明确的、可测试的完成标准
2. **独立评估** —— 由一个独立的 agent 按照这些标准打分，并被提示要严格挑刺

## 安装

```bash
clawhub install sprint-contract
```

或者直接克隆：

```bash
git clone https://github.com/Mrpixelraf/sprint-contract.git
cp -r sprint-contract ~/.openclaw/skills/
```

## 工作流程

```
你（规划者）→ 写 BRIEF.md，包含冲刺合同
                    ↓
         生成者（子 agent）执行开发
                    ↓
         评估者（独立子 agent）测试验收
                    ↓
         通过？→ 交付
         不通过？→ 生成者修复 → 评估者重新测试
```

### 第一步：写 BRIEF.md

包含**冲刺合同**，列出具体的、可验证的完成条件：

```markdown
## 冲刺合同（完成标准）
- [ ] 页面加载无控制台报错
- [ ] 移动端 375px 宽度自适应
- [ ] CTA 按钮链接到 /signup
- [ ] Lighthouse 性能分数 > 90
```

### 第二步：派遣生成者

把 BRIEF.md 交给它。它按冲刺合同开发，完成后写 HANDOFF.md。

### 第三步：派遣独立评估者

评估者按 4 个维度逐条测试：
1. **功能完整性** —— 每条完成标准是否达标
2. **用户体验** —— 流程是否直觉，有没有死路
3. **视觉质量** —— 布局、间距、配色是否专业
4. **代码质量** —— 有没有报错、逻辑问题

关键提示词：*"你的工作是找问题，不是夸奖。如果你觉得一切都好，说明你没仔细测。"*

### 第四步：交付或返工

- 全部通过 → 交付
- 有条目未通过 → 评估报告反馈给生成者修复
- 架构级问题 → 上报给人类决策

## 适用场景

| 任务复杂度 | 生成者 | 评估者 |
|-----------|--------|--------|
| 简单（改 typo、配置） | 子 agent | 自评，标注 "⚠️ 未经独立测试" |
| 中等（新功能、修 bug） | 子 agent | **独立子 agent** |
| 复杂（架构、新项目） | Claude Code / ACP | **独立子 agent + 人工审查** |

## 合同模板

`references/contract-examples.md` 包含 6 类项目模板：
- 🖥️ Web 应用（前端）
- 🎬 视频脚本
- 📊 Pine Script / 交易指标
- 🎨 设计 / 品牌资产
- ⚙️ API / 后端
- 🤖 Discord 机器人 / 定时任务

## 核心原则

1. **复杂任务绝不让执行者自评** —— 自评永远偏乐观
2. **标准必须具体可测** —— 不是"做好看点"而是"Lighthouse > 90"
3. **评估者要调教为严格模式** —— LLM 默认对 LLM 输出手下留情
4. **基于文件通信** —— BRIEF.md 进，HANDOFF.md 出
5. **一个冲刺做一件事** —— 不要把无关工作打包在一起

## 致谢

基于 Prithvi Rajasekaran 在 [Anthropic Labs](https://www.anthropic.com/engineering/harness-design-long-running-apps) 发表的研究。

## 许可证

MIT
