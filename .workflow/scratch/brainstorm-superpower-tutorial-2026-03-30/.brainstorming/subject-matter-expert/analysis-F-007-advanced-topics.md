# F-007: 高级主题深入技术分析

## 技术深度评估：⭐⭐⭐⭐⭐

**定位：** F-007 是从"使用者"到"专家"的关键跃升，涵盖 SuperPower 的核心技术和扩展能力。

## 高级主题分解

### 1. 自定义技能开发

**技术复杂度：⭐⭐⭐⭐⭐**

**核心技术：**
- Front Matter 元数据设计
- 工作流结构化
- 技能测试方法论
- 技能组合模式

**技术架构：**
```markdown
技能文件结构：
---
name: skill-name              # 技能唯一标识符
description: Use when...      # 触发条件描述
---

<SUBAGENT-STOP>              # 子代理停止标记（可选）
条件逻辑：是否在子代理模式下跳过此技能
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>        # 优先级提升（可选）
强调此技能的关键性
</EXTREMELY-IMPORTANT>

## 技能内容
- 执行步骤
- 决策树
- 验证清单
- 红旗检测
```

**技能类型分类：**

**1. 流程控制技能（Process Control）**
```yaml
---
name: my-workflow
description: Use when [specific condition]
---
```
- 特点：决定如何接近任务
- 优先级：最高
- 示例：brainstorming, systematic-debugging

**2. 实现指导技能（Implementation Guide）**
```yaml
---
name: my-pattern
description: Use when implementing [specific feature]
---
```
- 特点：指导具体实现
- 优先级：中等
- 示例：test-driven-development, frontend-design

**3. 领域专精技能（Domain Specific）**
```yaml
---
name: my-domain-skill
description: Use when working with [specific technology]
---
```
- 特点：封装领域知识
- 优先级：低（触发时优先级高）
- 示例：mcp-builder, api-design

**技能触发机制：**
```
用户输入 → 意图分析 → 技能匹配度评估
                            ↓
                  匹配度 ≥ 1% ?
                            ↓ 是
                      加载技能内容
                            ↓
                      执行技能步骤
                            ↓
                      返回结果
```

**技能测试方法论：**
```markdown
## 测试维度

### 1. 触发测试
- [ ] 技能在正确条件下触发
- [ ] 技能在错误条件下不触发
- [ ] 技能优先级正确

### 2. 执行测试
- [ ] 技能步骤完整执行
- [ ] 决策逻辑正确
- [ ] 错误处理恰当

### 3. 集成测试
- [ ] 与其他技能兼容
- [ ] 不会冲突或死循环
- [ ] 正确传递上下文

### 4. 效果测试
- [ ] 技能达到预期目标
- [ ] 质量指标改善
- [ ] 用户体验提升
```

### 2. 子代理调度策略

**技术复杂度：⭐⭐⭐⭐**

**核心技术：**
- 上下文构建
- 模型选择算法
- 状态机处理
- 失败恢复机制

**上下文构建技术：**
```markdown
## 最小化上下文原则

### 必需信息
- 任务描述（完整文本）
- 相关文件（完整内容）
- 依赖关系（明确说明）
- 验证标准（可执行步骤）

### 禁止信息
- 主会话历史（避免污染）
- 无关文件（减少噪音）
- 早期决策（避免偏差）
- 推测信息（避免误导）

### 上下文模板
```typescript
interface SubagentContext {
  // 任务信息
  task: {
    description: string;      // 完整任务描述
    requirements: string[];   // 明确要求
    constraints: string[];    // 约束条件
    verification: string[];   // 验证步骤
  };

  // 环境信息
  environment: {
    projectRoot: string;      // 项目根目录
    branch: string;           // 当前分支
    files: FileContext[];     // 相关文件
  };

  // 依赖信息
  dependencies: {
    previousTasks: string[];  // 前置任务
    relatedWork: string[];    // 相关工作
    standards: string[];      // 适用标准
  };
}
```

**模型选择算法：**
```markdown
## 渐进式模型选择

### 复杂度评估
```
任务复杂度 = 文件数量 × 集成度 × 创新度

文件数量：
- 1-2 文件：简单 (1.0)
- 3-5 文件：中等 (1.5)
- 6+ 文件：复杂 (2.0)

集成度：
- 独立函数：低 (1.0)
- 模块内集成：中 (1.5)
- 跨模块集成：高 (2.0)

创新度：
- 遵循模式：低 (1.0)
- 适配模式：中 (1.5)
- 新模式：高 (2.0)
```

### 模型映射
```
复杂度 < 3.0 → 快速模型 (成本优先)
复杂度 3.0-6.0 → 标准模型 (平衡)
复杂度 > 6.0 → 强力模型 (质量优先)
```

