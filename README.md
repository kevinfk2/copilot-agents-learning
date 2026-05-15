# GitHub Copilot CLI Agents Learning Project

一个专门用来学习和实践 GitHub Copilot CLI agents 的项目。

## 📚 学习路径

### Level 1: 基础概念
- [ ] 理解 agents 的核心概念
- [ ] 学习子代理（subagents）的工作原理
- [ ] 掌握任务委派和并行执行

### Level 2: 实际应用
- [ ] 创建简单的 agent 工作流
- [ ] 使用 `/fleet` 实现并行任务
- [ ] 学习任务跟踪和管理

### Level 3: 高级用法
- [ ] 自定义 agent 指令
- [ ] 集成 MCP servers
- [ ] 构建复杂的多代理工作流

## 🔧 主要命令速查表

### Agent 相关
```bash
/agent           # 浏览和选择可用的代理
/fleet           # 启用并行子代理模式
/tasks           # 查看和管理所有任务
/delegate        # 将任务提交给 GitHub
```

### 工作流优化
```bash
/plan            # 创建实现计划
/review          # 运行代码审查代理
/init            # 初始化项目的 Copilot 指令
```

### 诊断和管理
```bash
/env             # 查看已加载的 agents、skills、MCP 服务
/tasks           # 监控运行中的任务
/session         # 管理会话
```

## 💡 最佳实践

### 何时使用 Agents
✅ **适合使用**
- 复杂任务可以分解成多个独立子任务
- 需要并行处理多个文件或模块
- 任务耗时长（后台运行）

❌ **不必使用**
- 简单的文件查询或代码搜索
- 一次性的小改动
- 需要立即反馈的交互式任务

### 并行化最佳实践
1. 使用 `/fleet` 模式处理可并行的任务
2. 将独立的研究或编码工作分配给不同的代理
3. 使用 `/tasks` 监控进度

### 任务设计原则
- 清晰定义任务边界
- 提供完整的上下文信息
- 设置明确的成功标准

## 📁 项目结构

```
copilot-agents-learning/
├── README.md                 # 本文件
├── docs/
│   ├── agent-basics.md      # Agent 基础知识
│   ├── subagents-guide.md   # 子代理使用指南
│   └── fleet-mode.md        # 并行执行模式
├── examples/
│   ├── simple-task/         # 简单任务示例
│   ├── parallel-task/       # 并行任务示例
│   └── complex-workflow/    # 复杂工作流示例
└── notes/
    └── learning-log.md      # 学习记录
```

## 🚀 快速开始

1. Clone 本仓库
2. 阅读 `docs/` 中的文档
3. 在 `examples/` 中尝试不同的 agent 工作流
4. 记录你的学习过程到 `notes/learning-log.md`

## 📖 官方资源

- [Copilot CLI 文档](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/use-copilot-cli)
- [Copilot 官方文档](https://docs.github.com/en/copilot)

## 📝 学习笔记

在这个项目中记录你的：
- 新发现和最佳实践
- 遇到的问题和解决方案
- 有趣的 agent 使用案例

---

**开始学习吧！** 🎯
