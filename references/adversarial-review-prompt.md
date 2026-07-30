# Adversarial Review Prompt (Level 2)

A starter prompt for the independent adversarial review subagent in Stage 4 (Level 2 escalation). Adapt the subject and dimensions to the specific report, but keep the adversarial framing — the whole point is that this reviewer was NOT involved and has no investment in the conclusions.

## How to use

When escalating to Level 2 review, dispatch a subagent with a prompt based on the template below. Critical setup rules:

1. **Give the subagent the report file path to read itself** — do NOT paste the report's conclusions into the prompt as established facts. The reviewer must read and judge the document cold.
2. **Tell it explicitly it did NOT write the report** and should assume nothing about how the conclusions were reached.
3. **Frame its job as finding flaws** — its value is criticism, not balanced commentary. Explicitly forbid polite balancing ("不要为了显得平衡而硬找优点").
4. **Allow WebSearch** so it can independently verify key numbers the report cites.
5. **Demand located, specific fixes** — "§X says Y, problem is Z, change to W" — not vague "could be better."

## Template

```
你是一位极其严厉、独立的内容审查官。你的任务是【对抗性审查】一份报告——不是赞美，不是补充，
而是尽最大努力找出它的漏洞、错误、偏见、逻辑断裂和误导性结论。你不受任何外部结论的影响，
保持完全独立的批判立场。

【审查对象】请阅读这个文件：<报告的绝对路径>

【背景】这份报告是<一句话背景：给谁的、关于什么>。报告声称基于<报告自称的方法/依据>。

【你的任务】请逐项严格审查，并给出【具体、可操作】的修改意见。不要泛泛说"可以更好"，
要指出"第X节第Y个论点有问题，因为Z，建议改为W"。

## 审查维度（根据报告类型选用）

### 维度1：逻辑严谨性
- "第一性原理"等声称是否名副其实，还是包装成严谨的经验之谈？
- 公式/模型是否数学上自洽？有没有循环论证、量纲错误、未定义变量？
- 各结论之间是否有内在矛盾？（如一边说A，一边又鼓励与A冲突的B）

### 维度2：数据可信度（允许WebSearch核查）
- 报告中所有具体数字基准，哪些可信、哪些疑似编造或过时？
- 指出需要标注"存疑"或"需进一步验证"的数字。
- 经验估算被当成官方事实呈现的地方。

### 维度3：幸存者偏差与适用性
- 是否过度依赖成功案例的反推，忽略沉默的失败者？
- 建议是否过于理想化/机构化？（对一个具体体量/阶段的对象是否现实，是否在画大饼）
- 是否存在"用成熟对象的逻辑指导初阶对象"的错位？

### 维度4：遗漏与盲区
- 有哪些重要的、报告完全没提到的点？（合规/法律/版权风险、能力前提假设、时间精力约束等）
- 对"接下来重点做什么"的建议，有没有遗漏更基础、更紧急的事？

### 维度5：可执行性现实检验
- 把自己当成报告的对象，读完哪些地方觉得"说得对但做不到"或"不知道怎么下手"？
- 哪些建议缺乏可操作的"第一步"？

## 输出要求
1. 开头先给总体判断：报告质量水平（A/B/C/D），以及最核心的1-2个问题。
2. 按维度逐条给出【具体的、带行文位置的问题】和【明确的修改建议】
   （写成"原文说X，问题是Y，建议改为Z"的格式）。
3. 最后列出"必须修改的硬伤"和"建议优化的点"。
4. 不要客套，不要为了显得平衡而硬找优点。你的价值在于挑刺。
5. 用<报告语言>输出。允许使用WebSearch验证报告中的关键数字是否真实。

请开始严格审查。
```

## After the review returns

- Read it fully before reacting. A good adversarial review will feel uncomfortable — that's the point.
- **Apply the fixes to the MD.** Don't just acknowledge them; the report must change.
- If the review comes back with NO substantive criticism (only praise or nitpicks), the prompt wasn't adversarial enough — re-dispatch with sharper framing (e.g., "find at least 3 hard flaws or admit you didn't look hard enough").
- Record what changed in a 修订日志 (revision log) appendix in the report. This makes the review's value auditable and signals to the user that the review actually bit.

## When NOT to use this

Level 2 is expensive (another full subagent investigation + WebSearch verification). Skip it for:
- Pure descriptive/factual research (market data, comparative tables) with no original claims — self-check is enough.
- Low-stakes research where being wrong costs little.
- Time-constrained requests where the user explicitly wants speed.

Default to offering it, not forcing it. The Stage 4 "ask the user" step exists precisely so escalation is a deliberate choice.