### 示例
```
任务 1：实现单个函数（2 文件，低集成，遵循模式）
复杂度 = 1.0 × 1.0 × 1.0 = 1.0
选择：快速模型

任务 2：重构模块（4 文件，中集成，适配模式）
复杂度 = 1.5 × 1.5 × 1.5 = 3.375
选择：标准模型

任务 3：设计新架构（8 文件，高集成，新模式）
复杂度 = 2.0 × 2.0 × 2.0 = 8.0
选择：强力模型
```

**状态机处理：**
```markdown
## 子代理状态转换

```
                    ┌─────────────────┐
                    │  NEEDS_CONTEXT  │
                    │  (需要更多上下文)  │
                    └────────┬────────┘
                             │ 提供信息
                             │ 重新分派
                             ↓
┌─────────────────┐    ┌─────────────────┐
│  DONE (完成)     │ ←─ │    DISPATCH     │
└────────┬────────┘    │  (初始分派)       │
         │             └────────┬────────┘
         │ 审查                   │
         ↓                      ↓
┌─────────────────┐    ┌─────────────────┐
│  DONE_WITH_     │    │   BLOCKED       │
│  CONCERNS       │    │  (被阻塞)        │
│  (有疑虑完成)     │    └─────────────────┘
└─────────────────┘
         │
         │ 评估疑虑
         ↓
    ┌────────┐
    │ 继续   │ ← 疑虑是观察性质
    │ 审查   │
    └────────┘
```

### 状态处理逻辑
- **DONE**：直接进入审查流程
- **DONE_WITH_CONCERNS**：
  - 如果是正确性疑虑：处理后再审查
  - 如果是观察性质：记录后继续
- **NEEDS_CONTEXT**：提供缺失信息，重新分派同一模型
- **BLOCKED**：
  - 上下文问题：提供更多信息，重新分派
  - 能力问题：升级到更强模型
  - 任务过大：拆分任务
  - 计划错误：升级给人
```

**失败恢复机制：**
```markdown
## 失败分类和处理

### 1. 上下文失败
**症状：** 子代理请求更多信息
**处理：** 补充信息，重新分派同一模型
**预防：** 初始上下文尽可能完整

### 2. 能力失败
**症状：** 子代理无法完成任务
**处理：** 升级到更强模型，重新分派
**预防：** 复杂度评估更准确

### 3. 任务失败
**症状：** 子代理报告 BLOCKED
**处理：** 评估阻塞原因，可能拆分任务
**预防：** 任务粒度更细

### 4. 计划失败
**症状：** 多次重试仍失败
**处理：** 升级给人，重新规划
**预防：** 计划阶段更仔细
```

### 3. 工作流组合模式

**技术复杂度：⭐⭐⭐⭐**

**核心技术：**
- 技能依赖图
- 执行顺序约束
- 上下文传递
- 错误传播

**技能依赖分析：**
```markdown
## 依赖关系图

```
brainstorming (头脑风暴)
    ↓ 产生设计文档
    ↓ 约束：必须完成
writing-plans (计划编写)
    ↓ 产生实施计划
    ↓ 约束：基于已批准设计
using-git-worktrees (Git 工作树)
    ↓ 创建隔离环境
    ↓ 约束：必须有计划
subagent-driven-development (子代理驱动开发)
    ├── test-driven-development (TDD)
    │   └─ 约束：每个任务必须遵循
    ├── requesting-code-review (代码审查)
    │   └─ 约束：每个任务完成后
    └─ systematic-debugging (系统调试)
        └─ 约束：遇到问题时
finishing-a-development-branch (完成分支)
    └─ 约束：所有任务完成
```

## 依赖类型

### 1. 强依赖（Must Complete）
- brainstorming → writing-plans
- writing-plans → using-git-worktrees
- using-git-worktrees → subagent-driven-development

### 2. 弱依赖（Should Complete）
- test-driven-development → requesting-code-review
- subagent-driven-development → finishing-a-development-branch

### 3. 条件依赖（Conditional）
- systematic-debugging（仅在遇到问题时）
- receiving-code-review（仅在收到反馈时）
```

**组合模式：**
```markdown
## 常见工作流组合

### 模式 1：标准开发流程
```
brainstorming → writing-plans → using-git-worktrees
→ subagent-driven-development → finishing-a-development-branch
```
**适用：** 新功能开发
**特点：** 完整流程，质量保证

### 模式 2：快速修复流程
```
systematic-debugging → test-driven-development
→ receiving-code-review
```
**适用：** Bug 修复
**特点：** 快速迭代，问题聚焦

### 模式 3：探索性开发流程
```
brainstorming → using-git-worktrees
→ (手动探索) → writing-plans → subagent-driven-development
```
**适用：** 技术不确定
**特点：** 探索先行，计划后置

### 模式 4：并行开发流程
```
brainstorming → writing-plans (多个并行计划)
→ (并行) using-git-worktrees + subagent-driven-development
→ finishing-a-development-branch (合并)
```
**适用：** 独立功能
**特点：** 并行执行，效率优先
```

