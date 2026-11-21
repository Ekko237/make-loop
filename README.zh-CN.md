# make-loop

[English](README.md) | [简体中文](README.zh-CN.md)

一个给 Claude Code / Codex 用的技能：把一句大白话的任务，改写成能放心让 AI 自己一轮一轮跑下去的循环（loop）任务说明。

## 适合谁

已经在用 Claude Code 或 Codex，希望 agent 自己迭代一项任务、不必每一轮都人盯着——打磨一个产品、试玩一个游戏、审一批改动再修——但又不想自己写循环任务说明的人。

## 为什么现在就该试

只说「一直改到好」会留下这些常见的坑：目标没法检查（「更好看」）、所谓「试一遍」其实只读了代码、「过」了却没有证据、没有记录、什么时候停由模型自己说了算。[references/examples.md](references/examples.md) 里有一份改写前后对照：把一个双人横版闯关游戏的 loop 补全，正好堵住这些坑。

这个技能会替你补上：目标要能检查；「用」要真的操作，不能只从代码推断；每一项「过」都要有证据；主观质量用统一的尺子做独立评审；定期把前后对比给人本人看；刹车写全（做完了、卡住了、没进展、来回摆、上限、暂停），并按「有人 / 无人值守」决定是停下来问，还是绕过去接着跑。花钱和不可逆的事写进红线。默认只把任务写成 loop，不开始执行；只有你明确要求现在就在这次对话里执行，并先确认范围，才动手。

```bash
git clone --branch v1.0.1 https://github.com/Ekko237/make-loop.git && cd make-loop && bash install.sh
```

