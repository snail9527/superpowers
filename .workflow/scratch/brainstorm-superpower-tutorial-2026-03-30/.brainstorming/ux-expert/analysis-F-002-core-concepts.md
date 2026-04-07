# F-002 核心概念讲解 UX 分析

**功能 ID**: F-002
**功能名称**: 核心概念讲解（Agent/Skill/Command）
**分析者**: UX 专家
**文档版本**: 1.0
**分析日期**: 2026-03-30

---

## 1. 功能概述

**目标**：在 30 分钟内让用户深入理解 SuperPower 的三大核心概念，建立完整的心理模型。

**核心价值**：
- 系统化理解工具架构
- 掌握核心交互模式
- 为高级应用打下基础

---

## 2. 概念学习路径设计

### 2.1 三概念递进关系

**依赖关系分析**：
```
Command（用户视角）
    ↓
Agent（执行视角）
    ↓
Skill（实现视角）
```

**推荐学习顺序**：
1. **Command 优先**（5分钟）
   - 从用户最熟悉的入口开始
   - 理解"如何向系统发出指令"

2. **Agent 核心化**（15分钟）
   - Agent 是系统的中枢
   - 理解"指令如何被处理"

3. **Skill 深入**（10分钟）
   - Skill 是执行单元
   - 理解"任务如何被完成"

### 2.2 概念理解层次

**表层理解**（是什么）：
- Command = 用户指令
- Agent = AI 助手
- Skill = 工作能力

**关系理解**（如何协作）：
```
User 发送 Command
  ↓
Agent 接收并理解
  ↓
Agent 选择匹配的 Skill
  ↓
Skill 执行具体任务
  ↓
Agent 返回结果给 User
```

**深层理解**（为什么这样设计）：
- Agent 提供统一的接口
- Skill 实现模块化和可组合性
- Command 维持简洁的用户体验

---

## 3. 认知负荷管理

### 3.1 分步披露策略

**"每章 3 概念"规则**：

**Command 章节**（5分钟）：
1. Command 的基本结构
2. 参数传递方式
3. 输出格式解读

**Agent 章节**（15分钟）：
1. Agent 的职责
2. Agent 的生命周期
3. Agent 的选择策略

**Skill 章节**（10分钟）：
1. Skill 的触发条件
2. Skill 的执行机制
3. Skill 的组合方式

### 3.2 概念间桥梁设计

**过渡提示**：
```
📖 学习进度：Command → Agent

你已经了解了 Command（用户指令），现在来看看 Agent 如何处理这些指令。

思考问题：
当你发送一个 Command 时，谁在"听"？谁来"理解"？谁来"执行"？

答案：Agent。
```

**关联提醒**：
```
🔗 概念关联

在学习 Agent 时，回忆一下你刚刚学到的 Command：
- Command = 你说的话
- Agent = 听话并执行的人

这就像在餐厅点餐：
- Command = "我要一份牛排"
- Agent = 服务员（理解需求，传达给厨房）
- Skill = 厨师（实际制作）
```

### 3.3 认知辅助工具

**类比系统**：
- Agent = 餐厅服务员（理解需求，协调执行）
- Skill = 专业厨师（执行特定任务）
- Command = 顾客订单（表达需求）

**可视化图表**：
```
User
 │
 ├─ Command ──→ Agent
 │                   │
 │                   ├─→ Skill A (build)
 │                   ├─→ Skill B (test)
 │                   └─→ Skill C (deploy)
 │
 ←──────── Result ────┘
```

---

## 4. 交互模式建议

### 4.1 概念探索模式

**示例驱动学习**：
```
## Agent 机制

### 基本示例
让我们从一个简单的例子开始：

$ superpower brainstorm "添加用户登录"

### 发生了什么？

1. 你发送了一个 Command
2. Agent 接收并分析
3. Agent 匹配到 brainstorm Skill
4. brainstorm Skill 执行并生成结果
5. Agent 返回结果给你

### 为什么是 Agent 而不是直接执行 Skill？

想象一下，如果你需要：
- 先 brainstorm 方案
- 然后 plan 设计
- 最后 develop 实现

如果直接调用 Skill，你需要：
$ superpower-skill-brainstorm "..."
$ superpower-skill-plan "..."
$ superpower-skill-develop "..."

使用 Agent，你只需要：
$ superpower "添加用户登录"

Agent 会自动：
1. 理解你的完整需求
2. 选择合适的 Skill 顺序
3. 协调 Skill 间的数据传递
4. 处理错误和重试
```

