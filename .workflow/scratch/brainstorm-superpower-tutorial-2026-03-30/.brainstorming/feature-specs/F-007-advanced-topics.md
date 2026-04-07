# F-007: 高级主题深入

**功能 ID**: F-007
**功能名称**: 高级主题深入
**优先级**: P2（可以实现）
**预估字数**: 6000 字（每个主题 2000 字）
**创建日期**: 2026-03-30

---

## 1. 需求摘要

本功能提供 SuperPower 高级主题的深入讲解，包括技术深度、架构扩展和实战技巧，作为"深入理解"的查阅资料。

**MUST**:
- 按主题分类（技术深度/架构扩展/实战技巧）
- 提供完整的技术细节和边缘案例
- 支持按需学习（非强制路径）
- 参考性质（查阅资料）

**SHOULD**:
- 问题驱动的讲解风格
- 包含源码实现分析
- 展示高级模式

**MAY**:
- 添加性能调优指南
- 提供故障排除手册

**MUST NOT**:
- 作为强制学习路径
- 假设用户掌握所有基础知识

---

## 2. 设计决策

### 2.1 主题分类策略

[跨角色分析 RESOLVED] 三类主题

**技术深度**（技术领域专家）:
1. **Agent 提示词工程**
   - 提示词设计模式
   - 上下文优化策略
   - 子代理通信模式

2. **自定义 Skill 开发**
   - Skill 结构设计
   - 触发条件优化
   - 参数传递机制

**架构扩展**（系统架构师）:
1. **插件架构**
   - 扩展点设计
   - 插件生命周期
   - 依赖注入机制

2. **集成模式**
   - 外部工具集成
   - API 网关模式
   - 事件驱动架构

**实战技巧**（UX）:
1. **调试高级问题**
   - 性能分析
   - 内存泄漏诊断
   - 并发问题排查

2. **性能调优**
   - 缓存策略
   - 并发优化
   - 资源管理

### 2.2 问题驱动讲解

[跨角色分析 RESOLVED] 问题驱动 + 源码深入

**统一结构**:
```markdown
# [主题名称]

## 实际问题场景

[描述一个真实的高级场景]

---

## 解决方案（用户视角）

[如何使用 SuperPower 解决]

---

## 源码实现（深入理解）

[可选：源码分析]

> 💡 **仅限深入学习者**：可以跳过源码分析
```

**示例**:
```markdown
# Agent 提示词工程

## 实际问题场景

**问题**: Agent 在处理复杂任务时经常"迷失"方向，产生不相关输出

**症状**:
- 生成的代码与需求不符
- 跳过重要的验证步骤
- 在边缘情况下失败

---

## 解决方案：结构化提示词

**策略**: 使用 CLEAR 框架

[用户视角的使用方法]

---

## 源码实现：提示词模板系统

[可选：SuperPower 如何处理提示词]

> 💡 **仅限深入学习者**
```

### 2.3 参考文档性质

**定位**:
- 不是教程（按顺序学习）
- 是参考（按需查阅）
- 类似 API 文档的使用模式

**使用场景**:
- 遇到高级问题时查阅
- 想深入理解机制时学习
- 自定义系统时参考

**导航设计**:
```markdown
## 高级主题索引

### 遇到问题？

**Agent 行为异常** → @link(advanced/prompt-engineering)
**性能问题** → @link(advanced/performance)
**集成困难** → @link(advanced/integration)

### 深入学习？

**Agent 机制** → @link(advanced/agent-mechanics)
**Skill 设计** → @link(advanced/skill-patterns)
**插件开发** → @link(advanced/plugin-architecture)
```

### 2.4 渐进式源码深入

**三层深度**:
1. **用户层**: 如何使用
2. **机制层**: 如何工作
3. **源码层**: 具体实现

