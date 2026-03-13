# Context Relay

> 让 Agent 在断裂的执行单元之间保持记忆连续性。

## 问题

OpenClaw Agent 的记忆会在多个地方断裂：
- **Session 重启**：上下文窗口清空，之前聊了什么全忘了
- **Sub-agent 边界**：子 agent 是独立进程，不继承父 session 的记忆
- **Cron 任务隔离**：定时任务在 isolated session 里跑，不知道你白天跟用户聊了什么
- **Heartbeat 隔离**：同上
- **Context 压缩**：对话太长被压缩，细节丢失

没有 Context Relay，Agent 会反复问用户"之前说的是什么？"，或者 cron 任务因为缺少上下文做出错误决策。

## 核心原则

**文件是唯一的真相源。** 不依赖 session 记忆，不假设"我应该记得"。每个执行单元（session、cron、sub-agent、heartbeat）启动时，从文件读取 context。

## 安装

这个 skill 不是被调用的工具，而是一套工作框架。安装后它会融入你的核心 MD 和日常工作流。

### 步骤 1：创建 todos.json

在 workspace 根目录创建：

```json
{
  "todos": []
}
```

这是你的自我待办文件。对话中答应了但没做完的事写在这里，heartbeat 会捡取执行。

### 步骤 2：在你的 AGENTS.md（或等效核心工作方法文档）中加入以下内容

#### 2a. Context Relay 机制

```markdown
## Context Relay

### 为什么需要

你的记忆会在 session 重启、sub-agent 边界、cron 隔离时断裂。文件是唯一的真相源。

### Context 断开点与对策

| 断点 | 对策 |
|------|------|
| Session 重启 | 启动时读取项目文件恢复 context |
| Sub-agent 边界 | Task 参数传递文件路径，子 agent 显式读取 |
| Cron 任务隔离 | 在 cron message 中写明要读哪些文件 |
| Heartbeat 隔离 | todos.json 的 projectFiles 字段传递 context |
| Context 压缩前 | 抢救关键决策到日记或 decisions.md |
| 对话中承诺但未完成 | 写入 todos.json，heartbeat 接力执行 |
```

#### 2b. 自我待办（todos.json）

```markdown
## 自我待办（todos.json）

对话中如果产生了"现在不方便做、但之后要做"的事，记到 `workspace/todos.json`，heartbeat 会捡取执行。

**什么时候写 todo：**
- 当前 session 马上要结束，但还有一件事没做完
- 需要等某个外部条件（比如等某个 cron 跑完再检查结果）
- 用户说"你待会记得做xxx"

**什么时候不要写 todo（直接做）：**
- **能现在做的就现在做，不要拖到 todo** — heartbeat 不是即时的
- 任务只需要几秒/几分钟 → 直接做
- 用户在等你的结果 → 直接做

**什么时候用 cron 而不是 todo：**
- 有明确的执行时间 → `cron add --kind at --at "..."`
- 需要反复执行的 → `cron add --kind cron`

**格式（Context Relay 友好）：**

| 字段 | 必填 | 说明 |
|------|------|------|
| `task` | 是 | 具体要做什么 |
| `priority` | 是 | `urgent` > `normal` > `low` |
| `context` | 是 | 为什么要做、背景信息（人类可读） |
| `projectFiles` | 否 | 相关项目文件路径，heartbeat 执行前先读取代入 context |
| `createdAt` | 是 | ISO 时间戳 |

`projectFiles` 是 Context Relay 的关键 — heartbeat 是 isolated session，不知道你对话时的上下文。把相关的 state.json、PROJECT.md 路径写进去，heartbeat 才能带着完整 context 执行。
```

#### 2c. 项目管理

```markdown
## 项目管理

### 项目结构

每个项目是一个有明确边界的持续工作单元：

```
projects/{name}/
├── PROJECT.md              # 目标、成功标准、参与者
├── context/
│   ├── state.json          # 机器可读状态（version、updatedAt、关键指标）
│   └── decisions.md        # 决策日志（为什么做某个选择）
└── tasks/                  # 子任务配置（可选）
```

### 新建项目 Checklist

1. 创建目录结构（见上）
2. 登记到 MEMORY.md 项目档案（路径、状态、当前重点、相关 cron）
3. 写入 state.json 初始版本 + decisions.md 创建记录
4. 如需定时任务，cron message 中显式写明要读哪些项目文件

### 项目改动后必过 Checklist

每次对项目做改动后，在告诉用户"搞定了"之前：

| # | 检查项 | 问自己 |
|---|--------|--------|
| 1 | Cron payload | 有没有 cron 引用了被改动的内容？ |
| 2 | state.json | version + updatedAt 需要更新吗？ |
| 3 | decisions.md | 这次决策的原因记录了吗？ |
| 4 | MEMORY.md 项目档案 | 当前重点/状态列过时了吗？ |

### Cron 任务 Message 模板

```
【{Project} - {Task}】

## 读取 Context
1. {project}/PROJECT.md（目标）
2. {project}/context/state.json（当前状态）
3. {project}/context/decisions.md（历史决策）

## 执行任务
[具体步骤]

## 更新状态
- 更新 context/state.json（version + updatedAt）
- 追加 context/decisions.md（本次决策）
- 更新 MEMORY.md 项目档案状态/重点列
```

### Sub-agent Message 模板

```
任务：{具体目标}

## Context 文件（必须读取）
- {project}/context/state.json
- {project}/PROJECT.md

## 输出要求
- 结果保存到：tasks/results/{filename}
- 更新 {project}/context/state.json（如适用）
- 追加 {project}/context/decisions.md（关键决策）
```

子 agent 不继承父 session 的记忆，必须通过文件显式传递 context。
```

### 步骤 3：在你的 HEARTBEAT.md 中加入 todo 捡取

在 heartbeat 检查项中加入以下步骤：

```markdown
## 执行待办事项（todos.json）

### 步骤

1. 读取 `workspace/todos.json`，取 `todos` 数组
2. 如果为空 → 跳过
3. 按优先级排序：`urgent` > `normal` > `low`
4. 逐条执行：
   - 如果有 `projectFiles` → 先读取这些文件代入项目 context
   - 读 `task` 描述和 `context`
   - 执行任务
   - 完成后从数组中移除
5. 写回 todos.json（剩余未完成的）
6. 执行失败的 → 保留在列表中，向用户汇报

### 注意
- 每次 heartbeat 最多执行 5 条，防止超时
- 涉及对外发消息 → 先确认意图再执行
```

### 步骤 4：Workspace 文件体系表中登记

在你的文件体系表中加一行：

```
| todos.json | 自我待办，heartbeat 每小时捡取执行 | 协调 |
```

## 模板文件

安装时可从 `templates/` 目录复制：

| 文件 | 用途 |
|------|------|
| `templates/todos.json` | 空的待办文件 |
| `templates/project-scaffold/` | 新项目的目录模板 |

## 设计哲学

1. **文件 > 记忆**：不信任 session context，一切持久化到文件
2. **显式 > 隐式**：cron/sub-agent/heartbeat 读什么文件必须写明，不能假设"它应该知道"
3. **能做就做 > 待会做**：todo 是兜底机制，不是默认工作方式
4. **State + Decisions 分离**：state.json 给机器读（快速恢复），decisions.md 给人读（理解为什么）
