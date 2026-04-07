# F-002: 核心概念讲解技术分析

## 技术深度评估：⭐⭐⭐⭐⭐

**核心地位：** F-002 是整个教程的基石，理解这三个概念是掌握 SuperPower 的前提。

## 概念解释策略

### 1. Agent（代理）概念

**技术定义：**
Agent 是具有明确职责、隔离上下文、独立执行的 AI 实体。

**关键区分：**
- **主 Agent（Coordinator）**：协调整个流程，保持会话状态
- **子 Agent（Subagent）**：执行具体任务，零上下文继承

**心智模型建立：**
```
传统模式：
用户 → AI (单个有状态实体) → 代码

SuperPower 模式：
用户 → 主 Agent (协调者)
        ↓
      子 Agent A (执行者) [干净状态]
        ↓
      子 Agent B (审查者) [干净状态]
        ↓
      子 Agent C (审查者) [干净状态]
        ↓
      主 Agent (汇总结果)
```

**技术深度要点：**
1. **状态隔离**：每个子代理从干净状态启动
2. **上下文注入**：主代理精心构建子代理需要的上下文
3. **结果聚合**：主代理收集子代理的输出并决定下一步

**示例场景：**
```markdown
任务：实现用户认证功能

传统方式：
AI 直接开始编写代码，可能遗漏边界情况

SuperPower 方式：
1. brainstorming 技能触发 → 明确需求
2. writing-plans 技能触发 → 分解任务
3. 子代理 1 实现登录逻辑
4. 子代理 2 审查规范符合性
5. 子代理 3 审查代码质量
6. 主代理决定是否通过
```

### 2. Skill（技能）概念

**技术定义：**
Skill 是包含 Front Matter 元数据和结构化内容的工作流定义文件。

**技术架构：**
```yaml
---
name: skill-name              # 技能标识符
description: Use when...      # 触发条件描述
---

<content>                      # 工作流内容
- 步骤定义
- 决策树
- 红旗检测
- 验证清单
</content>
```

**自动发现机制：**
```
用户输入 → 解析意图 → 匹配 Skill description
                          ↓
                    触发匹配度 ≥ 1% ?
                          ↓ 是
                    加载 Skill 内容
                          ↓
                    执行工作流步骤
```

**技术深度要点：**
1. **声明式触发**：通过 description 自动匹配
2. **优先级系统**：流程技能 > 实现技能
3. **刚性 vs 灵活**：TDD 严格遵循，设计模式灵活适配

**技能分类矩阵：**
| 类型 | 技能示例 | 触发时机 | 刚性 |
|------|---------|---------|------|
| 流程 | brainstorming | 任务开始前 | 灵活 |
| 流程 | systematic-debugging | 遇到 bug | 严格 |
| 实现 | test-driven-development | 编写代码前 | 严格 |
| 实现 | subagent-driven-development | 执行计划时 | 灵活 |
| 协作 | requesting-code-review | 完成任务后 | 灵活 |

**示例代码：**
```markdown
用户："帮我实现用户登录功能"

系统自动触发流程：
1. using-superpowers (检查技能可用性)
2. brainstorming (明确需求细节)
3. writing-plans (制定实施计划)
4. using-git-worktrees (创建隔离环境)
```

### 3. Command（命令）概念

**技术定义：**
Command 是用户通过特定语法触发的显式操作指令。

**与 Skill 的区别：**
| 维度 | Skill | Command |
|------|-------|---------|
| 触发方式 | 自动/隐式 | 手动/显式 |
| 适用场景 | 工作流集成 | 直接操作 |
| 发现机制 | 意图匹配 | 语法解析 |
| 示例 | brainstorming | /plugin install |

**技术实现：**
```
用户输入 → 语法解析 → 识别命令前缀 → 执行命令逻辑
```

**命令类别：**
1. **系统命令**：`/plugin`, `/help`, `/clear`
2. **项目命令**：`/test`, `/build`, `/deploy`
3. **自定义命令**：用户定义的快捷操作

## 示例代码规划

### 1. Agent 概念示例

**目标：** 展示子代理的隔离特性

