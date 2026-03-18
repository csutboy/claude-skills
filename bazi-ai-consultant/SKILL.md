---
name: bazi-ai-consultant
description: >
  AI-powered Bazi (八字) fortune telling using a reinforcement-learning-inspired feedback loop.
  Use this skill whenever the user wants to: analyze their bazi/八字/four pillars of destiny,
  do Chinese fortune telling or fate analysis, ask about their destiny/运势/命盘, want to
  predict their future based on birth date and time, or ask about 大运/流年/命理.
  Also trigger for requests like "帮我算八字", "分析我的命盘", "用AI算命", "推算我的运势",
  "DeepSeek算八字", or any mention of 生辰八字 combined with AI analysis.
  This skill provides a structured 3-phase workflow (排盘→校准→预测) with exact prompt templates
  in Chinese that users can copy directly into their AI conversation.
---

# Bazi AI Consultant (八字 AI 命理顾问)

This skill implements a 3-phase, feedback-loop-driven bazi reading that mirrors how reinforcement
learning works: each round of user feedback helps the AI calibrate its understanding of the
person's specific chart before making predictions.

The core insight: bazi symbols are multi-layered (e.g., 财星 can mean money, spouse, or father).
Without feedback, AI guesses which meaning applies. With feedback, it learns which interpretation
fits *this specific person's* chart.

---

## How to guide the user

Tell the user the process has 3 phases and they should work through them in order in a single
conversation with their chosen AI (DeepSeek, Claude, GPT, etc.).

---

## Phase 1: 排盘 (Chart Setup)

**Goal**: Generate an accurate bazi chart. The most common error is miscalculating 起运年龄
(the age when major luck cycles begin) — always cross-validate this.

Give the user this prompt to copy into their AI:

```
# 角色
你是一位精通子平八字的命理大师，擅长排盘和命理推演。

# 任务
根据我的出生信息，排出完整的八字命盘。

# 输入信息
- 阳历：____年____月____日____时
- 性别：____
- 出生地：____

# 输出要求
请按以下格式输出：

1. 四柱八字
   - 年柱：天干+地支
   - 月柱：天干+地支
   - 日柱：天干+地支
   - 时柱：天干+地支

2. 起运信息
   - 起运年龄：X岁
   - 起运计算依据：（说明是阳年阳男/阴年阴女顺推，还是阳年阴女/阴年阳男逆推）

3. 大运排列
   - 列出未来10个大运（每个10年）
   - 格式：X-X岁：天干地支

4. 当前流年
   - 今年流年：天干地支

# 约束
- 起运年龄必须精确计算，这是后续推演的基础
- 大运顺序不能错
```

**Verify 起运年龄 before continuing.** Instruct the user to check with 3 of these sites and
confirm all three agree:
- 元亨利贞网: www.china95.net
- 信达利: www.xindali.com
- 卜易居: www.buyiju.com

If the AI got it wrong, give the user this correction prompt:

```
# 纠正指令
我通过多个专业排盘工具验证，我的起运年龄是X岁。

请按以下步骤重新计算：
1. 以X岁为起运点
2. 重新排列大运（每运10年）
3. 输出修正后的完整大运表

# 输出格式
X-X岁：天干地支（第一大运）
X-X岁：天干地支（第二大运）
...
```

---

## Phase 2: 校准 (Calibration) — The Most Important Phase

**Goal**: 3 feedback rounds that help the AI learn what the symbols in *this specific chart*
actually represent. Think of it as giving the AI labeled training data from the person's
real life before asking it to predict the future.

After each round, the user gives feedback using ✓ (correct) and ✗ (incorrect), and the AI
recalibrates its understanding.

### Round 1 — Past Life Events

```
# 任务
根据我的八字和大运，推断我过去可能发生的重大事件。

# 分析维度
请从以下4个维度进行分析：
1. 学业：考学、升学、学业转折
2. 事业：工作变动、职位变化、行业转换
3. 感情：恋爱、分手、婚姻
4. 健康：重大疾病、意外伤害

# 输出要求
对每个维度：
- 列出3-5个关键年份
- 说明该年可能发生的事件
- 给出推断依据（基于哪个大运、哪些天干地支的生克关系）

# 输出格式
【学业】
- XXXX年：可能发生___，依据：___

【事业】
- XXXX年：可能发生___，依据：___

【感情】
- XXXX年：可能发生___，依据：___

【健康】
- XXXX年：可能发生___，依据：___

# 约束
- 年份必须具体（如2018年，不能说"20多岁时"）
- 推断依据必须说明五行逻辑
```

**User feedback format after Round 1:**

