# Architect Builder

[English](README.md) | **简体中文**

Architect Builder 是一套轻量级 Codex skill 工作流，用来把 AI 辅助软件开发拆成三个明确阶段：

1. 使用 `plantask` 做规划
2. 使用 `executetask` 做执行
3. 使用 `reviewtask` 做审查

它的目标是让 AI 编码过程更可控、可审计、可复现。不要让同一个模型自由地规划、实现、再审查自己，而是通过 `.ai/` 目录下的交接文件，把规划、执行、验收标准和审查结论固化下来。

## Skills

| Skill | 用途 | 产出 |
|---|---|---|
| `plantask` | 生成技术方案和任务交接文件 | `AGENTS.md`、`.ai/PLAN.md`、`.ai/TASK.md`、`.ai/ACCEPTANCE.md` |
| `executetask` | 按任务文件执行开发并记录实现过程 | `.ai/IMPLEMENTATION.md`、`.ai/IMPLEMENTATION.diff`、git commit |
| `reviewtask` | 对照计划、任务和验收标准审查实现结果 | `.ai/REVIEW.md` |

## 推荐工作流

```text
Codex + plantask
  -> 生成技术方案、任务列表和验收标准

执行模型 + executetask
  -> 只按照任务文件实现，不重新规划，不扩大范围

Codex + reviewtask
  -> 审查 diff、范围、测试、风险和是否可以合并
```

这个流程特别适合“一个模型做架构师和审查员，另一个模型做执行工程师”的协作方式。

## 仓库结构

```text
Architect-Builder/
  README.md
  README.zh-CN.md
  skills/
    plantask/
      SKILL.md
    executetask/
      SKILL.md
    reviewtask/
      SKILL.md
```

## 安装

先克隆这个仓库：

```powershell
cd D:\Users\bubble\Desktop
git clone https://github.com/bubble0462/Architect-Builder.git
cd Architect-Builder
```

然后把 skill 文件夹复制到你的本地 Codex skills 目录：

```powershell
Copy-Item -Recurse .\skills\plantask "$env:USERPROFILE\.codex\skills\plantask"
Copy-Item -Recurse .\skills\executetask "$env:USERPROFILE\.codex\skills\executetask"
Copy-Item -Recurse .\skills\reviewtask "$env:USERPROFILE\.codex\skills\reviewtask"
```

如果本地已经有同名 skill，建议先备份。只有在你明确想覆盖时才加 `-Force`。

为什么是复制，而不是直接从 GitHub 安装？因为 Codex 会从本机 `.codex/skills` 目录加载本地 skills。这个仓库只是源码包；复制这一步是把 skill 放到 Codex 能发现的位置，同时让 Git 仓库和你正在使用的本地 skill 目录保持隔离。

## 使用方法

### 1. 规划

当你希望 Codex 读取项目并生成可执行计划时使用：

```text
plantask
```

预期产出：

```text
AGENTS.md
.ai/PLAN.md
.ai/TASK.md
.ai/ACCEPTANCE.md
```

### 2. 执行

在同一个项目中，规划完成后使用：

```text
executetask
```

执行者必须读取 `.ai/PLAN.md`、`.ai/TASK.md`、`.ai/ACCEPTANCE.md`，并且只实现任务文件中定义的内容。

预期产出：

```text
.ai/IMPLEMENTATION.md
.ai/IMPLEMENTATION.diff
```

### 3. 审查

实现完成后使用：

```text
reviewtask
```

审查者会对照原始方案、任务列表、验收标准、实现记录和 git diff 检查结果。

预期产出：

```text
.ai/REVIEW.md
```

## 为什么需要它

AI 编码很容易在长对话中偏离范围。Architect Builder 通过把每个阶段显式化来降低漂移：

- 方案文件定义范围。
- 任务文件定义执行边界。
- 验收文件定义成功标准。
- 实现文件记录实际改动。
- 审查文件判断是否通过。

## 说明

- 这些 skill 是普通的本地 Codex skills。
- 不需要额外运行时依赖。
- 设计用途是复制到本机 `.codex/skills` 目录中使用。
- 每个项目产生的 `.ai/` 交接文件应该留在对应项目里，不要放进这个仓库。

