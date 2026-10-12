---
title: "Trellis：项目级 Spec Coding 闭环"
created: 2026-08-11
tags:
  - ai-coding
  - harness
  - spec-coding
  - trellis
---

# Trellis：项目级 Spec Coding 闭环

## 定位

Trellis 是面向 AI 编程的项目级 Harness。它的重点不是替代模型，也不只是提供一组命令，而是让规范、任务、运行状态、检查结果和工作记忆形成可持续演进的事实源。

可以用下面的结构理解：

```text
Agent = Model + Harness

Model：理解、推理、生成代码
Harness：注入上下文、管理状态、调用工具、约束流程、检查和收尾
```

## 核心资产

| 资产 | 职责 | 生命周期 |
| --- | --- | --- |
| Spec | 保存跨任务复用的规范与边界 | 长期 |
| Task artifact | 保存当前需求、设计、计划与调研 | 单个任务，可归档 |
| Workflow state | 标记当前所处阶段和下一步动作 | 会话/任务运行期 |
| Check 配置 | 指定复核时需要读取的规范与材料 | 随任务 |
| Journal | 记录做了什么、提交了什么、踩过什么坑、下一步是什么 | 长期工作记忆 |

这些资产共同解决三个问题：新会话如何恢复、实现如何对照规范、任务经验如何进入后续工作。

## 工作闭环

```text
恢复上下文
  ↓
判断是否需要 Task
  ↓
Planning：形成需求/设计/计划/调研材料
  ↓
Execute：结合 Task + Spec 实现
  ↓
Check：结合 Diff + Task + Spec 复核
  ↓
Update Spec：筛选稳定、可复用经验
  ↓
提交业务代码
  ↓
Finish Work：归档任务并追加 Journal
```

这里最关键的设计是“回流”：任务结束后，不把所有过程细节塞进长期规范，只把未来任务仍然适用的规则写回 spec。

适合写入 spec 的内容包括：

- 某类接口必须统一使用的错误格式；
- 模块之间不能跨越的依赖边界；
- 数据库迁移必须满足的回滚约定；
- 已反复出现、值得永久规避的工程陷阱。

仅属于本次功能的路径、参数或实现细节，更适合留在 task 或 journal 中。

## 版本控制建议

根据来源文章的实践建议：

| 路径                           | 建议                    | 原因                     |
| ---------------------------- | --------------------- | ---------------------- |
| `.trellis/spec/`             | 提交 Git，并对重要变更做 review | 属于团队规范                 |
| `.trellis/tasks/`            | 提交 Git                | 承载需求、设计、计划和调研，是项目资产    |
| `.trellis/workspace/{name}/` | 提交 Git                | 保存各开发者 journal，便于交接与追溯 |
| `.trellis/.developer`        | 忽略                    | 本机开发者身份                |
| `.trellis/.runtime/`         | 忽略                    | 会话级运行状态                |

团队协作时，应给任务明确 assignee，减少多人同时修改同一任务材料造成的冲突。

## 采用前的成本

- 需要理解 `.trellis/` 下各类资产的职责；
- 需要适应 plan / execute / check / finish 的节奏；
- Spec 需要持续更新和清理，过时规范会误导 AI；
- Task 太大会让 AI 迷失，太小又会制造流程负担；
- 不同 Agent 平台的 Hook、命令和 Subagent 能力不同，接入完整度会有差异。

因此，Trellis 更适合长期、复杂、多人或频繁切换 AI 工具的项目，不适合一次性脚本和没有维护价值的短任务。

## 原文中的常用操作

> [!warning]
> 以下命令和配置来自 2026-07-26 的实践文章，尚未在本知识库中独立核验。Trellis 和 Codex 的接口可能变化，使用前应查看官方文档。

```powershell
npm install -g @mindfoldhq/trellis@latest
cd your-project
trellis init -u your-name
```

原文列出的常用命令：

- `/trellis:start`：需要手动恢复或启动上下文时使用；
- `/trellis:continue`：确认当前阶段结果并推进下一阶段；
- `/trellis:finish-work`：业务代码提交后，归档任务并写 journal。

初始化后应先让 AI 从真实代码提取第一版 spec，避免长期规范目录只剩空模板。

## 选择原则

如果主要诉求是约束 Agent 的做事方式，Skills/Rules 更轻；如果主要诉求是规范先行，OpenSpec 一类方案更直接；如果需要角色分工，可考虑多 Agent 编排；如果核心痛点是跨会话恢复和项目资产闭环，Trellis 的设计更对口。

这些工具可以组合使用。Trellis 管项目状态与事实，TDD、代码审查和调试 Skill 管单个阶段的执行纪律。

## 相关页面

- [[Wiki/Wiki 索引|返回知识索引]]
- [[来源总结 - 从 vibe coding 到 spec coding：我用 Trellis 的实践总结]]
- [[Vibe Coding 与 Spec Coding]]
- [[Claude code + Codex 项目最佳实践]]
