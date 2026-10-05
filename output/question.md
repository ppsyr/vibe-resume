# 1. Poirot：可持久化的深度研究 Agent 内核

## 1. ReAct 运行框架

## 2. Skill 自进化与评估

生产代码零改动，全部新增文件都在 `evaluation/` 下。

---

## 阶段 0 和 1 完成

### 阶段 0：评测集

`evaluation/datasets/skill_evolution_v1.jsonl` — 37 个真实内置 Skill，每个构造 3 个变体，共 74 个决策样本。

这里我修正了原计划的一个偏差：**程序化门控评估的对象是 Skill 文档质量，不是任务回答质量**，所以不需要 40 条任务集。评测单位是「同一 base Skill 的多个变体」，37 × 3 就能得到足够的样本量，而且用的是真实 Skill 文档，不是人造数据。

### 阶段 1：程序化门控评测台

调用生产代码路径（`ProgrammaticEvalBridge` → `ScoreDeltaGate`），没有改动任何生产代码。

**主评测集结果（74 样本）**

| 指标 | 值 | 95% CI |
|---|---|---|
| 门控准确率 | 100% | 95.1% – 100% |
| 误升级率 | 0% | 0% – 4.9% |
| 误拒率 | 0% | 0% – 4.9% |
| hard failure 拦截率 | 100% | 90.6% – 100% |

**对抗集结果（15 样本）**

我额外加了 5 类边界样本（同分、低分、hard failure、最小改进、完全改进）来检验门控是否真的在比较分数。结果暴露出一个真实缺陷。

### 发现的缺陷：K01

**规则集不对称导致同分候选被误 accept。**

根因在 `ContractCompiler` 只按 candidate 文本编译规则。当 candidate 新增了 contract 规则（比如声明了"先给结论"从而编译出 `lead_with_conclusion`）：

```
rule                      base    cand
nonempty                  True    True
json_parseable            True    True
must_cite                 True    True
lead_with_conclusion      False   True   ← 新增规则，baseline 未通过
no_unfounded_claims       True    True
semantic_density          True    True

candidate = 6/6 = 1.00
baseline  = 5/6 = 0.83  ← 被压低
→ 门控判定 1.00 > 0.83，accept
```

实际上 candidate 和 baseline 在各自适用的规则下都满分，没有真实提升，但因为规则集从 4 条扩到 6 条，baseline 反推分被压低，造成伪提升。对抗集里 2/15 的误升级全部来自这个缺陷。

我没有在阶段 1 修它。修法涉及 `ContractCompiler` 的双向编译语义（baseline 应按 candidate 的规则集重新评估），属于功能变更，应该单独做。评测台把 `affected: 2` 锁定为回归基线，修好后会变成 0，测试会提醒你更新。

### 顺带发现的一个规则问题

`semantic_density` 只统计英文大写指令词（MUST/ALWAYS/NEVER），导致 **37 个真实 Skill 里 28 个只得 0.75 分**，全部因为密度低于 0.005 下限。10 个 Skill 密度为 0，18 个在 0–0.005 之间。这个规则对中文 Skill 不适用，是个需要单独调整的设计问题。

### 可复现

```bash
.venv/bin/python evaluation/runner.py          # 输出 JSON 报告
.venv/bin/python -m pytest evaluation/ -q      # 6 个测试
```

报告已存到 `evaluation/report_stage1.json`。

---

## 能写进简历的数字

现在有真实数据支持的：

> 晋升门在 **74 个程序化评估样本**上准确率 **100%**（95% CI 95.1–100%），误升级率 **0%**，hard failure 拦截率 **100%**；对抗测试发现并定位规则集不对称导致的伪提升缺陷

要不要我继续修 K01，还是先进入阶段 2（LLM judge 一致性）？