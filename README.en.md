# WKJ-Human

[中文](README.md) · **English**

**A skill for human-reviewing and minimally rewriting AI-flavored business writing.** It identifies the genre first, spots templated phrasing, checks formal documents for consistency and caliber issues, and only then decides whether to touch the text at all. The goal is not "washing out the AI flavor" — it is pulling text back to "how a specific person would say this in a specific situation."

> **Note:** The skill's rules and examples are written in Chinese and tuned for Chinese business writing. The underlying principles (review-first, minimal rewrite, preserve the author's voice) are language-agnostic; English text has not been systematically validated. See [SKILL.md](SKILL.md) for the full specification.

---

## What it does

Five typical genres: business emails, meeting notes, reports/proposals, slide drafts (including page-by-page image-generation prompt templates), and WeChat-official-account articles.

Three working modes, chosen automatically based on your request:

| Mode | You say | Output |
|---|---|---|
| A · Review | "Check if this reads like AI — don't change anything yet" | Genre judgment + item-by-item findings (quoted text, type, severity, whether a change is advised) |
| B · Review + direction | "Show me how to fix it, don't rewrite it for me" | Item-by-item advice + rewrite direction + which authorial marks to keep |
| C · Review + minimal rewrite | "Fix it directly, make it sound like me" | Rewritten version + rationale + what was deliberately left untouched |

## Core principles

1. **Review first, rewrite last.** Judge where it reads like AI, why, and whether it's worth changing — before any rewrite.
2. **Tell template-speak from the author's hand.** Colloquialisms, pauses, slight repetition, unresolved hesitation are treated as "evidence of a human" to preserve, not defects to clean.
3. **Minimal necessary rewriting.** Delete a word rather than rephrase a sentence; rephrase a sentence rather than rewrite a paragraph. Never invent stories, emotions, times, scenes, or stances.
4. **Formal-document QA (the differentiator).** For emails, reports, and prompt templates it also checks: numbers/titles/layout hints that contradict each other, conclusions outrunning evidence, stray numbering and paste pollution left by editing — and treats these as higher priority than any style issue.
5. **It stays in its lane.** When the real problem is thin content, weak logic, or questionable facts, it says so instead of polishing over them.

## What it does not do

- **It is not an AI detector.** "AI flavor" is editorial judgment; it does not claim to identify AI-written text, nor to beat any detector.
- **It does not verify facts.** It checks expression, caliber, and consistency — not whether numbers and facts are true.
- **It does not promise "better".** The goal is "more like the author," not "prettier."
- **It is not a general polish tool.** If you want "fancier, more press-release, more brand-voice," it will tell you that's not its home turf.

## Getting started

This is a pure-Markdown Agent Skill (Claude Code Skill format): no scripts, no dependencies, no build step.

### Install manually (universal)

Put the folder into your runtime's skill directory, e.g. for Claude Code:

```bash
git clone https://github.com/Keji-Wang/wkj-human.git
mkdir -p ~/.claude/skills
cp -r wkj-human ~/.claude/skills/WKJ-Human
```

Restart the session afterwards. For other runtimes that follow the Agent Skills convention, place the same content according to that runtime's own skill-directory convention — **compatibility is not claimed for any platform where this has not been tested**.

### Install via a skill manager (optional)

If you use a skill manager such as [cc-switch](https://github.com/farion1231/cc-switch), you can install from this repository directly. **cc-switch is optional; using this skill does not require adopting the author's local setup.**

### Try it

- "Check whether this email reads too much like AI. Don't change it yet."
- "Rewrite this report draft so it sounds like me talking — don't turn it into another kind of AI."
- "Review this image-generation prompt template — is the wording consistent throughout?"

## A 30-second example

**Input (Mode A · Review):**

> 会议围绕组织协同、流程优化与资源配置展开了深入探讨，并在多个关键议题上形成初步共识，为后续工作的高效推进提供了有力支撑。

**WKJ-Human's judgment (excerpt):**

- "深入探讨" "形成初步共识" "高效推进" "有力支撑" are stock meeting-note filler (`AI 自动升华 | high risk`)
- Meeting notes are about facts, disagreements, and open items — not uplift; if nothing was actually decided, write "to be confirmed"

More examples: [references/examples.md](references/examples.md). A full worked demo of the formal-document QA layer (contradictory numbers, stray prefixes, conclusions outrunning evidence) — all fictional, clearly labeled: [examples/formal-material-check-demo.md](examples/formal-material-check-demo.md).

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

## Relationship to upstream projects

WKJ-Human is not presented as fully original: it synthesizes ideas from three open-source skills — [Humanizer-zh](https://github.com/op7418/Humanizer-zh) (MIT), [dbs-ai-check](https://github.com/dontbesilent2025/dbskill) (CC BY-NC 4.0, ideas only), and [renwei-writing](https://github.com/orange2ai/renwei-writing) (custom open-source license) — and iterated its own differentiating layers (genre adaptation + formal-document QA) through real client delivery work. The per-item boundary between inspiration and original content, and the license verification process, are documented in [SOURCES.md](SOURCES.md).

## Validation status and limitations

- Current version **v0.3.1**: structural verification done, live trials passed, author-reviewed. **Pre-1.0; not claimed to be mature or stable.** Details and limitations in [docs/validation.md](docs/validation.md).
- Rules are tuned for Chinese business writing; English text has not been systematically validated.
- Check density on very long documents (10k+ characters) is not yet validated.
- Released as-is; issues are read but response times are not guaranteed.

## License

MIT — see [LICENSE](LICENSE). Third-party idea licensing boundaries are documented in [SOURCES.md](SOURCES.md).

## Acknowledgments

This skill did not start from zero — it stands on the shoulders of several open-source projects. Sincere thanks to their authors:

- **歸藏 (op7418)**, [Humanizer-zh](https://github.com/op7418/Humanizer-zh) (MIT) — the organization of this project's signal library was directly inspired by it, and it brought the "signs of AI writing" conversation to the Chinese community.
- **dontbesilent**, [dbs-ai-check](https://github.com/dontbesilent2025/dbskill) (CC BY-NC 4.0) — the diagnostic philosophy of "diagnose by default, never auto-rewrite" and "the essence of AI flavor is excessive uniformity" shaped this skill's review-first backbone.
- **橘子 (Orange) & Cola**, [renwei-writing](https://github.com/orange2ai/renwei-writing) — the preserve-the-human principles ("treat rough edges as the author's hand first," "the ceiling of editing is invisibility") are the parts this project treasures most and hopes to pass on.
- **blader**, [humanizer](https://github.com/blader/humanizer) (MIT), and Wikipedia's ["Signs of AI writing"](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) — the common root of the whole "signs of AI writing" knowledge line.
- **Daniel Miessler**, [PAI](https://github.com/danielmiessler/Personal_AI_Infrastructure) — the inspiration behind the author's local skill-system organization.

That is what open source is for: every layer of improvement stands on thinking someone else published. May this repository become the next set of shoulders.
