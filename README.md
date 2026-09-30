# WKJ-Human

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

中文 | [English](README.en.md)

**面向商业写作的人味审阅与最小化改写 skill。** 先判断体裁、识别套路化表达，再检查正式材料中的口径与一致性问题，最后才决定要不要动文字——目标不是“洗掉 AI 味”，而是把文本拉回“像一个具体的人在具体场景下说话”。它是编辑判断工具，不是 AI 检测器，也不承诺绕过任何检测。

> 想直接上手：跳到[快速开始](#快速开始)——一条命令安装，附三个可直接复制的试用句。

## 先看一个例子（虚构演示材料）

以下例子摘自仓库内的[虚构演示文件](examples/formal-material-check-demo.md)：一份逐页生图提示词母稿，故意埋入了数字冲突、口径抢跑和编辑污染。

**原文（节选）：**

> 【第 4 页】正文：采购团队 2025 年累计可导出 2300+ 条供应商联系记录，审批链路形同虚设。
> 版式建议：以 300+ 作为本页高亮数字，放大处理。
>
> 【第 5 页】开头：1继续沿用上一页已经确定的母版风格，不重新发明风格。
>
> 【第 6 页】正文：经抽查，2 个共享账号存在离线批量导出行为，证明该部门存在系统性的数据外泄文化。

**WKJ-Human 审阅判断（输出摘录）：**

> - 第 4 页正文 `2300+` vs 版式建议 `300+`——正文与版式提示数字冲突。这不是 AI 味，但两页指令打架会让高亮数字直接生成错误事实。`一致性风险 | 高风险 | 建议立即修改，正确值需回溯源文件确认，不要猜`
> - 第 6 页 `2 个共享账号……证明该部门存在系统性的数据外泄文化`——证据是 2 个抽查样本，结论却是“系统性文化”。结论强度远超证据支持范围，在正式汇报里属于口径抢跑，被追问时最伤可信度。`口径稳定性 | 高风险 | 建议收紧`
> - 第 5 页开头 `1继续沿用上一页已经确定的母版风格`——残留编号前缀，大概率是编辑或粘贴过程带入的污染，不是你有意写的内容。`编辑污染 | 中风险 | 建议删除前缀字符`
>
> 总体判断：这不是“AI 味重”的样本，主要风险在一致性层。

**必要修改（演示文件中给出的方向）：**

- 口径抢跑句收紧为：“抽查发现 2 个共享账号存在离线批量导出行为，具体范围需扩大核查后定性”
- 数字冲突：注意审阅输出**不替你猜正确值**，只标注“需回溯源文件确认”——这是有意设计，不为凑出漂亮的 Before/After 而编造一个数字。

**刻意保留的表达：**

- `这页的表格别做成图片，审阅的人要能直接复制数字`——不工整，但这是作者自己的判断和约束，是有效指令，不清洗。
- `形成视觉压强`——在有结构约束的提示词里属于可接受的辅助风格词，没有替代操作定义。

完整五类问题的审阅输出见 [examples/formal-material-check-demo.md](examples/formal-material-check-demo.md)。

## 为什么不是普通 Humanizer

- **不只查套话。** 判断依据是功能与手迹，不是词表匹配——同一页里“这页的表格别做成图片，审阅的人要能直接复制数字”并不工整，但因为它是作者有效的判断和约束，被明确保留。
- **会查数字与口径冲突。** 上面的 `2300+ / 300+` 和“2 个共享账号”推成“系统性文化”就是这类命中——对正式材料，这类问题的优先级高于一切文风判断。
- **会保护人的毛边。** “我其实到现在也没完全想通这件事到底算不算好消息……我心里还是会硌一下”——不工整，但这是人的犹豫，不是待清洗的缺陷（[更多示例](references/examples.md)）。

## 快速开始

**安装**（已实测的第一入口）：

```bash
npx skills add Keji-Wang/wkj-human
```

实测记录：在项目目录运行，完整装入 15 个文件（SKILL.md + references/ + examples/ + evals/），并自动为 Claude Code 建立符号链接。

**试用**（装好后直接说）：

```text
帮我看下这封邮件是不是太 AI 了，先别改
这段汇报底稿改得像我讲的话，别改成另一种 AI
帮我审下这个生图提示词母稿，口径稳不稳
```

**手动安装（通用方式）：**

```bash
git clone https://github.com/Keji-Wang/wkj-human.git
mkdir -p ~/.claude/skills
cp -r wkj-human ~/.claude/skills/WKJ-Human
```

## 30 秒理解

- **输入**：一段 AI 生成或被怀疑“AI 味重”的中文商业文本——商业邮件、会议纪要、报告/方案、PPT 底稿（含逐页生图提示词母稿）、公众号文章。
- **它做什么**：判断体裁 → 按原文顺序逐处诊断（套路化表达 / 作者手迹 / 数字与口径冲突 / 编辑污染）→ 按你的要求决定动不动。
- **输出**（三种模式，按请求自动选择）：

| 模式 | 你说 | 你拿到 |
|---|---|---|
| A · 审阅 | “先别改，帮我看看哪里像 AI” | 体裁判断 + 逐处问题定位（引用原文、类型、严重度） |
| B · 审阅 + 修改方向 | “给我改法，别替我全写” | 逐处建议 + 修改方向 + 应保留的手迹 |
| C · 审阅 + 最小化改写 | “直接帮我改，改得像我写的” | 优化后版本 + 改动说明 + 刻意没动的地方 |

## 核心特点

1. **默认先审，不先改。** 先判断哪里像 AI、为什么像、值不值得改，再决定是否进入改写。
2. **区分套路与手迹。** 口语、停顿、轻微重复、没修圆的犹豫，先当作“人的证据”保留，而不是待清洗的缺陷。
3. **最小必要改写。** 能删词不换句，能换句不改段；不补造故事、情绪、时间、场景、立场。
4. **正式材料质检（差异化能力）。** 对邮件、汇报、提示词母稿这类文本，同时检查：数字/标题/版式口径前后打架、结论抢跑证据（过度定性）、编辑带入的残留编号和污染字符——并且这类问题的优先级高于一切文风判断。
5. **不越界。** 内容本身空洞、逻辑错误、事实存疑时，直接说问题不在 AI 味，不用润色掩盖。

## 它不做什么（能力边界）

- **不是 AI 检测器。**“AI 味”是编辑判断，不声称能鉴定文本是否由 AI 写成，也不承诺绕过任何检测工具。
- **不验证事实。** 检查表达、口径、一致性，不核实数字与事实本身的真实性。
- **不承诺“改完更好”。** 目标是改完更像作者，不是改完更漂亮。
- **不是通用润色器。** 你要“更华丽、更媒体稿、更品牌腔”时，它会直说这不是它的主场。

## 文件结构

```
wkj-human/
├── SKILL.md                  # 主规则：定位、工作流、三种输出模式、改写边界
├── references/
│   ├── ai-signals.md         # AI 痕迹信号库（24 类，含保人信号）
│   ├── scene-adaptation.md   # 六种体裁的适配规则
│   ├── rewrite-boundaries.md # 改写边界：能改/谨慎/默认不做
│   ├── formal-material-checks.md  # 正式材料质检层（一致性/口径/污染）
│   └── examples.md           # 判断方式示例
├── examples/
│   └── formal-material-check-demo.md  # 正式材料质检虚构演示
├── evals/evals.json          # 9 个设计用例（全部虚构材料）
├── docs/validation.md        # 验证记录（如实，区分结构检查/试跑/人工评阅）
├── SOURCES.md                # 来源核实与致谢（重要）
├── README.md / README.en.md  # 中英双语说明
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

## 安装备注（其他运行时与工具链）

- 对其他兼容 Agent Skills 约定的运行时（如 Codex 类工具），按该运行时自己的 skill 目录约定放置同样内容即可——**未在某个平台上实测过，就不声称兼容该平台**。
- 如果你在用 [cc-switch](https://github.com/farion1231/cc-switch) 之类的 skill 管理器，也可以从本仓库安装。**cc-switch 是可选工具，使用本 skill 不需要采用作者的任何本地环境。**

## 与上游的关系

WKJ-Human 不是完全原创：它综合了 [Humanizer-zh](https://github.com/op7418/Humanizer-zh)（MIT）、
[dbs-ai-check](https://github.com/dontbesilent2025/dbskill)（CC BY-NC 4.0，仅理念启发）、
[renwei-writing](https://github.com/orange2ai/renwei-writing)（自定义开源许可）三个开源 skill
的理念，并在真实商业交付中迭代出自己的差异化层（场景适配 + 正式材料质检）。
逐项的借鉴/原创边界、许可证核实过程见 [SOURCES.md](SOURCES.md)。

## 姊妹项目

同一作者的姊妹项目：[wkj-insight-distiller](https://github.com/Keji-Wang/wkj-insight-distiller)——访谈/会议材料 → 洞察备忘录的 Agent Skill，可作为本 skill 的上游素材整理。

## 验证状态与限制

- 当前版本 **v0.3.1**：结构验证完成 + 执行试跑通过 + 作者自查。**非 1.0，不宣称成熟稳定**，验证细节与局限见 [docs/validation.md](docs/validation.md)。
- 规则为中文商业写作调校；英文文本未系统验证。
- 长文（万字级）下的检查密度未充分验证。
- 本 skill 按 “as-is” 释出，不承诺支持响应时效，Issue 会看但不保证修复节奏。

## 致谢与致敬

这个 skill 不是从零开始的，它站在几个开源项目的肩膀上。向这些作者致以真实的感谢：

- **歸藏（op7418）** 的 [Humanizer-zh](https://github.com/op7418/Humanizer-zh)（MIT）——本项目信号库的组织方式直接受它启发，它也把“AI 写作痕迹”这个话题带进了中文社区。
- **dontbesilent** 的 [dbs-ai-check](https://github.com/dontbesilent2025/dbskill)（CC BY-NC 4.0）——“默认只诊断不改写”和“AI 味的本质是过度均匀”这两条诊断哲学，塑造了本 skill“先审后改”的骨架。
- **橘子（Orange）与 Cola** 的 [renwei-writing](https://github.com/orange2ai/renwei-writing)——“毛边先假设是手迹”“改写的上限是隐形”这些保人原则，是本项目最珍视、也最希望传下去的部分。
- **blader** 的 [humanizer](https://github.com/blader/humanizer)（MIT）与 Wikipedia 的 ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)——整个“AI 写作特征”知识脉络的公共源头。
- **Daniel Miessler** 的 [PAI](https://github.com/danielmiessler/Personal_AI_Infrastructure)——作者本地 skill 体系的组织灵感来源。

开源的意义就在于此：每一层改进都踩在前人公开的思考上。愿这个仓库也能成为下一层肩膀。

## 联系

- X（Twitter）：[@JiafuWang](https://x.com/JiafuWang)
- 邮箱：[keji.dev@outlook.com](mailto:keji.dev@outlook.com)
- 问题与建议优先走 [Issue](https://github.com/Keji-Wang/wkj-human/issues)

## License

MIT，见 [LICENSE](LICENSE)。第三方理念的许可边界见 [SOURCES.md](SOURCES.md)。

© 2026 Jeffrey Wang (Keji-Wang)
