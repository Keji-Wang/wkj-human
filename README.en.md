# WKJ-Human

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[中文](README.md) | English

**A skill for human-reviewing and minimally rewriting AI-flavored business writing.** It identifies the genre first, spots templated phrasing, checks formal documents for consistency and caliber issues, and only then decides whether to touch the text at all. The goal is not "washing out the AI flavor" — it is pulling text back to "how a specific person would say this in a specific situation." It is an editorial-judgment tool, not an AI detector, and makes no claim about beating detectors.

> **Note:** The skill's rules and examples are written in Chinese and tuned for Chinese business writing. The underlying principles (review-first, minimal rewrite, preserve the author's voice) are language-agnostic; English text has not been systematically validated. See [SKILL.md](SKILL.md) for the full specification.

## An example first (fictional demo material)

The following example is excerpted from the [fictional demo file](examples/formal-material-check-demo.md) in this repo: a page-by-page image-generation prompt template with planted numeric conflicts, conclusions outrunning evidence, and editing pollution.

**Original text (excerpt):**

> 【Page 4】Body: 采购团队 2025 年累计可导出 2300+ 条供应商联系记录，审批链路形同虚设。
> Layout hint: 以 300+ 作为本页高亮数字，放大处理。
>
> 【Page 5】Opening: 1继续沿用上一页已经确定的母版风格，不重新发明风格。
>
> 【Page 6】Body: 经抽查，2 个共享账号存在离线批量导出行为，证明该部门存在系统性的数据外泄文化。

**WKJ-Human's review findings (output excerpt):**

> - Page 4 body `2300+` vs layout hint `300+` — the body and the layout hint contradict each other. Not "AI flavor," but conflicting instructions will generate a wrong highlighted number. `consistency risk | high | fix now; verify the correct value against the source file — do not guess`
> - Page 6 `2 个共享账号……证明该部门存在系统性的数据外泄文化` — the evidence is 2 sampled accounts; the conclusion claims a "systemic culture." The conclusion runs far beyond what the evidence supports — a caliber risk in any formal report. `caliber stability | high | tighten`
> - Page 5 opening `1继续沿用上一页已经确定的母版风格` — a stray numbering prefix, most likely paste/edit pollution, not intentional content. `editing pollution | medium | delete the prefix`
>
> Overall: not an "AI-flavor-heavy" sample; the real risk sits in the consistency layer.

**Necessary changes (directions given in the demo):**

- The caliber-risk sentence tightened to: "抽查发现 2 个共享账号存在离线批量导出行为，具体范围需扩大核查后定性"
- Numeric conflict: note that the review **does not guess the correct value** — it only flags "verify against the source file." This is deliberate: no invented numbers just to produce a tidy before/after.

**Deliberately preserved:**

- `这页的表格别做成图片，审阅的人要能直接复制数字` — unpolished, but it is the author's own working constraint, a valid instruction. Not cleaned.
- `形成视觉压强` — an acceptable auxiliary style word inside a well-constrained prompt; it does not replace operational definition.

The full review covering all five planted problems: [examples/formal-material-check-demo.md](examples/formal-material-check-demo.md).

## Why it is not another Humanizer

- **More than a stock-phrase finder.** Judgment is based on function and authorship, not a word blacklist — in the same prompt template, "这页的表格别做成图片，审阅的人要能直接复制数字" is unpolished but explicitly preserved, because it is the author's own working constraint.
- **It checks numeric and caliber conflicts.** The `2300+ / 300+` clash and "2 shared accounts" inflated into a "systemic culture" above are exactly this layer's hits — for formal documents, these outrank any style issue.
- **It protects the author's rough edges.** "我其实到现在也没完全想通这件事到底算不算好消息……我心里还是会硌一下" — unpolished, but it is human hesitation, not a defect to clean ([more examples](references/examples.md)).

## Getting started

**Install** (tested entry point):

```bash
npx skills add Keji-Wang/wkj-human
```

Tested: running it in a project directory installs all 15 files (SKILL.md + references/ + examples/ + evals/) and automatically links them for Claude Code.

**Try it** (just say):

```text
Check whether this email reads too much like AI. Don't change it yet.
Rewrite this report draft so it sounds like me talking — don't turn it into another kind of AI.
Review this image-generation prompt template — is the wording consistent throughout?
```

**Manual install (universal):**

```bash
git clone https://github.com/Keji-Wang/wkj-human.git
mkdir -p ~/.claude/skills
cp -r wkj-human ~/.claude/skills/WKJ-Human
```

## Understand it in 30 seconds