### 4.2 交互式验证

**概念自测**：
```
📝 快速自测

问题 1：Agent 的主要职责是什么？

A. 执行具体任务
B. 理解指令并协调 Skill
C. 存储数据
D. 渲染界面

[思考 10 秒...]

↓
↓
↓

✅ 正确答案：B

解析：
- A 是 Skill 的职责
- C 和 D 不是 Agent 的工作
- Agent 的核心价值是"理解"和"协调"

[继续下一题] [返回 Agent 章节]
```

### 4.3 动手实验

**实验式学习**：
```
🔬 实验：观察 Agent 的 Skill 选择

实验 1：观察 Skill 匹配
$ superpower brainstorm "构建新功能"
$ superpower brainstorm "build new feature"

观察：两个命令触发了相同的 Skill

结论：Agent 理解同义词，能正确匹配 Skill

---

实验 2：观察 Skill 协作
$ superpower "从零开发用户登录"

观察：Agent 按顺序执行了多个 Skill
1. brainstorm（生成方案）
2. plan（制定计划）
3. develop（开发实现）

结论：Agent 能智能编排 Skill 序列

---

实验 3：观察错误处理
$ superpower invalid-command

观察：Agent 提供了清晰的错误信息和建议

结论：Agent 有友好的错误处理机制

💡 总结：Agent 不是简单的路由器，而是智能的协调者
```

---

## 5. 记忆强化设计

### 5.1 重复与变式

**螺旋式重复**：
```
第 1 次接触（快速入门）：
"Command 是你发送给 Agent 的指令"

第 2 次接触（Command 章节）：
"Command 由意图、参数和选项组成"

第 3 次接触（Agent 章节）：
"Agent 通过解析 Command 来理解你的需求"

第 4 次接触（Skill 章节）：
"Skill 定义了能响应哪些 Command 触发条件"
```

### 5.2 关联记忆法

**概念矩阵**：
```
      │ User │  System  │ Implementation
──────┼──────┼──────────┼───────────────
入口  │Command│          │
处理  │      │  Agent   │
执行  │      │          │    Skill
```

**角色扮演**：
```
如果你是 User：
"我发送 Command"

如果你是 Agent：
"我接收 Command，选择 Skill"

如果你是 Skill：
"我被 Agent 触发，执行任务"
```

### 5.3 记忆检查点

**章节回顾**：
```
📚 Command 章节回顾

核心要点：
✓ Command 是用户与 Agent 的交互接口
✓ Command 包含意图、参数和选项
✓ Command 的格式影响 Agent 的理解

记忆检查：
你能用一句话描述 Command 吗？

[显示答案]
```

**跨章节综合**：
```
🎯 核心概念综合测试

场景：你运行 `superpower develop "用户登录"`

问题链：
1. 这是什么？ → 这是一个 Command
2. 谁接收它？ → Agent 接收并解析
3. 接下来发生什么？ → Agent 匹配到 develop Skill
4. Skill 做什么？ → 执行开发任务
5. 结果如何返回？ → Agent 收集 Skill 输出并返回给你

完整流程图：
User → Command → Agent → Skill → Agent → Result

✅ 理解完整流程！
```

---

## 6. 常见误解预防

### 6.1 概念混淆点

**混淆点 1：Agent vs Skill**

常见误解：
- ❌ "Agent 就是 Skill 的别名"
- ❌ "Skill 是 Agent 的一部分"

正确理解：
- ✅ Agent 是协调者，Skill 是执行者
- ✅ Agent 包含多个 Skill，选择执行哪个