```markdown
# 场景：实现简单的计数器功能

## 传统方式演示
[展示单个 AI 逐步实现，可能忽略边界情况]

## SuperPower 方式演示
### 主 Agent 角色代码
```
// 伪代码：主代理协调逻辑
function coordinateTask(task) {
  const plan = createPlan(task);
  const results = [];

  for (const subtask of plan.tasks) {
    const subagent = createSubagent(); // 零上下文
    const context = buildContext(subtask);
    const result = subagent.execute(subtask, context);

    // 审查阶段
    const specReview = createSubagent().review(result, 'spec');
    if (!specReview.passed) {
      result = subagent.fix(result, specReview.issues);
    }

    const qualityReview = createSubagent().review(result, 'quality');
    if (!qualityReview.passed) {
      result = subagent.fix(result, qualityReview.issues);
    }

    results.push(result);
  }

  return aggregateResults(results);
}
```

### 子 Agent 角色代码
```
// 伪代码：子代理执行逻辑（干净状态）
function execute(task, context) {
  // 没有之前任务的记忆
  // 只有当前任务的完整上下文

  // 遵循 TDD
  const test = writeFailingTest(task);
  const impl = writeMinimalCode(test);
  const verified = verifyTestsPass(impl);

  return {
    status: 'DONE',
    code: impl,
    tests: test
  };
}
```

### 2. Skill 概念示例

**目标：** 展示技能的自动触发机制

```markdown
# 场景：修复一个 bug

## 用户输入
"用户登录后 session 没有持久化"

## 自动触发链
```
1. using-superpowers
   → 检查：有 bug 需要修复
   → 匹配：systematic-debugging skill

2. systematic-debugging
   → 阶段 1：观察问题
   → 阶段 2：根因分析
   → 阶段 3：验证假设
   → 阶段 4：修复验证

3. test-driven-development
   → 写失败测试复现 bug
   → Watch it fail
   → 修复代码
   → 验证通过

4. receiving-code-review
   → 响应审查反馈
   → 修复问题
   → 重新提交
```

## 技能 Front Matter 示例
```yaml
---
name: systematic-debugging
description: Use when investigating unexpected behavior,
             finding root causes, and verifying fixes
---
```

### 3. Command 概念示例

**目标：** 展示命令的显式触发特性

```markdown
# 场景：安装 SuperPower 插件

## 传统方式（手动配置）
1. 下载源码
2. 配置路径
3. 设置环境变量
4. 验证安装

## Command 方式
```
/plugin install superpowers@claude-plugins-official
```

## 命令解析流程
```
输入：/plugin install superpowers@claude-plugins-official
       ↓
解析：/plugin (命令) + install (动作) + superpowers@... (参数)
       ↓
执行：安装逻辑 → 下载 → 配置 → 验证
       ↓
输出：安装成功/失败信息
```
```

## 潜在陷阱警告

### 1. Agent 概念陷阱

**陷阱 1：混淆主 Agent 和子 Agent**
- 错误理解：子代理继承主代理的上下文
- 正确理解：子代理从零开始，主代理注入精确上下文

**陷阱 2：过度使用子代理**
- 错误做法：简单任务也用子代理
- 正确做法：机械任务可以直接执行，复杂任务才用子代理

**陷阱 3：忽略状态机**
- 错误做法：所有状态都当 DONE 处理
- 正确做法：根据状态（DONE_WITH_CONCERNS/NEEDS_CONTEXT/BLOCKED）采取不同行动

### 2. Skill 概念陷阱

**陷阱 1：理性化逃避**
- 常见想法："这只是简单问题，不需要技能"
- 现实：问题 = 任务，必须检查技能

**陷阱 2：技能顺序错误**
- 错误顺序：实现技能 → 流程技能
- 正确顺序：流程技能 → 实现技能

**陷阱 3：刚性 vs 灵活混淆**
- 错误做法：灵活适配 TDD 流程
- 正确做法：TDD 严格遵循，设计模式灵活适配

### 3. Command 概念陷阱

**陷阱 1：命令 vs 技能混淆**
- 错误理解：命令和技能是同一东西
- 正确理解：命令是显式操作，技能是自动工作流

**陷阱 2：过度依赖命令**
- 错误做法：总是用命令触发功能
- 正确做法：让技能自动触发，命令用于特殊情况

## 学习检查点

学完 F-002 后，学习者应该能够：

1. **解释区别**
   - 主 Agent vs 子 Agent
   - Skill vs Command
   - 自动触发 vs 显式触发

2. **描述流程**
   - 一个完整的工作流如何通过多个技能协作
   - 子代理的生命周期和状态转换
   - 技能的自动发现和触发机制

3. **识别陷阱**
   - 什么情况下会理性化逃避技能
   - 何时使用子代理 vs 直接执行
   - 刚性技能和灵活技能的区别

4. **应用场景**
   - 给定场景，判断应该触发哪些技能
   - 给定任务，决定是否使用子代理
   - 给定需求，选择合适的工作流

## 技术深度总结

F-002 是整个教程的技术基础，必须深入讲解而非浅层介绍。每个概念都要配合：

1. **精确定义**：技术层面的严格定义
2. **心智模型**：帮助理解的类比和图示
3. **代码示例**：展示而非讲述
4. **陷阱警告**：常见的误解和错误
5. **实践练习**：巩固理解的具体任务

只有这三个概念都理解透彻，后续的功能和工作流讲解才能顺利进行。
