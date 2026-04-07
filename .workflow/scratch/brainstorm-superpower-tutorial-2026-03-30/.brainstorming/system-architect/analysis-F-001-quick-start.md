# F-001 快速入门教程 - 架构分析

## 功能概述

**功能 ID**: F-001
**功能名称**: 快速入门教程
**目标**: 10 分钟内完成安装验证和第一个命令
**优先级**: P0（必须实现）

## 架构设计目标

### 用户体验目标

1. **零摩擦启动**
   - 从访问文档到第一个命令执行 < 5 分钟
   - 最小化前置依赖检查
   - 提供多平台安装指南（Windows/macOS/Linux）

2. **即时反馈验证**
   - 每个步骤完成后提供可验证的输出
   - 清晰的成功/失败状态指示
   - 友好的错误消息与修复建议

3. **认知负荷最小化**
   - 单次操作仅引入 1-2 个新概念
   - 使用渐进式信息披露
   - 提供上下文相关的帮助提示

### 技术架构目标

1. **跨平台兼容性**
   - 支持三大主流操作系统（Windows/macOS/Linux）
   - 处理平台特定的路径分隔符、权限问题
   - 提供 Shell 适配方案（bash/zsh/powershell）

2. **版本鲁棒性**
   - 支持多个 SuperPower 版本范围
   - 版本检查与升级提示机制
   - 向后兼容性保证

3. **离线友好**
   - 核心安装步骤可离线完成
   - 提供离线故障排除指南
   - 减少对外部服务的依赖

## 数据模型

### 安装状态跟踪

```typescript
interface InstallationState {
  platform: 'windows' | 'macos' | 'linux';
  shell: 'bash' | 'zsh' | 'powershell' | 'cmd';
  nodeVersion: string;              // Node.js 版本（如果需要）
  superpowerVersion: string;         // 已安装版本（如有）
  installPath: string;               // 安装路径
  configPath: string;                // 配置文件路径
  lastChecked: ISO8601;              // 版本检查时间
  status: 'not-installed' | 'installed' | 'outdated';
}

interface InstallationStep {
  id: string;                        // 步骤 ID
  name: string;                      // 步骤名称
  command: string;                   // 执行命令
  expectedOutput?: string;           // 预期输出（用于验证）
  validationCmd?: string;            // 验证命令
  estimatedTime: number;             // 预估耗时（秒）
  dependencies: string[];            // 前置步骤 ID
  platformSpecific?: {               // 平台特定配置
    [platform: string]: {
      command: string;
      notes?: string;
    }
  };
}
```

### 快速入门教程元数据

```typescript
interface QuickStartMetadata {
  id: 'F-001';
  title: string;
  estimatedTime: number;             // 总耗时（分钟）
  difficulty: 'beginner';
  prerequisites: Prerequisite[];
  steps: InstallationStep[];
  troubleshooting: TroubleshootingEntry[];
  successCriteria: SuccessCriterion[];
}

interface Prerequisite {
  type: 'software' | 'hardware' | 'knowledge';
  description: string;
  optional: boolean;
  checkCommand?: string;             // 验证命令
}

interface TroubleshootingEntry {
  problem: string;
  symptoms: string[];
  diagnosis: string;
  solution: string;
  relatedSteps: string[];            // 相关步骤 ID
}
```

### 用户进度跟踪

```typescript
interface QuickStartProgress {
  userId?: string;                   // 可选，支持匿名进度
  startedAt: ISO8601;
  completedSteps: string[];          // 已完成步骤 ID
  currentStep: string | null;        // 当前步骤 ID
  stuckAtStep?: string;              // 卡住的步骤 ID
  errorCount: number;                // 累计错误次数
  completedAt?: ISO8601;
  feedback?: string;                 // 用户反馈
}
```

## 状态机

### 安装流程状态机

```
┌─────────────┐
│  Initial    │  ← 用户访问快速入门页面
└──────┬──────┘
       │ check_prerequisites
       ↓
┌─────────────┐         ┌─────────────┐
│ Prerequisites│────────→│ MissingDep  │
└──────┬──────┘         └─────────────┘
       │ all_met
       ↓
┌─────────────┐
│ Install     │  ← 下载/安装 SuperPower
└──────┬──────┘
       │ install_complete
       ↓
┌─────────────┐         ┌─────────────┐
│  Verify     │────────→│  Failed     │
└──────┬──────┘         └─────────────┘
       │ version_ok
       ↓
┌─────────────┐
│ FirstCommand│  ← 执行第一个命令
└──────┬──────┘
       │ command_success
       ↓
┌─────────────┐
│ Complete    │  ← 快速入门完成
└─────────────┘
```

### 状态转换表

