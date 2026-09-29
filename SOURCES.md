# 来源与致谢（SOURCES）

> 本文件记录 WKJ-Human 的真实来源：哪些是本项目原创，哪些借鉴自上游项目，
> 各上游的许可证是什么、如何核实的。**WKJ-Human 不是完全原创**——它是在几个
> 开源 skill 基础上迭代的产物，本文如实说明边界。

## 一句话结论

WKJ-Human 的**文本表达为独立撰写**（经程序化逐字比对，与上游无成段重合），
**核心概念与工作流借鉴自三个开源 skill**，并在真实商业交付中迭代出了上游没有的
正式材料质检层。发布采用 MIT，同时在本文保留完整致谢与许可说明。

## 上游项目

### 1. Humanizer-zh（op7418 / 歸藏）— MIT

- 仓库：<https://github.com/op7418/Humanizer-zh>
- 许可证：MIT（已核实本地副本 LICENSE 文件与 GitHub 仓库页）
- 借鉴内容（理念层）：AI 痕迹模式库的组织方式——把“不是 X 而是 Y”“意义拔高、
  宣传腔、模糊归因、排比三连”等高频模式作为可检索的信号类别。
- 未沿用内容：WKJ-Human 的信号库（`references/ai-signals.md`）为独立撰写与扩充，
  未复制其条目文本；并新增了中文语感信号、正式材料信号两类上游没有的分组。
- 关系说明：Humanizer-zh 自述基于 blader/humanizer v3.0.0。blader/humanizer 的
  检查脉络可上溯至 Wikipedia "Signs of AI writing"。在此一并致谢。

### 2. dbs-ai-check（dontbesilent / dbskill 仓库）— CC BY-NC 4.0

- 仓库：<https://github.com/dontbesilent2025/dbskill>
- 许可证：CC BY-NC 4.0（个人与非商业用途可自由使用，商用需另行授权）
- 借鉴内容（理念层）：诊断哲学——默认只诊断不改写；“AI 味的本质是过度完美、
  过度均匀、过度光滑”；“去 AI 味不等于内容变好”；按体裁调整判定标准。
- 未沿用内容：未复制任何文本。上述原则在 WKJ-Human 中以独立措辞重写
  （如“默认先审，不先改”“AI 味的强信号是过度均匀，不是单纯书面”）。
  程序化逐字比对（≥8 字连续重合）仅命中一处通用短语（“整体感觉有点 AI”）。
- 许可说明：**本项目未包含 dbskill 仓库的任何文本**，因此不受其 NC 条款约束。
  如未来引入其文本，须遵循 CC BY-NC 4.0。

### 3. renwei-writing（orange2ai / 橘子 Orange & Cola）— 自定义双许可

- 仓库：<https://github.com/orange2ai/renwei-writing>
- 许可证：自定义双许可——开源与个人用途免费（含“以 OSI 认可许可证公开发布的
  项目”）；闭源商业用途需向作者购买商业授权。署名非强制。
- 借鉴内容（理念层 + 少量短语）：保人原则——少动原文只做减法；“毛边先假设是
  手迹，不是瑕疵”；拿不准就白描；“改写的上限是隐形（读者感觉不到编辑来过）”；
  改完只检查动过的地方。
- 表达重合情况：逐字比对命中三处短语级重合（“事情是什么就说什么”“作者自己的
  抽象层级”“希望这对你有帮助”），均为通用短语，无成段复制。
- 许可说明：本项目以开源形式发布，符合其 Free License 的开源项目条款；
  本节即为致谢。**若下游使用者将本项目用于闭源商业交付，涉及上述原则表述时
  请自行确认对 renwei-writing 原许可的义务。**

### 4. 命名说明

本项目的私有前身（2026-06 ~ 09 内部迭代版）名为 **CORE-Human**，“CORE-” 前缀来自作者本地
skill 体系的家族命名习惯，而该习惯受 Daniel Miessler 的
[PAI (Personal AI Infrastructure)](https://github.com/danielmiessler/Personal_AI_Infrastructure)
的 CORE skill 影响。公开发布时（v0.3.1 起）更名为 **WKJ-Human**，改用作者自己的
skill 家族前缀。WKJ-Human 与 PAI 无代码或文本关联。

## 借鉴与原创的边界

| 部分 | 性质 |
|---|---|
| 工作流骨架（体裁判断 → 痕迹诊断 → 判断该不该动 → 最小化改写） | 理念综合自上游三者，表达为本项目原创 |
| AI 信号库（24 类信号） | 类别思路受 humanizer / renwei 检查清单启发，条目为本项目独立撰写与扩充 |
| 场景适配层（邮件/纪要/报告/PPT/提示词/公众号） | **本项目原创**（上游无对应物） |
| 正式材料质检层（数字一致性/口径稳定性/编辑污染） | **本项目原创**，来自真实商业交付迭代（见 CHANGELOG） |
| 三种输出模式（审阅 / 审阅+方向 / 审阅+最小化改写） | 本项目原创 |
| “毛边先假设是手迹”“白描优先”“上限是隐形”等保人原则 | 理念源自 renwei-writing，以本项目措辞重写，个别通用短语重合 |

## 核实方法与可复核性

1. **设计记录**：WKJ-Human 的设计文档（2026-06-17）明确记录了“整合三个 skill
   的长处”及各自贡献的模块，本文据此归类。
2. **时间线**：cc-switch 数据库安装记录显示 dbs-ai-check（2026-06-04）、
   renwei-writing（2026-06-15）、humanizer（2026-05）均先于 WKJ-Human 的
   设计时间（2026-06-17），佐证设计文档的真实性。
3. **逐字比对**：以 ≥8 字连续重合为阈值，对 WKJ-Human 全部正文与三个上游全文
   做程序化 n-gram 扫描，命中仅为本文件所列的通用短语。比对方法简单可复现。
4. **许可证核实**：三个上游的许可证分别通过本地 LICENSE 文件与 GitHub 仓库页
   双重核对（2026-09-29）。

## 无法确认的部分

- WKJ-Human 设计时参考的 humanizer 具体版本/提交无法精确锚定（本地安装于
  2026-05，早于设计时间；后续该 skill 有更新）。本文按“当时安装版本”如实说明。
- 如任何权利人认为本项目存在不当使用，请通过 Issue 联系，我们会尽快处理
  （补充署名、移除内容或调整许可）。

## License 注记

> 2026-09-29 自 LICENSE 文件迁入。原注记追加在 LICENSE 尾部，导致 GitHub 将许可
> 识别为 NOASSERTION；迁出后 LICENSE 恢复为标准 MIT 全文。以下为原注记，一字未改：

Note on third-party ideas: this project's design draws on concepts from
several open-source skills. Their licenses and the exact boundary between
inspiration and original content are documented in SOURCES.md, which is part
of this project's license documentation. In particular, principle phrasing
related to renwei-writing is used under its Free License for open-source
projects; closed-source commercial use of this project may create obligations
toward that author. See SOURCES.md for details.