**上下文传递机制：**
```markdown
## 数据流分析

### 设计 → 计划
传递数据：
- 设计决策
- 约束条件
- 验证标准

### 计划 → 执行
传递数据：
- 任务列表
- 文件结构
- 依赖关系

### 执行 → 审查
传递数据：
- 实现代码
- 测试结果
- Git SHA

### 审查 → 修复
传递数据：
- 问题列表
- 严重程度
- 修复建议
```

### 4. 性能优化技术

**技术复杂度：⭐⭐⭐⭐**

**核心技术：**
- 子代理缓存
- 增量执行
- 并行化策略
- 资源管理

**子代理缓存：**
```markdown
## 缓存策略

### 可缓存内容
- 技能内容（不常变化）
- 计划文档（执行前稳定）
- 标准模板（重复使用）

### 缓存失效
- 技能更新
- 计划变更
- 模板改进

### 缓存实现
```typescript
interface CacheEntry {
  key: string;
  content: any;
  timestamp: number;
  ttl: number;        // 生存时间
}

class SkillCache {
  private cache: Map<string, CacheEntry>;

  get(key: string): any | null {
    const entry = this.cache.get(key);
    if (!entry) return null;

    if (Date.now() - entry.timestamp > entry.ttl) {
      this.cache.delete(key);
      return null;
    }

    return entry.content;
  }

  set(key: string, content: any, ttl: number): void {
    this.cache.set(key, {
      key,
      content,
      timestamp: Date.now(),
      ttl
    });
  }
}
```

**增量执行：**
```markdown
## 增量执行策略

### 任务级别增量
- 已完成任务跳过
- 失败任务重试
- 新任务执行

### 文件级别增量
- 未修改文件跳过
- 已修改文件重测
- 新文件全测

### 检测点机制
```
检查点 1: 任务 1 完成
检查点 2: 任务 2 完成
检查点 3: 任务 3 完成
...

失败后从最近检查点恢复
```

**并行化策略：**
```markdown
## 并行执行模式

### 任务并行（独立任务）
```
任务 1 → 子代理 A ─┐
                   ├→ 并行执行
任务 2 → 子代理 B ─┘
```
**条件：** 任务无依赖
**风险：** Git 冲突
**缓解：** 工作树隔离

### 审查并行（不同维度）
```
实现 → 规范审查 ─┐
                ├→ 并行审查
实现 → 质量审查 ─┘
```
**条件：** 审查维度独立
**风险：** 问题冲突
**缓解：** 合并问题列表

### 流水线并行（阶段重叠）
```
任务 1 执行 → 任务 1 审查
      ↓
任务 2 执行 → 任务 2 审查
      ↓
任务 3 执行 → 任务 3 审查
```
**条件：** 任务间有依赖但可流水线
**风险：** 错误传播
**缓解：** 快速反馈
```

**资源管理：**
```markdown
## 资源优化策略

### 模型资源
- 简单任务用快速模型
- 复杂任务用强力模型
- 批量任务用混合模型

### 上下文资源
- 精确构建上下文
- 避免过度传递
- 及时清理缓存

### 并发资源
- 限制并发数量
- 避免资源竞争
- 优雅降级策略

### 成本控制
- 监控 token 使用
- 优化提示长度
- 缓存重复内容
```

## 示例代码规划

### 1. 自定义技能示例

**目标：** 展示技能开发的完整流程

```markdown
# 示例：创建 API 设计技能

## 技能文件结构
```yaml
---
name: api-design
description: Use when designing REST APIs, GraphQL schemas,
             or RPC interfaces before implementation
---

# API Design

## Overview
Design APIs that are intuitive, consistent, and maintainable.
Follow REST best practices for resource-oriented APIs.

## Design Checklist

### 1. Resource Modeling
- [ ] Identify resources (nouns)
- [ ] Define resource relationships
- [ ] Establish resource hierarchies

### 2. Endpoint Design
- [ ] Use appropriate HTTP methods
- [ ] Design consistent URLs
- [ ] Plan pagination and filtering

### 3. Request/Response
- [ ] Define request schemas
- [ ] Define response schemas
- [ ] Plan error responses

### 4. Documentation
- [ ] Document all endpoints
- [ ] Provide examples
- [ ] Include error scenarios

## Common Patterns

### RESTful Conventions
```
GET    /users          # List users
GET    /users/{id}     # Get user
POST   /users          # Create user
PUT    /users/{id}     # Update user
DELETE /users/{id}     # Delete user
```

### Response Format
```json
{
  "data": { ... },
  "meta": {
    "page": 1,
    "perPage": 20,
    "total": 100
  }
}
```