[范例](references/examples.md) · [模板](references/template.md) · [自查清单](references/checklist.md) · [反馈 / 提 issue](https://github.com/Ekko237/make-loop/issues) · [ClawHub](https://clawhub.ai/ekko237/skills/make-loop)

觉得有用请点右上角 ★ Star，能让更多人搜到。

## 运行要求

需要已经安装 Claude Code 和/或 Codex，也就是本机已有 `~/.claude` 或 `~/.codex`。安装脚本是 bash，并且会调用 `rsync`，所以还需要类 Unix 的 shell（macOS、Linux 或同等环境），并且 `PATH` 里有 `git`、`bash` 和 `rsync`。本仓库没有文档说明如何在 Windows 上安装。

`bash install.sh` 会自动检测两种工具，找到哪个就装到哪个。`bash install.sh claude` 或 `bash install.sh codex` 只装其中一个。如果目标目录里已经有一个同名 skill、而且不是这个脚本装的，会先整份备份再停下来，不会覆盖；确认要覆盖，在原来的命令后面加 `--force`。如果上一次安装在创建目标目录之后失败了（比如当时没有 `rsync`），下一次运行可能会把那个空目录当成已有的 skill 并停下来；加上 `--force`，或者删掉空的 `~/.codex/skills/make-loop` 或 `~/.claude/skills/make-loop` 后再跑一次安装脚本。如果 `~/.claude` 和 `~/.codex` 都不存在，脚本会退出，并提示你先安装 Claude Code 或 Codex。

当前版本是 [v1.0.1](https://github.com/Ekko237/make-loop/releases/tag/v1.0.1)。

## 快速使用

装好后，打开 Claude Code 或 Codex，说「帮我变成 loop」或 "turn this into a loop"，或者输入 `/make-loop` 加上你的任务。文档里写明的触发语还包括：「做成循环」「写个 loop」「让它自己一轮一轮改到好」「反复迭代直到没问题」「让它一直跑」。你应该得到一份完整的循环任务说明，放在一个代码块里，方便复制到新对话；下面还会用几句话说明它补了哪些默认值、两个开关选了什么（有人 / 无人值守；批量 / 小步），以及最关键的刹车。除非你明确要求现在就在这次对话里执行，并先确认范围，否则它不会开始跑这个 loop。

一个最小的例子：

```
/make-loop 做一页个人站。要能在手机上用，带上我的名字和一段短介绍，打开就能看，不需要构建步骤。
```

一份完整的改写前后对照（双人横版闯关游戏）见 [references/examples.md](references/examples.md)。填写模板见 [references/template.md](references/template.md)。交付前自查清单见 [references/checklist.md](references/checklist.md)。

## 什么时候别用

- 不适合没有迭代的一次性任务（改一处、答一句）。这个技能写的是 loop：目标、每轮过程、检查、记录和刹车。
- 它不提供任务本身的专业知识。那部分来自当时的对话和你的其他技能。
- 默认只交付说明。如果你需要 agent 在同一轮里直接动手、而且不要先写出 loop，这就不是合适的工具——除非你同时也明确要求按说明执行，并确认范围。
- 安装脚本需要已经有 Claude Code 或 Codex（`~/.claude` 或 `~/.codex`）。两个目录都不存在时，脚本会退出。

## 和其他做法比

| | 自己写 loop 说明 | 只说「一直改到好」 | make-loop |
| --- | --- | --- | --- |
| 怎么开始 | 你已经知道怎么写目标、证据、评审和刹车 | 跟 agent 说一句 | 装好技能，然后说「帮我变成 loop」，或用 `/make-loop` 加上任务 |
| 你会得到什么 | 取决于你能想到写进去的部分 | 一句由模型自己理解的宽松指令 | 按[模板](references/template.md)写出的完整说明：三段（目标、过程、检查迭代）写全五件事（目标是什么、每轮做什么、怎么检查、记录什么、什么时候停），并对照[自查清单](references/checklist.md)补齐 |
| 常见的坑 | 容易漏掉证据、真实使用、记录或上限 | 就像 [examples.md](references/examples.md) 里改写前的例子：「直到没有更多意见」容易第 2 轮就收工；「关卡设计是否科学」没法检查；「试一遍」可能只读了代码 | 会补上：目标能检查、真的用并附证据、记录写进文件、六种刹车、按有人 / 无人值守处理，以及花钱和不可逆的事的红线 |
| 会不会直接开工 | 由你决定 | 模型常常马上开始做 | 默认只交付说明。只有你明确要求现在就在这次对话里执行，并先确认范围，才开始跑 |

## 说明里有什么

一个循环就三段，每段里要写清楚的事细化开是五件：

| 三段 | 五件事 | 一句话 |
| --- | --- | --- |
| **目标**：我要做什么 | 1. 目标是什么 | 做成什么样算好，必须能检查 |
| **过程**：它要怎么做 | 2. 每轮做什么 | 先做第一版，然后每轮固定几个动作 |
| **检查迭代** | 3. 怎么检查 · 4. 记录什么 · 5. 什么时候停 | 拿证据检查、留记录、有刹车 |

先定两个开关（从你的话里判断，判断不了就用默认值）：**有人 / 无人值守**（默认无人值守：卡住了先搁置、换别的做，只有截止、你叫停、或救不回来才停）和 **批量 / 小步**（有评审的任务默认批量：评审一次给全清单，开发一口气改完再评）。

完整说明放在一个代码块里，方便一键复制到新对话。如果任务在某个项目文件夹里，也可以同时存一份到项目里（例如 `LOOP_BRIEF.md`），并且不要提交到公开仓库。代码块下面用三五行说明：补了哪些默认值、两个开关选了什么、最关键的刹车是什么。任务本身的专业知识仍然来自当时的对话和你的其他技能。

## 社区

用过了？[开一个 issue](https://github.com/Ekko237/make-loop/issues)，告诉我哪里坏了，或者你做出了什么。欢迎提 issue 和 pull request。

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/github/license/Ekko237/make-loop?style=flat&color=24292f" alt="MIT license" /></a>
  &nbsp;
  <a href="https://github.com/Ekko237/make-loop/releases/tag/v1.0.1"><img src="https://img.shields.io/github/v/tag/Ekko237/make-loop?style=flat&color=24292f" alt="Latest tag v1.0.1" /></a>
</p>

MIT，见 [LICENSE](LICENSE)。