- **Input:** a piece of AI-generated or AI-suspected Chinese business writing — business emails, meeting notes, reports/proposals, slide drafts (including page-by-page image-generation prompt templates), WeChat-official-account articles.
- **What it does:** identifies the genre → diagnoses item by item in reading order (templated phrasing / author's hand / numeric & caliber conflicts / editing pollution) → decides whether to touch the text based on your request.
- **Output** (three modes, chosen automatically):

| Mode | You say | You get |
|---|---|---|
| A · Review | "Check where it reads like AI — don't change anything" | Genre judgment + item-by-item findings (quotes, type, severity) |
| B · Review + direction | "Show me how to fix it, don't rewrite it" | Item-by-item advice + which authorial marks to keep |
| C · Minimal rewrite | "Fix it directly, make it sound like me" | Rewritten version + rationale + what was deliberately untouched |

## Core principles

1. **Review first, rewrite last.** Judge where it reads like AI, why, and whether it's worth changing — before any rewrite.
2. **Tell template-speak from the author's hand.** Colloquialisms, pauses, slight repetition, unresolved hesitation are treated as "evidence of a human" to preserve, not defects to clean.
3. **Minimal necessary rewriting.** Delete a word rather than rephrase a sentence; rephrase a sentence rather than rewrite a paragraph. Never invent stories, emotions, times, scenes, or stances.
4. **Formal-document QA (the differentiator).** For emails, reports, and prompt templates it also checks: numbers/titles/layout hints that contradict each other, conclusions outrunning evidence, stray numbering and paste pollution — and treats these as higher priority than any style issue.
5. **It stays in its lane.** When the real problem is thin content, weak logic, or questionable facts, it says so instead of polishing over them.

## What it does not do

- **It is not an AI detector.** "AI flavor" is editorial judgment; it does not claim to identify AI-written text, nor to beat any detector.
- **It does not verify facts.** It checks expression, caliber, and consistency — not whether numbers and facts are true.
- **It does not promise "better".** The goal is "more like the author," not "prettier."
- **It is not a general polish tool.** If you want "fancier, more press-release, more brand-voice," it will tell you that's not its home turf.

## Repository layout

```
wkj-human/
├── SKILL.md                  # Core spec: positioning, workflow, three output modes, rewrite boundaries
├── references/
│   ├── ai-signals.md         # Signal library: 24 AI-writing signals incl. "preserve-the-human" signals
│   ├── scene-adaptation.md   # Genre-specific adaptation rules (6 genres)
│   ├── rewrite-boundaries.md # What to change, what to treat carefully, what never to touch
│   ├── formal-material-checks.md  # Formal-document QA layer (consistency / caliber / pollution)
│   └── examples.md           # Worked judgment examples
├── examples/
│   └── formal-material-check-demo.md  # Fictional demo of the formal-document QA layer
├── evals/evals.json          # 9 design test cases (all fictional material)
├── docs/validation.md        # Validation log (honest: structure checks / live trials / human review)
├── SOURCES.md                # Provenance, upstream licenses, and attribution (important)
├── README.md / README.en.md  # Bilingual readme
├── CHANGELOG.md
├── CONTRIBUTING.md
└── LICENSE
```

## Notes on other runtimes and tooling

- For other runtimes that follow the Agent Skills convention, place the same content according to that runtime's own skill-directory convention — **compatibility is not claimed for any platform where this has not been tested**.
- If you use a skill manager such as [cc-switch](https://github.com/farion1231/cc-switch), you can install from this repository directly. **cc-switch is optional; using this skill does not require adopting the author's local setup.**

## Relationship to upstream projects

WKJ-Human is not presented as fully original: it synthesizes ideas from three open-source skills — [Humanizer-zh](https://github.com/op7418/Humanizer-zh) (MIT), [dbs-ai-check](https://github.com/dontbesilent2025/dbskill) (CC BY-NC 4.0, ideas only), and [renwei-writing](https://github.com/orange2ai/renwei-writing) (custom open-source license) — and iterated its own differentiating layers (genre adaptation + formal-document QA) through real client delivery work. The per-item boundary between inspiration and original content, and the license verification process, are documented in [SOURCES.md](SOURCES.md).

## Sister project

A sister project by the same author: [wkj-insight-distiller](https://github.com/Keji-Wang/wkj-insight-distiller) — an Agent Skill that turns interview and meeting material into insight memos; it can serve as upstream material preparation for this skill.

## Validation status and limitations

- Current version **v0.3.1**: structural verification done, live trials passed, author-reviewed. **Pre-1.0; not claimed to be mature or stable.** Details and limitations in [docs/validation.md](docs/validation.md).
- Rules are tuned for Chinese business writing; English text has not been systematically validated.
- Check density on very long documents (10k+ characters) is not yet validated.
- Released as-is; issues are read but response times are not guaranteed.

## Acknowledgments

This skill did not start from zero — it stands on the shoulders of several open-source projects. Sincere thanks to their authors:

- **歸藏 (op7418)**, [Humanizer-zh](https://github.com/op7418/Humanizer-zh) (MIT) — the organization of this project's signal library was directly inspired by it, and it brought the "signs of AI writing" conversation to the Chinese community.
- **dontbesilent**, [dbs-ai-check](https://github.com/dontbesilent2025/dbskill) (CC BY-NC 4.0) — the diagnostic philosophy of "diagnose by default, never auto-rewrite" and "the essence of AI flavor is excessive uniformity" shaped this skill's review-first backbone.
- **橘子 (Orange) & Cola**, [renwei-writing](https://github.com/orange2ai/renwei-writing) — the preserve-the-human principles ("treat rough edges as the author's hand first," "the ceiling of editing is invisibility") are the parts this project treasures most and hopes to pass on.
- **blader**, [humanizer](https://github.com/blader/humanizer) (MIT), and Wikipedia's ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) — the common root of the whole "signs of AI writing" knowledge line.
- **Daniel Miessler**, [PAI](https://github.com/danielmiessler/Personal_AI_Infrastructure) — the inspiration behind the author's local skill-system organization.

That is what open source is for: every layer of improvement stands on thinking someone else published. May this repository become the next set of shoulders.

## Contact

- X (Twitter): [@JiafuWang](https://x.com/JiafuWang)
- Email：[keji.dev@outlook.com](mailto:keji.dev@outlook.com)
- For questions and suggestions, please prefer [opening an issue](https://github.com/Keji-Wang/wkj-human/issues)

## License

MIT — see [LICENSE](LICENSE). Third-party idea licensing boundaries are documented in [SOURCES.md](SOURCES.md).

© 2026 Jeffrey Wang (Keji-Wang)
