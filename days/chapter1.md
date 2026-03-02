# 第 1 章：编程 LLM 和 AI 开发入门

## 课程概述

在大语言模型时代，软件开发正在经历一场革命性的变革。本课程将帮助你掌握现代 AI 编程工具，理解 AI 辅助开发的最佳实践。

### 学习目标
- 理解大语言模型（LLM）的基本原理
- 掌握高效提示工程技巧
- 明确 Human-agent engineering 的核心技能
- 理解 LLM 的能力边界

---

## 1. 为什么学这个？

> "You won't be replaced by AI. You'll be replaced by a competent engineer who knows how to use AI."

AI 编程是未来软件工程师的核心技能。不会 AI 的工程师将被会 AI 的工程师替代。但这**不是** "vibe coding"课程——不能盲目相信 AI 输出，Human-agent engineering 才是核心。

> Prompts are the lingua franca for getting LLMs to do what we want and also effectively programming them
>
> — Andrej Karpathy

### 坏消息 vs 好消息

| 消息 | 内容 |
|------|------|
| **坏消息** | Windsurf 团队（AI 编程公司被收购） |
| **好消息** | 开发者可以比以往更高效；AI 工程师可以以前所未有的速度掌握技术栈和工具 |

---

## 1.1 课程信息

| 项目 | 内容 |
|------|------|
| 课程代码 | CS146S |
| 学校 | 斯坦福大学，2025 年秋季 |
| 讲师 | Mihail Eric |
| 课程网站 | themodernsoftware.dev |

### 课程安排
- **上课时间**: 周一/周五 8:30-9:20 am PT
- **交付内容**: 9 次作业（每周 1 次）+ 1 个期末项目
- **评分方式**: 项目 80% / 作业 15% / 参与度 5%

### 讲师简介
Mihail Eric 是斯坦福大学本科/研究生，曾：
- 在一家隐秘的销售领域创业公司领导 AI
- 在 Amazon Alexa 构建首批 LLM
- 创立并出售了一家 ML 教育创业公司
- 创立了一家 YC 支持的 AI 编程公司

### 课程助教
- Febie Lin

### 嘉宾讲座
本课程包含来自顶级 AI 开发者创业公司创始人的嘉宾讲座：
- 融资数亿美元，估值数十亿
- 不要错过这些讲座！

### Human-agent Engineering 核心技能
1. **阅读和审查大量代码** - 学会辨别好代码和坏代码
2. **有好的品味** - 理解什么是优秀的软件
3. **积极实验** - 没有人有标准答案，每个人都在探索

### 关键洞察
- LLM 和你一样好（Good context leads to good code）
- 如果你不能理解你的代码库，LLM 也不能

---

## 2. LLM 基础概念

### 什么是 LLM？
LLM（Large Language Models，大语言模型）是用于下一个 token 预测的**自回归模型**（autoregressive models）。

```
输入文本 → Tokenize → Embedding → Transformer 层（12-96+）→ 输出预测
```

#### Transformer 层
- 使用自注意力机制（Self-attention mechanism，参考 Viswani et al.）
- 12-96+ 层 Transformer
- 将 tokens 转换为固定维度的数值向量（~1-3K 维度）

#### Embedding
将文本 tokens 转换为固定维度的数值向量。

---

## 3. LLM 训练过程

### 三个训练阶段

| 阶段 | 名称 | 数据量 | 目标 |
|------|------|--------|------|
| Stage 1 | Self-supervised Pretraining | 100B - 1T+ tokens | 学习语言概念 |
| Stage 2 | Supervised Finetuning | 10K - 100K pairs | 遵循指令 |
| Stage 3 | Preferencing Tuning | 10K - 100K comparisons | 对齐人类偏好 |

### Stage 1: 预训练 (Pretraining)
- 使用海量公开数据（Common Crawl, Wikipedia, StackExchange、GitHub）
- 学习语言的基本概念和模式
- **示例**: "Write a for loop" → "that could be used in a piece of code"

### Stage 2: 监督微调 (SFT)
- 使用高质量的指令-响应对
- 教模型遵循人类指令
- **示例**: "what is the capital of Croatia" → "Zagreb is the capital"

### Stage 3: 偏好调优 (Preferencing)
- 收集同一提示的多个输出对
- 训练奖励模型预测偏好输出
- 对齐人类偏好（有用性、正确性、可读性）
- **示例**: "Write a for loop" → "for idx in range(10):"

#### Reasoning Models（推理模型）
- 添加 chain-of-thought 推理 traces
- 集成工具使用能力
- 通过强化学习学习评估推理过程
- 学习回溯等推理步骤

#### 模型规模参考
- GPT-3 / Claude 3.5 Sonnet: ~175B 参数
- LLaMA 3.1: 405B 参数
- GPT-4: ~1.8T 参数

---

## 4. LLM 的能力与局限

### 优势
- **Expert-level code completion** - 专家级代码补全
- **Code understanding** - 代码理解
- **Code fixing** - 代码修复

### 局限性
| 局限性 | 描述 | 应对方案 |
|--------|------|----------|
| Hallucinations | 幻觉（生成不存在的/过时的 API） | 健壮的上下文工程 |
| Context window limits | 上下文窗口限制（~100-200K tokens，但并非所有都同等有效） | 精选上下文 |
| Latency | 延迟（每秒到每分钟不等） | 根据任务规划 |
| Cost | 成本（输入 $1-3/百万 tokens，输出 $10+/百万） | 优化提示长度 |