| 当前状态 | 事件 | 下一状态 | 动作 | 错误处理 |
|---------|------|---------|------|---------|
| Initial | check_prerequisites | Prerequisites | 运行环境检查 | 缺失依赖 → MissingDep |
| Prerequisites | all_met | Install | 显示安装指令 | - |
| MissingDep | install_dependency | Prerequisites | 引导安装依赖 | 安装失败 → 显示帮助 |
| Install | install_complete | Verify | 运行版本检查 | 安装失败 → Failed |
| Verify | version_ok | FirstCommand | 显示示例命令 | 版本过低 → 显示升级指南 |
| Verify | version_mismatch | Upgrade | 提示升级操作 | - |
| FirstCommand | command_success | Complete | 显示祝贺信息 | - |
| FirstCommand | command_failed | Debug | 进入故障排除 | - |
| Failed | retry | Verify | 重新验证 | - |
| Failed | skip_to_manual | ManualTroubleshooting | 显示手动安装指南 | - |

## 错误处理策略

### 安装失败处理

**检测机制**：
- 命令退出码检查（非零 = 失败）
- 超时检测（安装超时 > 5 分钟）
- 验证命令失败（安装后验证失败）

**错误分类**：

```typescript
enum InstallationErrorType {
  PermissionDenied = 'permission_denied',
  NetworkFailure = 'network_failure',
  DependencyConflict = 'dependency_conflict',
  UnsupportedPlatform = 'unsupported_platform',
  DiskSpace = 'disk_space_insufficient',
  Unknown = 'unknown'
}

interface InstallationError {
  type: InstallationErrorType;
  stepId: string;
  command: string;
  exitCode: number;
  stderr: string;
  suggestedFix: string;
  documentationLink?: string;
}
```

**恢复策略**：

| 错误类型 | 自动恢复 | 用户干预 | 文档指引 |
|---------|---------|---------|---------|
| PermissionDenied | 尝试 sudo | 提示手动提权 | 链接到权限指南 |
| NetworkFailure | 重试 3 次 | 提示检查网络 | 链接到离线安装 |
| DependencyConflict | 尝试清理缓存 | 提示手动卸载冲突版本 | 链接到冲突解决 |
| UnsupportedPlatform | - | 显示不支持消息 | 建议替代方案 |
| DiskSpace | - | 提示清理空间 | 显示空间要求 |

### 验证失败处理

**场景**：安装成功但验证命令失败

**诊断流程**：
```bash
# 1. 检查 PATH 环境变量
$ which superpower
# 失败 → 添加到 PATH 指南

# 2. 验证文件完整性
$ ls -l $(which superpower)
# 失败 → 重新安装指南

# 3. 检查依赖
$ superpower --version
# 失败 → 依赖检查指南
```

**自动修复尝试**：
- PATH 问题：提供 Shell 配置代码片段
- 权限问题：提供 chmod 命令
- 缓存问题：提供清理命令

## 可观测性指标

### 教学效果指标（5+）

1. **完成率**：完成快速入门的用户数 / 访问用户数 × 100%
   - 目标：> 60%
   - 监控：每日聚合

2. **平均完成时间**：从开始到完成的平均耗时
   - 目标：< 15 分钟（包含缓冲）
   - 监控：中位数 + p95

3. **步骤卡点**：每个步骤的停留时长
   - 目标：单步骤 < 3 分钟
   - 监控：识别耗时 > 5 分钟的步骤

4. **错误率**：遇到错误的用户比例
   - 目标：< 20%
   - 监控：按错误类型分类

5. **回访率**：完成快速入门后 7 日内回访比例
   - 目标：> 40%
   - 监控：每周分析

### 技术性能指标

1. **页面加载时间**：快速入门页面首屏渲染时间
   - 目标：< 2 秒（p95）
   - 监控：真实用户监控（RUM）

2. **命令示例可复制性**：代码块复制成功率
   - 目标：> 95%
   - 监控：复制按钮点击率

3. **跨平台成功率**：各平台完成率对比
   - 目标：所有平台 > 55%
   - 监控：按平台分类

### 用户参与度指标

1. **帮助文档点击率**：用户点击"了解更多"的比例
   - 目标：< 30%（快速入门应自包含）
   - 监控：链接点击追踪

2. **故障排除页面访问率**：进入故障排除流程的比例
   - 目标：< 15%
   - 监控：页面跳转分析

## 配置模型

### 快速入门配置

```typescript
interface QuickStartConfig {
  versioning: {
    minVersion: string;             // 最低支持版本
    recommendedVersion: string;     // 推荐版本
    checkUpdate: boolean;           // 是否检查更新
  };
  platforms: {
    [platform: string]: PlatformConfig;
  };
  steps: {
    autoDetectPlatform: boolean;    // 自动检测平台
    skipOptionalSteps: boolean;     // 跳过可选步骤
    allowSkipPrerequisites: boolean; // 允许跳过前置检查
  };
  feedback: {
    collectAnonymousMetrics: boolean; // 收集匿名指标
    showProgressIndicator: boolean;   // 显示进度指示器
  };
}

interface PlatformConfig {
  installCommand: string;
  verifyCommand: string;
  firstCommand: string;
  pathSetup?: string;                // PATH 配置代码
  troubleshootingUrl: string;
  aliases?: { [alias: string]: string }; // 命令别名
}
```

### 示例配置