```
我来反馈：

✓ 准的：
- 你说我XX年事业有变动，确实，那年我____

✗ 不准的：
- 你说我XX年感情顺利，实际上那年我____

请根据这些反馈，重新理解我八字中的符号含义。
```

### Round 2 — Current Traits

```
# 任务
基于我的八字命盘，推测我目前的基本情况。

# 分析维度
请从以下5个维度进行分析：
1. 外貌特征：体型、气质、面相特点
2. 性格特质：主要性格特征、行为模式、思维方式
3. 职业方向：适合的行业、职业类型、工作风格
4. 学历层次：学业成就、学习能力
5. 家庭背景：原生家庭特点、家庭关系、经济状况

# 输出要求
对每个维度：
- 给出具体推测（不要模糊表达）
- 说明推断依据（基于哪些天干地支、五行特征）
- 标注确定程度（高/中/低）

# 输出格式
【外貌特征】
推测：___
依据：___
确定程度：高/中/低

【性格特质】
推测：___
依据：___
确定程度：高/中/低

...

# 约束
- 避免"可能"、"也许"等模糊词，要给出明确判断
- 推断依据必须具体，不能只说"根据八字"
```

**User feedback format after Round 2:** Same ✓/✗ format, correcting what's wrong.

### Round 3 — Relationship History

```
# 任务
基于我的八字命盘和前两轮校准的理解，分析我的感情历史。

# 分析维度
请从以下4个方面进行分析：
1. 感情数量：可能经历过几段感情
2. 关键年份：每段感情大致在什么时间段
3. 对象特征：每段感情对象的特点（性格、外貌、职业等）
4. 问题类型：每段感情遇到的主要问题或结束原因

# 输出要求
- 具体到年份范围（如2015-2017年）
- 对象特征要具体，不要泛泛而谈
- 问题分析要深入，不只是"性格不合"

# 输出格式
【第一段感情】
时间：XXXX-XXXX年
对象特征：___
关系发展：___
主要问题：___
推断依据：___

【第二段感情】
...

【整体感情模式】
你的感情模式特点：___
容易遇到的问题：___
依据：___

# 约束
- 年份必须具体，不能说"年轻时"、"某个阶段"
- 分析要基于五行生克和大运流年，说明逻辑
```

**User feedback format after Round 3:** Same ✓/✗ format.

---

## Phase 3: 预测 (Prediction)

**Goal**: Now that the AI has been calibrated with real feedback, ask it to predict the future.
The predictions will be grounded in an understanding of what the chart's symbols actually mean
for this person — not generic interpretations.

```
# 任务
基于经过三轮校准的理解，对我的未来进行预测分析。

# 预测维度

## 1. 未来配偶特征
请分析我未来配偶可能的特征：
- 外貌：体型、气质、外在特点
- 性格：主要性格特征、行为模式
- 职业：可能从事的行业或职业类型
- 家庭：原生家庭背景、家庭关系
- 相遇时机：大致在哪个年份段遇到

## 2. 未来10年大运走势
请分析未来10年的运势变化：
- 事业运：发展趋势、关键机遇、需要注意的风险
- 财运：财富积累趋势、投资运势
- 感情运：感情发展、婚姻时机
- 健康运：需要注意的健康问题

## 3. 关键年份节点
请列出未来10年中的关键年份：
- XXXX年：可能发生的重大事件，依据
- XXXX年：可能发生的重大事件，依据
...

# 输出要求
- 预测要具体，避免"可能会很好"这种模糊表达
- 每个预测必须说明依据（基于哪个大运、哪些五行关系）
- 标注确定程度（高/中/低）
- 给出建议（如何顺应或规避）

# 约束
- 基于前三轮校准建立的理解，保持逻辑一致
- 年份必须具体
- 避免过于绝对的断言，但也不要过于模糊
- 预测的同时给出可操作的建议
```

---

## Important caveats to communicate

1. **No persistent memory**: The AI doesn't remember the calibration in a new conversation.
   Each reading must be done in a single continuous session.

2. **Feedback loop effect**: After multiple calibration rounds, later predictions naturally
   incorporate information the user provided. This is a feature, not a bug — it's exactly
   how professional fortune tellers work too.

3. **The real value**: Users often report that the calibration process itself — systematically
   reviewing which years were pivotal — is as valuable as the predictions.

---

## Tips for better results

- Do all three phases in a single conversation without refreshing
- Give specific feedback (not just ✓/✗, but what actually happened)
- The more honest and specific the feedback, the better the calibration
- Cross-validate the 起运年龄 step — skipping it causes all subsequent analysis to be wrong