---

## 5. 提示工程技巧

### 提示工程背景
提示既是艺术也是科学。LLM 的黑箱特性意味着有一些"LLM 低语"的技巧...但有已建立的技术经验性地提高了 LLM 性能。

### Zero-shot Prompting
直接让 LLM 执行任务，无需示例。

**示例**: Write me a Rust for-loop that iterates over a list of strings for every, printing every value in an even index

### K-shot Prompting
提供 k 个示例（1, 3, 5 个），也称为"上下文学习"（in-context learning），适合需要特定格式的任务。

**示例**:
```
Write a for-loop iterating over a list of strings using the naming convention in our repo. Here are some examples of how we typically format variable names.

<example>var StRaRrAy = ['cat', 'dog', 'wombat']</example>
<example>def func CaPiTaLiZeStR = () => {}</example>
```

### Chain-of-Thought (CoT)
- **Multi-shot CoT**: 展示推理步骤示例
- **Zero-shot CoT**: 添加"Let's think step-by-step"或提示在显式 <reasoning> 标签中推理
- 适合多步逻辑任务（编程、数学）

### Role Prompting
指定模型角色，增强输出质量。

**角色提示示例**:
```
You are a helpful assistant that loves programming at the level of a senior software developer and is very detailed and pedantic in your answers.
```
```
You are a Gen Z digital bestie. Always sound like you're texting on Snapchat at 2am.
```

### System Prompt vs User Prompt

| 类型 | 描述 |
|------|------|
| **System Prompt** | 第一条消息，定义 LLM 整体行为和规则（通常最终用户看不到） |
| **User Prompt** | 用户的实际请求和指令 |
| **Assistant** | LLM 实际生成的响应 |

### 最佳实践

#### 提示改进
- 使用 [Claude Prompt Improver](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-improver)
- 给一个几乎不了解上下文的人看提示，如果他们困惑，LLM 也会困惑

#### 结构化提示
```
Here are the logs:
<log>LOG MESSAGE</log> and the stack trace:
<error>STACK TRACE</error>
```

#### 明确具体
- 指定语言、技术栈、库、约束条件

#### 分解任务
- 复杂任务拆分成简单步骤

---

## 6. 实践练习

### 练习 1: 尝试不同提示技巧
使用 Claude 或其他 LLM 尝试：
1. Zero-shot: "Write a function to check if a number is prime"
2. K-shot: 提供示例后让 LLM 按照相同格式输出
3. CoT: 添加"Let's think step-by-step"观察输出变化

### 练习 2: 角色提示
尝试不同角色提示，比较输出质量：
- "You are a junior developer..."
- "You are a senior software architect..."
- "You are a Gen Z digital bestie..."

### 练习 3: 结构化提示
提供日志和错误信息，观察 LLM 如何解析和响应。

### 练习 4: 使用 Prompt Improver
使用 Claude Prompt Improver 优化你的提示，观察改进效果。

---

## 7. 课程学习路径

| 天数 | 主题 |
|------|------|
| Chapter 1 | LLM 基础 + 提示工程 |
| Chapter 2 | Coding Agent + MCP |
| Chapter 3 | AI IDE + 同步/异步 |
| Chapter 4 | Agent 管理 + Claude Code |
| Chapter 5 | AI 开发产品设计 |
| Chapter 6 | 测试与安全 |
| Chapter 7 | Code Review |
| Chapter 8 | AI 应用构建 |
| Chapter 9 | AI DevOps |
| Chapter 10 | 未来展望 |

---

## 讲座资料

### Lecture 1: Introduction and How LLMs are Made
- [Slides (PDF)](../slides/week1-lecture1-introduction.pdf)
- **讲师**: Mihail Eric

### Lecture 2: Power Prompting for LLMs  
- [Slides (PDF)](../slides/week1-lecture2-power-prompting.pdf)

---

## 阅读材料

### 必读
1. **[Prompt Engineering Guide](https://www.promptingguide.ai/techniques)** - 全面的提示工程技术指南
2. **[Deep Dive into LLMs (YouTube)](https://www.youtube.com/watch?v=7xTGNNLPyMI)** - LLM 深度解析

### 选读
3. **[Prompt Engineering Overview](https://cloud.google.com/discover/what-is-prompt-engineering)** - Google Cloud 提示工程概述

---

## 作业

### LLM Prompting Playground
**[Chapter 1 Assignment](https://github.com/mihail911/modern-software-dev-assignments/tree/master/week1)**

通过实践掌握 LLM 提示技巧。

---

## 关键概念

| 术语 | 含义 |
|------|------|
| **Tokenization** | 文本如何被分割成 tokens |
| **Context Window** | 模型能处理的上下文长度 |
| **Temperature** | 控制输出随机性的参数 |
| **Embedding** | 将 tokens 转换为固定维度的数值向量 |
| **Human-agent Engineering** | 人类工程师作为 AI 的管理者和决策者 |
| **Autoregressive Models** | 自回归模型，用于下一个 token 预测 |
| **Self-attention** | 自注意力机制 |
| **In-context Learning** | 上下文学习，即 K-shot Prompting |

---

## 下一章

[下一章：Chapter 2](./chapter2.md)

---