**预防策略**：
```
⚠️ 概念澄清：Agent vs Skill

Agent ≠ Skill

比喻：
- Agent = 项目经理（理解需求，分配任务）
- Skill = 专业工程师（执行具体任务）

关键区别：
┌────────────┬──────────┬──────────┐
│            │  Agent   │  Skill   │
├────────────┼──────────┼──────────┤
│ 职责       │ 协调     │ 执行     │
│ 数量       │ 1 个主   │ 多个     │
│ 选择能力   │ 有       │ 无       │
│ 交互能力   │ 与用户   │ 与 Agent │
└────────────┴──────────┴──────────┘
```

**混淆点 2：Command vs 参数**

常见误解：
- ❌ "Command 的参数就是 Command"

正确理解：
- ✅ Command = 动作 + 参数的完整结构

**预防策略**：
```
⚠️ 概念澄清：Command 结构

Command ≠ 参数

完整 Command 结构：
┌─────────────────────────────────┐
│ superpower develop "用户登录"    │
│          ↑       ↑      ↑        │
│        动作    参数  参数值      │
│                     (整体是 Command)│
└─────────────────────────────────┘

类比：
- 动作 = 动词（develop）
- 参数 = 宾语（"用户登录"）
- Command = 完整句子（"开发用户登录"）
```

### 6.2 过度简化陷阱

**风险**：类比过度简化导致误解。

**缓解**：
```
💡 类比的局限性

我们用"餐厅"来比喻 SuperPower：
- Agent = 服务员
- Skill = 厨师
- Command = 订单

⚠️ 但这个类比有限制：

1. Agent 比服务员更智能
   - 服务员不懂"烹饪方法"
   - Agent 理解"实现策略"

2. Skill 比厨师更模块化
   - 厨师有完整的技能
   - Skill 只专注一个任务

3. Command 比订单更灵活
   - 订单格式固定
   - Command 支持自然语言

✅ 类比帮助入门，但不要局限于类比
```

---

## 7. 学习路径可视化

### 7.1 概念地图

```
核心概念地图
│
├─ Command（用户接口）
│  ├─ 基本结构
│  ├─ 参数传递
│  └─ 输出格式
│
├─ Agent（核心引擎）
│  ├─ 职责定义
│  ├─ 生命周期
│  └─ 选择策略
│  └─ 与 Command 的关系
│  └─ 与 Skill 的关系
│
└─ Skill（执行单元）
   ├─ 触发条件
   ├─ 执行机制
   └─ 组合方式
   └─ 与 Agent 的关系
```

### 7.2 学习进度追踪

```
核心概念学习进度
████████░░░░░░░░░░░  40% 完成 (1.2/3 章节)

已完成：
✓ Command 结构 (5分钟)
✓ Agent 概念 (15分钟)

进行中：
→ Skill 机制 (预计 10分钟)

待完成：
○ 三者关系 (5分钟)
○ 综合练习 (10分钟)
```

---

## 8. 设计决策记录

### 8.1 概念顺序选择

**决策**：Command → Agent → Skill

**理由**：
1. Command 是用户最熟悉的入口
2. Agent 是连接 User 和 Skill 的桥梁
3. Skill 是实现细节，最后讲解

**替代方案考虑**：
- Skill → Agent → Command：实现者视角，不适合用户
- Agent → Skill → Command：过于抽象，缺乏切入点

### 8.2 深度 vs 广度

**决策**：深度优先于广度

**理由**：
- 三个核心概念必须深入理解
- 表面了解会导致后续学习困难
- 时间预算（30分钟）允许深入讲解

**风险**：用户可能在某个概念上卡住。

**缓解**：
- 提供多次解释角度
- 使用多样化示例
- 允许跳过细节，先掌握整体

---

## 9. 成功指标

### 9.1 理解验证指标

**概念关系理解**：
- 能画出 Agent、Skill、Command 的关系图
- 能解释为什么需要 Agent 层
- 能描述 Skill 的触发机制

**应用能力验证**：
- 能预测给定 Command 会触发哪个 Skill
- 能解释错误发生的原因
- 能设计简单的多 Skill 协作流程

### 9.2 学习效果指标

**时间效率**：
- 80% 用户在 30 分钟内完成
- 不会在某个概念上停留超过 10 分钟

**知识保持**：
- 完成 24 小时后能回忆核心概念
- 能解释概念给其他开发者

---

*F-002 分析结束*
