# job-pilot · 求职周复盘副驾

**一个会记账、会自己醒来的求职副驾。**它用一份纯 CSV 台账记住你投过的每一家公司，每周把它变成漏斗分析、跟进提醒和可执行的策略——用你的真实回复率说话，不凭感觉。

[English](README.md) · [示例周报](examples/weekly-report-example.md)

## 专家模式替代测试

你随时可以问 AI"帮我看看求职该怎么办"，得到泛泛的建议。但有些东西**问了也得不到**：

| 能力 | job-pilot 做的事 | 为什么问 AI 替代不了 |
|---|---|---|
| **状态** | 一份 CSV 跨会话记住你所有投递 | 对话明天就失忆 |
| **调度** | 周复盘由宿主定时自动触发；跟进靠日期计算（投递 >7 天无声、面试 >5 天无声、7 天内待办） | AI 不会周一早上自己醒来 |
| **计算** | 按渠道/岗位类型算你的真实转化率："内推 4/4=100%，猎聘 0/4=0%，火力转移" | 泛建议根本不看你的漏斗 |

## 它能做什么

- **记一笔**——说"今天投了字节的后端，Boss 上聊的，30k" → 解析入台账
- **更新状态**——"星辰约面了" → 行更新，待办自动跟踪
- **周复盘**（核心）——按渠道/岗位拆漏斗、带现成话术的跟进清单、可执行的策略。投递不足 10 条时老实说"样本不足"，只给跟进不给策略
- **Boss直聘增量**（可选，配合 [Agent-Reach](https://github.com/Panniantong/Agent-Reach)）——按保存的筛选条件扫 Boss，对台账去重，只给新岗位

## 安装

```bash
# ZCode / 使用 ~/.agents/skills 的 agent
git clone https://github.com/tujunqi/job-pilot ~/.agents/skills/job-pilot

# Claude Code
git clone https://github.com/tujunqi/job-pilot ~/.claude/skills/job-pilot
```

台账在 `~/.job-pilot/applications.csv`（首条记录自动创建；环境变量 `JOB_PILOT_CSV` 可改路径）。纯 CSV、无锁定——数据是你的。

## 定时周复盘

- **ZCode**：建一个自动化——每周一 09:00，让它执行本 skill 的周复盘动作
- **有 cron 的环境**：把同样的指令挂到每周任务上
- **没有定时器？**随时说一句"复盘一下"，状态层不依赖调度也成立

## 示例

[examples/applications.example.csv](examples/applications.example.csv)（25 条虚构数据）→ 它生成的 [周报示例](examples/weekly-report-example.md)：渠道漏斗、可直接发的跟进话术、绑定数字的策略。

## 设计原则

- **台账是唯一事实源**——报告里每个数字都能溯源到行，禁止编造
- **小样本自知**——投递 <10 条只给跟进清单，不给策略结论
- **建议必须可执行**——"内推占比从 16% 提到 40%"✓，"要更努力"✗
- **对招聘平台只读**——绝不代你自动投递（合规与心智双保险）

MIT © 2026 tujunqi · 可选数据层：[Agent-Reach](https://github.com/Panniantong/Agent-Reach)