## Red Flags

- ❌ Using verbs in URLs (e.g., /getUsers)
- ❌ Inconsistent naming (e.g., user_id vs userId)
- ❌ Missing error handling
- ❌ No pagination for large datasets
```

## 测试文件
```typescript
describe('api-design skill', () => {
  test('triggers when designing API', async () => {
    const input = "I need to design a user management API";
    const shouldTrigger = await skillMatches(input);
    expect(shouldTrigger).toBe(true);
  });

  test('does not trigger when implementing', async () => {
    const input = "Implement the user API";
    const shouldTrigger = await skillMatches(input);
    expect(shouldTrigger).toBe(false);
  });

  test('provides REST guidance', async () => {
    const guidance = await executeSkill(input);
    expect(guidance).toContain('REST');
    expect(guidance).toContain('HTTP methods');
  });
});
```
```

### 2. 复杂工作流组合示例

**目标：** 展示多个技能的组合使用

```markdown
# 示例：实现微服务架构

## 工作流设计
```
阶段 1: 架构设计
├─ brainstorming (明确需求)
├─ api-design (设计服务接口)
└─ writing-plans (分解实施计划)

阶段 2: 并行实现
├─ 服务 A (工作树 1)
│  ├─ subagent-driven-development
│  ├─ test-driven-development
│  └─ requesting-code-review
├─ 服务 B (工作树 2)
│  ├─ subagent-driven-development
│  ├─ test-driven-development
│  └─ requesting-code-review
└─ 服务 C (工作树 3)
   ├─ subagent-driven-development
   ├─ test-driven-development
   └─ requesting-code-review

阶段 3: 集成测试
├─ finishing-a-development-branch (每个服务)
└─ systematic-debugging (集成问题)

阶段 4: 部署
└─ verification-before-completion
```

## 执行脚本
```typescript
async function implementMicroservices() {
  // 阶段 1：架构设计
  await brainstorming("Design user management microservice");
  await apiDesign("Define service interfaces");
  const plan = await writingPlans("Create implementation plan");

  // 阶段 2：并行实现
  const services = ['auth', 'user', 'notification'];
  const worktrees = await Promise.all(
    services.map(service => createWorktree(service))
  );

  const implementations = await Promise.all(
    worktrees.map(async (worktree, index) => {
      const service = services[index];
      return await subagentDrivenDevelopment(
        plan.tasks[service],
        worktree
      );
    })
  );

  // 阶段 3：集成测试
  await Promise.all(
    worktrees.map(worktree =>
      finishingDevelopmentBranch(worktree)
    )
  );

  // 阶段 4：验证
  await verificationBeforeCompletion();
}
```
```

## 潜在陷阱警告

### 1. 技能过度设计

**陷阱：** 试图创建"万能技能"
- **症状：** 技能描述过于宽泛，触发条件模糊
- **后果：** 技能误触发，干扰正常流程
- **正确做法：** 专注特定场景，精确触发条件

### 2. 子代理上下文过度

**陷阱：** 传递过多信息给子代理
- **症状：** 子代理响应慢，成本高
- **后果：** 效率低下，可能偏离任务
- **正确做法：** 精确构建最小化上下文

### 3. 工作流死锁

**陷阱：** 技能循环依赖
- **症状：** A 触发 B，B 触发 A
- **后果：** 无限循环，无法执行
- **正确做法：** 依赖关系设计为有向无环图（DAG）

### 4. 性能优化过早

**陷阱：** 过早优化缓存和并行
- **症状：** 复杂度高，收益不明显
- **后果：** 维护成本高，引入 bug
- **正确做法：** 先正确实现，再优化性能

## 学习检查点

学完 F-007 后，学习者应该能够：

1. **开发自定义技能**
   - 设计技能元数据和触发条件
   - 编写技能内容和决策逻辑
   - 测试技能的触发和执行

2. **优化子代理调度**
   - 精确构建子代理上下文
   - 选择合适的模型
   - 处理各种子代理状态

3. **组合工作流**
   - 设计复杂的工作流组合
   - 处理技能依赖关系
   - 管理数据流和上下文传递

4. **优化性能**
   - 实施缓存策略
   - 并行化独立任务
   - 管理资源使用

5. **诊断问题**
   - 识别技能触发问题
   - 解决子代理失败
   - 处理工作流死锁

## 技术深度总结

F-007 是从用户到专家的跃升。重点是：

1. **理解底层机制**：不只是使用，而是理解如何工作
2. **扩展能力**：创建自定义技能和流程
3. **优化效率**：通过技术手段提升性能
4. **解决问题**：诊断和修复复杂问题

掌握这些高级主题后，学习者可以：
- 创建符合团队需求的定制技能
- 优化 SuperPower 的使用效率
- 解决复杂的技术挑战
- 为 SuperPower 项目做出贡献
