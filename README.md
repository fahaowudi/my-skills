# deep-research

A ZCode skill for structured, rigorous research deliverables.

把"深度调研"沉淀成一个可复用的 skill：把一个难的研究问题拆解 → 并行派多个子agent独立调研 → 综合成 MD（强制数据诚实声明）→ 批判性审查（默认自检，高风险升级为独立对抗审查）→ 输出 MD + 设计精良的 HTML。

## 安装

把这个仓库 clone 到 ZCode 的个人技能目录：

```bash
git clone git@github.com:fahaowudi/research-skill.git ~/Desktop/research-skill
# 然后把 deep-research 文件夹放到技能目录：
# Windows: C:\Users\<你>\.zcode\skills\deep-research\
# macOS/Linux: ~/.zcode/skills/deep-research/
```

或者直接把 `deep-research/` 整个文件夹复制到 `.zcode/skills/` 下即可。

## 结构

```
deep-research/
├── SKILL.md                          # 主流程：5个阶段的调研流水线
└── references/
    ├── design-reference.md           # HTML默认设计语言（大地色系，按主题调整）
    └── adversarial-review-prompt.md  # 独立对抗审查的prompt模板
```

## 触发方式

在 ZCode 里对模型说"调研/研究/深度分析/对比分析/策略"等，且问题复杂到值得一份报告时，skill 会自动触发。也可以用 `/skill deep-research` 强制加载。

## 流程概览

1. **对齐范围** — 用 AskUserQuestion 批量问边界/目的/深度/约束
2. **并行调研** — 按复杂度派 2-5 个子agent（MECE 切分维度），各 prompt 自包含、互不见
3. **综合成 MD** — TL;DR 开头、解决矛盾、强制数据诚实声明
4. **批判审查** — 默认自检；综合后询问是否升级对抗审查（高风险才主动推荐）
5. **输出** — 默认 MD + HTML（带 signature 设计元素，浏览器验证渲染）

详细说明见 `deep-research/SKILL.md`。

## 许可

个人使用。