```json
{
  "versioning": {
    "minVersion": "5.0.0",
    "recommendedVersion": "5.0.6",
    "checkUpdate": true
  },
  "platforms": {
    "macos": {
      "installCommand": "brew install superpower",
      "verifyCommand": "superpower --version",
      "firstCommand": "superpower brainstorm \"Hello World\"",
      "pathSetup": "export PATH=\"$PATH:/opt/homebrew/bin\""
    },
    "windows": {
      "installCommand": "winget install superpower",
      "verifyCommand": "superpower --version",
      "firstCommand": "superpower brainstorm \"Hello World\"",
      "troubleshootingUrl": "/docs/troubleshooting/windows"
    }
  }
}
```

## 边界场景处理

### 场景 1：离线环境安装

**挑战**：用户无法访问互联网或软件仓库

**策略**：
- 提供离线安装包下载链接
- 提供手动安装步骤（复制二进制文件）
- 提供离线验证命令

**约束**：
- 离线安装包大小 SHOULD < 50MB
- MUST 提供离线故障排除指南

### 场景 2：企业防火墙限制

**挑战**：企业网络阻止下载或执行

**策略**：
- 提供免安装的便携版本
- 提供代理配置说明
- 列出需要白名单的域名

**约束**：
- SHOULD 支持环境变量配置代理
- MAY 提供企业版安装指南

### 场景 3：过时的系统版本

**挑战**：用户使用不受支持的旧系统

**策略**：
- 提前显示系统要求
- 提供最后兼容版本信息
- 引导升级系统或使用替代方案

**约束**：
- MUST 在安装前检查系统版本
- SHOULD 提供清晰的降级方案

### 场景 4：并发安装冲突

**挑战**：用户已有其他版本或类似工具

**策略**：
- 检测现有安装
- 提供并行安装配置
- 提供版本切换指南

**约束**：
- MUST 警告覆盖现有安装
- SHOULD 支持多版本共存

### 场景 5：辅助功能需求

**挑战**：视障、运动障碍用户需要特殊支持

**策略**：
- 提供屏幕阅读器友好的内容
- 支持键盘导航
- 提供大字体/高对比度版本

**约束**：
- MUST 符合 WCAG 2.1 AA 级别
- SHOULD 通过辅助功能测试

## 可扩展性设计

### 国际化支持（i18n）

**架构预留**：
```typescript
interface LocalizedQuickStart {
  locale: string;
  title: string;
  installCommand: string;
  troubleshooting: LocalizedTroubleshooting[];
}

interface LocalizedTroubleshooting {
  problem: string;
  solution: string;
  culturalNotes?: string;            // 文化相关说明
}
```

**支持语言**：
- 初始：英文、中文
- 未来：西班牙语、日语、法语

### 平台扩展

**新平台集成流程**：
1. 添加平台检测逻辑
2. 提供平台特定安装命令
3. 编写平台特定故障排除指南
4. 在文档中添加平台标签

**示例**：
```typescript
function detectPlatform(): Platform {
  const ua = navigator.userAgent;
  if (ua.includes('Win')) return 'windows';
  if (ua.includes('Mac')) return 'macos';
  if (ua.includes('Linux')) return 'linux';
  return 'unknown';
}
```

## 性能优化

### 页面加载优化

**策略**：
- 延迟加载非关键内容（故障排除、高级选项）
- 预加载下一步内容
- 代码分割：平台特定内容按需加载

**目标**：
- 首次内容绘制（FCP）< 1.5 秒
- 交互时间（TTI）< 3 秒

### 命令执行优化

**策略**：
- 并行运行独立的验证命令
- 缓存版本检查结果（5 分钟有效期）
- 使用 Web Worker 执行耗时操作

**示例**：
```typescript
async function parallelCheck(commands: string[]): Promise<Result[]> {
  return Promise.all(
    commands.map(cmd => executeCommand(cmd))
  );
}
```

## 安全考虑

### 代码执行安全

**风险**：用户盲目复制执行命令

**缓解**：
- 所有命令示例经过安全审查
- 不使用 `sudo` 除非绝对必要
- 提供命令解释说明

**约束**：
- MUST 避免 destructive 操作（rm、format 等）
- SHOULD 使用 `--dry-run` 等安全标志

### 依赖安全

**策略**：
- 验证下载包的校验和
- 使用 HTTPS 下载
- 提供源代码编译选项（可选）

## 测试策略

### 自动化测试

**测试类型**：
1. **命令有效性测试**：所有示例命令可执行
2. **跨平台测试**：在三大平台验证
3. **链接完整性测试**：所有文档链接有效
4. **可访问性测试**：WCAG 2.1 AA 合规

**测试框架**：
- Playwright（跨浏览器测试）
- axe-core（可访问性）
- Custom validators（命令验证）

### 手动验证清单

**发布前检查**：
- [ ] 在 Windows/macOS/Linux 上完整走一遍流程
- [ ] 使用全新安装环境验证
- [ ] 从旧版本升级验证
- [ ] 所有外部链接有效
- [ ] 屏幕阅读器测试通过

---

**文档版本**：1.0.0
**最后更新**：2026-03-30
**架构师**：System Architect
**状态**：Final Draft