**示例**:
```markdown
## Agent 生命周期

### 用户层：如何创建 Agent

```bash
superpower brainstorm "Task"
```

### 机制层：Agent 如何启动

1. Command 解析
2. Skill 匹配
3. Agent 实例化
4. 上下文初始化
5. 任务执行

### 源码层：AgentFactory 实现

> 💡 **可选：深入源码**

```typescript
// src/core/agent-factory.ts
export async function createAgent(
  config: AgentConfig
): Promise<Agent> {
  // 实现...
}
```
```

### 2.5 边缘案例覆盖

**必须包含的边缘案例**:
- 并发冲突
- 资源耗尽
- 网络故障
- 依赖缺失
- 版本兼容

**格式**:
```markdown
## 边缘案例处理

### 案例 1: 并发 Agent 冲突

**场景**: 多个 Agent 同时修改同一文件

**症状**: 文件冲突，数据丢失

**解决方案**:
- 文件锁机制
- 乐观并发控制
- 冲突解决策略

**示例**:
[代码示例]
```

---

## 3. 接口约定

### 3.1 高级主题元数据 Schema

```yaml
---
id: advanced-prompt-engineering
title: Agent 提示词工程
level: advanced
estimatedTime: 60
category:
  primary: technical-depth  # 技术深度
  secondary: agent-mechanics
prerequisites:
  - knowledge: [agent-lifecycle, prompt-basics]
  - experience: [completed-core-workflows]
topics:
  - prompt-design-patterns
  - context-optimization
  - subagent-communication
verified:
  - markdown: L1
relatedTopics:
  - advanced/skill-patterns
  - advanced/agent-mechanics
optionalSections:
  - source-code-analysis
---
```

### 3.2 主题页面模板

```markdown
# [主题名称]

## 实际问题场景

**问题**: [描述]
**影响**: [为什么重要]
**频率**: [常见程度]

---

## 解决方案

[用户视角的解决方案]

### 策略 1: [名称]

[如何实施]

**示例**:
```bash @verified
[命令]
```

### 策略 2: [名称]

...

---

## 机制解析

[如何工作]

### 关键概念

- [概念 1]: [解释]
- [概念 2]: [解释]

### 流程图

```text
[ASCII 流程图]
```

---

## 源码实现

> 💡 **可选：仅限深入学习者**

[源码分析]

---

## 边缘案例

[常见边缘案例及处理]

---

## 最佳实践

[推荐做法]
[反模式警告]

---

## 相关主题

- @link(advanced/related-topic-1)
- @link(advanced/related-topic-2)
```

### 3.3 索引页结构

```markdown
# 高级主题深入

## 按问题查找

### Agent 行为异常
- [提示词工程](advanced/prompt-engineering.md)
- [上下文管理](advanced/context-management.md)

### 性能问题
- [性能调优](advanced/performance-tuning.md)
- [并发优化](advanced/concurrency.md)

### 集成困难
- [集成模式](advanced/integration-patterns.md)
- [API 网关](advanced/api-gateway.md)

### 扩展需求
- [插件架构](advanced/plugin-architecture.md)
- [自定义 Skill](advanced/custom-skills.md)

---

## 按主题深入学习

### 技术深度

**Agent 机制**
- [提示词工程](advanced/prompt-engineering.md)
- [子代理通信](advanced/subagent-communication.md)
- [上下文隔离](advanced/context-isolation.md)

**Skill 设计**
- [设计模式](advanced/skill-patterns.md)
- [触发条件](advanced/trigger-conditions.md)
- [参数传递](advanced/parameter-passing.md)

### 架构扩展

**插件系统**
- [插件架构](advanced/plugin-architecture.md)
- [生命周期](advanced/plugin-lifecycle.md)
- [依赖注入](advanced/dependency-injection.md)

**集成模式**
- [工具集成](advanced/tool-integration.md)
- [API 网关](advanced/api-gateway.md)
- [事件驱动](advanced/event-driven.md)

### 实战技巧

**调试**
- [性能分析](advanced/performance-analysis.md)
- [内存诊断](advanced/memory-diagnostics.md)
- [并发调试](advanced/concurrency-debugging.md)

**优化**
- [缓存策略](advanced/caching-strategies.md)
- [资源管理](advanced/resource-management.md)
- [性能调优](advanced/performance-tuning.md)
```

