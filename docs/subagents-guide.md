# Subagents 使用指南

## 什么是 Subagent？

Subagents（子代理）是由主 Agent 创建的、具有特定目的的 AI 助手，可以：
- 独立执行被分配的任务
- 在后台并行运行
- 返回结构化的结果给主代理

## 创建 Subagents 的方式

### 方式 1：通过主代理委派

主代理会自动分析你的请求，决定是否需要创建子代理。

```bash
# 例子：自动创建子代理进行分析
"请同时分析 auth.ts、database.ts 和 api.ts 这三个文件"

# 主代理可能会：
# - 创建 Subagent 1：分析 auth.ts
# - 创建 Subagent 2：分析 database.ts  
# - 创建 Subagent 3：分析 api.ts
```

### 方式 2：明确请求子代理

```bash
# 显式要求使用子代理
"使用 Explore Agent 同时搜索这些代码库..."
"使用 Task Agent 并行运行测试..."
```

## Subagent 生命周期

```
创建 → 初始化 → 执行 → 监控 → 完成 → 收集结果
 ↓      ↓       ↓      ↓      ↓      ↓
新建  设置环境  运行  进度跟踪 清理  汇总
```

### 1️⃣ 创建（Creation）
```bash
# 主代理决定需要子代理时自动创建
条件：
- 有多个独立的子任务
- 任务之间无依赖
- 使用 /fleet 模式
```

### 2️⃣ 初始化（Initialization）
```bash
# 子代理获取运行所需的上下文
- 项目信息
- 文件系统访问权限
- 相关工具集
```

### 3️⃣ 执行（Execution）
```bash
# 子代理独立执行被分配的任务
- 不需要与主代理通信
- 可以调用其专属的工具集
- 运行时间不受主代理影响
```

### 4️⃣ 监控（Monitoring）
```bash
# 通过 /tasks 命令跟踪进度
/tasks          # 查看所有运行的子代理

# 输出例：
# Agent 1: Analyzing auth.ts ... 60% complete
# Agent 2: Analyzing database.ts ... 100% complete
# Agent 3: Analyzing api.ts ... 30% complete
```

### 5️⃣ 完成（Completion）
```bash
# 子代理任务完成时自动清理
- 保存结果
- 释放资源
- 准备返回结果
```

### 6️⃣ 结果收集（Collection）
```bash
# 主代理汇总所有子代理的结果
- 合并输出
- 解决冲突
- 生成最终报告
```

## 与子代理通信

### 创建时提供信息

```bash
✅ 清晰的子代理指令：
"Agent 1: 审查 src/auth/auth.ts
- 检查：安全漏洞、错误处理、边界情况
- 格式：JSON 格式的发现列表
- 标准：遵循 OWASP 安全标准"

❌ 模糊的指令：
"检查代码质量"
```

### 优化指令结构

```bash
# 最有效的子代理指令包括：

1. 清晰的目标
   "分析前端组件性能"

2. 具体的范围
   "仅关注 src/components/ 目录"

3. 检查清单
   - 渲染性能
   - 内存泄漏
   - 不必要的重新渲染

4. 输出格式
   "以 Markdown 表格格式返回结果"

5. 成功标准
   "找出性能瓶颈并提出改进建议"
```

## 最佳实践

### ✅ 何时创建子代理

1. **多个独立任务**
   ```bash
   # 创建 3 个子代理
   Task A: 构建前端
   Task B: 构建后端
   Task C: 运行测试
   ```

2. **长时间运行的任务**
   ```bash
   # 子代理在后台运行，主代理继续其他工作
   子代理：npm run build
   主代理：同时做其他事情
   ```

3. **需要不同能力的任务**
   ```bash
   Explore Agent: 搜索和分析
   Task Agent: 运行命令
   Code Review Agent: 审查代码
   ```

### ❌ 何时不用子代理

1. **简单、快速的任务**
   ```bash
   ❌ 不好：为 1 行改动创建子代理
   ✅ 好：直接进行修改
   ```

2. **需要频繁交互的任务**
   ```bash
   ❌ 不好：创建子代理处理需要主代理决策的任务
   ✅ 好：由主代理直接处理
   ```

3. **任务之间有强依赖**
   ```bash
   ❌ 不好：A 必须完成 B 才能开始，却用并行子代理
   ✅ 好：按顺序进行
   ```

## 实战例子

### 例子 1：代码审查工作流

```bash
# 需求：审查 PR 的 3 个主要部分

# 命令
启用 /fleet
"请创建 3 个 Code Review Agents 分别审查：
1. 认证逻辑（auth/）- 查找安全问题
2. API 端点（api/）- 查找设计问题
3. 数据模型（models/）- 查找性能问题"

# 系统执行
主代理 ✓
├─ Subagent 1：审查 auth/ ✓
├─ Subagent 2：审查 api/ ✓
└─ Subagent 3：审查 models/ ✓

# 结果
5 分钟获得 3 个完整的审查报告（vs 顺序执行的 15 分钟）
```

### 例子 2：多模块构建

```bash
# 需求：同时构建 5 个独立的微服务

# 命令
启用 /fleet
"为这 5 个微服务创建 Task Agents 并行构建：
- service-users
- service-products
- service-orders
- service-payments
- service-notifications"

# 系统执行
主代理 ✓
├─ Subagent 1：构建 service-users ✓
├─ Subagent 2：构建 service-products ✓
├─ Subagent 3：构建 service-orders ✓
├─ Subagent 4：构建 service-payments ✓
└─ Subagent 5：构建 service-notifications ✓

# 结果
10 分钟全部完成（vs 顺序的 50 分钟）
```

### 例子 3：研究和学习

```bash
# 需求：研究 3 个不同的实现方式

# 命令
启用 /fleet
"创建 3 个 Explore Agents 研究：
1. GraphQL vs REST API 设计
2. 关系型 vs NoSQL 数据库
3. 微服务 vs 单体架构"

# 系统执行
主代理 ✓
├─ Subagent 1：研究 GraphQL/REST ✓
├─ Subagent 2：研究数据库选择 ✓
└─ Subagent 3：研究架构模式 ✓

# 结果
并行收集对比分析（而非按顺序等待）
```

## 故障排除

| 问题 | 症状 | 解决方案 |
|------|------|---------|
| 子代理无响应 | 长时间卡顿 | 使用 /tasks 检查，可能需要重新启动 |
| 结果不完整 | 某个子代理返回错误 | 检查指令是否清晰，提供更多上下文 |
| 内存溢出 | 系统变慢 | 减少并行子代理数量（5-8 最佳） |
| 权限错误 | 子代理无法访问文件 | 检查目录权限设置 |

## 学习检查清单

- [ ] 理解子代理的生命周期
- [ ] 知道何时使用和不使用子代理
- [ ] 能够编写清晰的子代理指令
- [ ] 掌握监控子代理进度的方法
- [ ] 能够设计多子代理工作流

---

下一步：[Fleet Mode 指南](./fleet-mode.md) 或 [Agent 基础知识](./agent-basics.md)
