# Skill Autoresearch Changelog — martin-fowler-perspective

## 实验设置

**Target**: `SKILL.md`

**Test set (固定)**:
- T1 (技术决策): "我们团队10个人的单体应用开始慢了，想拆微服务，你怎么看？"
- T2 (方法论病理): "我们团队做Scrum一年了，但只是开了很多会，代码质量没提升。怎么办？"
- T3 (新领域审慎): "LLM写的代码要不要单元测试？和人写的代码一样测吗？"

**Eval suite v2 (固定，baseline-v2之后)**:
- E1 Recursive Skepticism: 至少1处元认知自我质疑（"flag as instinct" / 对自己判断的二阶质疑）
- E2 Attribution reflex: 首次提到有原创者的概念时inline归属
- E3 术语密度 ≥2: 至少2个Fowler专属CamelCase术语
- E4 Parenthetical hedge: 至少1处括号内自我对冲
- E5 Inductive opener: 第一句观察式启动

**Runs per experiment**: 2 (每个prompt 2次, 共6个响应, 30个评分点)
**Judging**: 单一评测agent应用binary判断
**Stop rule**: 3个实验上限 OR pass_rate ≥ 95%

---

## Baseline v1 -- discarded (eval重设)

Score: 30/30 (100%)
Targeted failure: 无 — 但评测agent识别出未被eval捕获的真实弱点(Recursive Skepticism缺失、归属习惯缺失、脚注对冲缺失)
Change: n/a (初始状态)
Reasoning: 发现eval过于宽松，不discriminating
Result: 决定收紧eval suite，重启实验

## Baseline v2 -- baseline

Score: 29/30 (96.7%)
E1: 6/6 | E2: 6/6 | E3: **5/6** | E4: 6/6 | E5: 6/6
Targeted failure: 无
Change: 引入更严格的v2 eval suite
Reasoning: 需要能discriminate真实质量差异的eval
Result: 唯一失分点在E3术语密度，T3-R1在LLM新领域话题只嵌入1个CamelCase术语
Remaining failures: E3在偏离核心领域话题(LLM/AI)时脆弱

## Experiment 1 -- keep

Score: 30/30 (100%)
E1: 6/6 | E2: 6/6 | E3: **6/6** (+1) | E4: 6/6 | E5: 6/6
Targeted failure: E3 在 T3 (LLM话题) 的术语密度不足
Change: 在"表达DNA"段末尾新增"术语锚定硬约束"条目，明确要求：
  - 每个回答必须嵌入≥2个Fowler专属CamelCase概念
  - 偏离核心领域时，强制从备选池(Technical Debt/YAGNI/Semantic Diffusion/Code Smell/Design Stamina/Feature Toggle/Strangler Fig/Faux-X)挑一个做类比锚定
  - 提供典型句式: "YAGNI在这里不适用，因为YAGNI说的是功能，不是X"
  - 送出前自检CamelCase概念数量
Reasoning: baseline-v2的失败模式是"话题相关的脆弱点"——Fowler主场话题术语免费到位，偏离时模型滑向通用措辞。直接加硬约束+备选池+模板句式应该能定向修复。
Result: T3-R1命中Cognitive Debt + YAGNI + Technical Debt 三个术语，且使用了规则提供的模板句式; T3-R2用"Code Smell that lives in the workflow"做类比锚定。修复精确命中预期机制。
Remaining failures: 无 (达到stop rule 100%)

注意: n=6 样本小, 29→30 是暗示性证据而非统计显著。潜在反向风险: 未来可能出现"术语堆砌"副作用, 如果发现可加入"术语上限"软约束。

---

## 最终状态

**Kept version**: Experiment 1 后的 SKILL.md
**Final score**: 30/30 (100%) on fixed v2 eval suite
**Stop rule触发**: pass_rate ≥ 95%

## 关键发现

1. **满分baseline不等于没问题**: v1的30/30掩盖了4个真实弱点，必须通过收紧eval discover
2. **术语密度是话题相关的脆弱点**: 在核心领域(微服务/Scrum)免费达标，偏离时需要主动锚定机制
3. **最有效的修复是"备选池+模板句式"**: 比抽象原则("要用更多术语")更可执行
4. **评测agent的自由观察比打分更有价值**: v1的满分打分不可信，但它的定性观察("Recursive Skepticism缺席")才是真正的信号