---

## 4. 约束与风险

### 4.1 技术约束

| 约束项 | 影响 | 缓解策略 |
|--------|------|---------|
| 内容深度 | 可能过于技术化 | 问题驱动，从场景开始 |
| 源码变更 | 源码分析可能过时 | 标注版本号，关注机制 |

### 4.2 内容风险

| 风险 | 影响 | 缓解策略 |
|------|------|---------|
| 认知过载 | 内容太深太难 | 明确标注"可选" |
| 范围蔓延 | 高级主题无限扩展 | 严格限定三类主题 |
| 维护成本 | 源码分析更新成本高 | 标注"可选"，降低优先级 |

### 4.3 教学风险

| 风险 | 缓解策略 |
|------|---------|
| 太技术化 | 问题驱动，从实际场景开始 |
| 边缘案例太多 | 分类展示，按需查阅 |
| 源码分析吓退用户 | 明确标注"可选" |

---

## 5. 验收标准

### 5.1 功能验收

- [ ] 三类主题完整覆盖
- [ ] 每个主题问题驱动
- [ ] 包含边缘案例
- [ ] 支持按需查阅

### 5.2 质量验收

- [ ] 通过 Markdown lint（L1）
- [ ] 代码示例标注 `@verified`
- [ ] ASCII 图表 76 字符宽度
- [ ] 源码标注版本号

### 5.3 UX 验收

- [ ] 索引清晰可查
- [ ] 问题导向导航
- [ ] 源码分析标注"可选"
- [ ] 边缘案例实用

### 5.4 教学验收

- [ ] 从场景到解决方案
- [ ] 机制解析清晰
- [ ] 源码分析准确
- [ ] 最佳实践可复用

---

## 6. 详细分析引用

### 6.1 跨角色共识

**来源**: `.cross-role-analysis.md` - F-007 章节

- **按需学习**: 高级主题不应该是强制路径
- **深度覆盖**: 提供完整的技术细节
- **参考性质**: 作为查阅资料

### 6.2 冲突解决记录

**决策点 1: 内容覆盖范围**
- **技术领域专家**: 提示词工程、自定义 Skill
- **系统架构师**: 扩展开发、插件架构
- **UX 专家**: 调试、调优、故障排除
- **解决方案**: 三类主题

**决策点 2: 讲解方式**
- **技术领域专家**: 深入源码
- **UX 专家**: 保持问题导向
- **解决方案**: 问题驱动 + 源码深入

### 6.3 增强建议应用

- **EP-001**: 渐进式深度标记
- **EP-004**: 概念依赖图导航

### 6.4 用户决策应用

- **UD-001**: 认知优先路径
  - 从用户可见的问题开始
- **UD-002**: 纯 ASCII 图表
- **UD-003**: L1（语法）验证

---

## 7. 跨功能依赖

### 7.1 前置依赖

**所有核心功能**（F-001 到 F-006）
- 假设用户已完成核心学习

### 7.2 后续功能

**扩展开发文档**（外部）
- 系统架构师的扩展文档

### 7.3 内部关联

**高级主题 ← → 核心概念**:
- 高级主题引用核心概念的进阶层
- 核心概念链接到高级主题

---

## 附录 A: 提示词工程主题示例

```markdown
# Agent 提示词工程

## 实际问题场景

**问题**: Agent 在处理复杂任务时经常"迷失"方向

**症状**:
- 生成的代码与需求不符
- 跳过重要的验证步骤
- 在边缘情况下失败

**影响**:
- 需要多次重试
- 降低开发效率
- 信任度下降

---

## 解决方案：结构化提示词

### CLEAR 框架

**C**oncise - 简洁
- 去除冗余信息
- 聚焦核心任务

**L**imited - 有限
- 明确任务边界
- 限定输出格式

**E**xplicit - 明确
- 详细说明要求
- 提供示例

**A**ctionable - 可操作
- 清晰的执行步骤
- 可验证的输出

**R**eviewable - 可审查
- 包含检查点
- 支持迭代改进

### 实施策略

#### 策略 1: 分阶段提示

```bash
# 阶段 1: 理解需求
superpower ask "分析以下需求：[需求]"

# 阶段 2: 生成方案
superpower ask "基于分析，生成实现方案"

# 阶段 3: 实现代码
superpower implement "[方案]"
```

#### 策略 2: 上下文注入

```markdown
# 规格文档
@link(specs/feature-spec.md)

# 相关代码
@link(src/impl/file.ts)

# 约束条件
- 必须通过 TDD 流程
- 遵循团队编码规范
- 包含错误处理

# 任务
实现上述规格
```

---

## 机制解析

### 提示词处理流程

```text
用户提示词
    │
    ▼
┌─────────────┐
│ 上下文收集   │ ← 相关文档、代码、历史
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ 提示词优化   │ ← CLEAR 框架处理
└──────┬──────┘
       │
       ▼
┌─────────────┐
│ Agent 执行   │
└──────┬──────┘
       │
       ▼
    输出结果
```

### 关键概念

**上下文窗口管理**
- Token 限制: ~100K tokens
- 优先级: 规格文档 > 相关代码 > 历史记录
- 压缩策略: 摘要长文档，保留关键代码

**提示词模板系统**
- 内置模板: brainstorm、spec、implementer
- 自定义模板: 用户可扩展
- 参数化: 支持变量替换

---

## 源码实现

> 💡 **可选：仅限深入学习者**

### PromptBuilder 实现

```typescript
// src/core/prompt-builder.ts
export class PromptBuilder {
  private context: Context[] = [];

  addContext(ctx: Context): this {
    this.context.push(ctx);
    return this;
  }

  build(template: string): string {
    return template
      .replace('{{context}}', this.formatContext())
      .replace('{{constraints}}', this.formatConstraints());
  }

  private formatContext(): string {
    // 按优先级排序上下文
    return this.context
      .sort((a, b) => b.priority - a.priority)
      .map(ctx => ctx.content)
      .join('\n\n');
  }
}
```

**关键点**:
- 上下文优先级管理
- 模板变量替换
- 链式调用设计

---

## 边缘案例

### 案例 1: 上下文溢出

**场景**: 文档超过 100K tokens

**症状**: 提示词被截断，信息丢失

**解决方案**:
1. **智能摘要**: 压缩长文档
2. **分块处理**: 大任务拆分
3. **优先级队列**: 只保留关键上下文

**示例**:
```bash
# 使用摘要模式
superpower brainstorm "[需求]" --context-mode=summary
```

### 案例 2: 提示词冲突

**场景**: 多个上下文源有矛盾信息

**症状**: Agent 输出不一致

**解决方案**:
1. **冲突检测**: 识别矛盾
2. **优先级解析**: 按来源权重
3. **显式声明**: 要求 Agent 确认

---

## 最佳实践

### ✅ 推荐做法

1. **结构化提示词**: 使用 CLEAR 框架
2. **上下文管理**: 只包含相关信息
3. **迭代优化**: 根据输出调整提示词
4. **模板复用**: 为常见任务创建模板

### ❌ 反模式

1. **提示词过长**: 超过必要信息
2. **模糊指令**: "优化代码"而不说明具体目标
3. **忽略上下文**: 不提供相关代码和文档
4. **一次性完美**: 期望第一次就得到完美输出

---

## 相关主题

- @link(advanced/context-management)
- @link(advanced/subagent-communication)
- @link(advanced/skill-patterns)
```

---

**规格版本**: 1.0
**最后更新**: 2026-03-30
**状态**: Final Draft
