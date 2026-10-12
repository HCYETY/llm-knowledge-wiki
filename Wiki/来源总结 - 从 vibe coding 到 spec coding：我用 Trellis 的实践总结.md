---
title: "来源总结：从 vibe coding 到 spec coding——我用 Trellis 的实践总结"
source: "https://mp.weixin.qq.com/s/SPVVCoyWGp77h-DkPij3Uw"
author: "[[洛卡卡了]]"
created: 2026-08-11
source_published: 2026-07-26
tags:
  - source-summary
  - ai-coding
  - spec-coding
  - trellis
---

# 来源总结：从 vibe coding 到 spec coding——我用 Trellis 的实践总结

## 一句话总结

AI 编程的长期瓶颈不只是模型能否写出代码，而是项目能否保存规范、任务状态、历史决策和可复用经验；Trellis 试图把这些材料组织成一个可恢复、可检查、可沉淀的项目级闭环。

## 核心观点

1. **Vibe Coding 适合短、低风险任务**：一次性脚本、验证想法和无需长期维护的小项目，直接对话通常更快。
2. **长期项目不能只依赖临时会话**：AI 容易围绕当前报错或需求给出局部最优修改，却不了解模块边界、历史取舍和跨模块影响。
3. **Agent 不只是模型**：可将其理解为 `Agent = Model + Harness`。模型负责理解与生成，Harness 负责上下文注入、任务推进、规则读取、工具调用、检查和收尾。
4. **Prompt、Rules、Skill 各自只解决局部问题**：Prompt 处理临时意图，Rules 保存静态规范，Skill 固化操作流程；它们通常不能独立解决跨会话状态、工具切换和经验回流。
5. **Spec Coding 的重点是项目事实源**：将需求、设计、任务、约束和决策保存在仓库中，使 AI 和人都能读取、审查与追溯。
6. **Trellis 的价值在闭环，而非单个命令**：它把 spec、task、workflow state、check、journal、update-spec 和 finish-work 串成持续演进的工作系统。

## Trellis 的项目闭环

原文描述的完整链路可以整理为：

1. 新会话启动时恢复项目上下文。
2. 每轮交互注入当前 workflow state。
3. 判断工作是否复杂到值得创建 task。
4. Planning 阶段形成 PRD、设计、计划和调研等任务材料。
5. Execute 阶段按 task artifact 和相关 spec 实现。
6. Check 阶段结合 diff、任务目标和规范进行复核，而不只运行 lint/test。
7. Update-spec 阶段只筛选值得长期复用的稳定经验。
8. 先提交业务代码。
9. Finish-work 归档任务并写入个人 journal。

原文称其为“八步”，但在最终串联时把“业务代码先提交”单列出来，因此实际呈现为九个动作。这不影响其核心结构：**恢复 → 规划 → 执行 → 对照规范检查 → 筛选经验 → 归档**。

## 四类 Harness 的设计取向

| 类型          | 代表                       | 主要解决的问题              | 主要代价                  |
| ----------- | ------------------------ | -------------------- | --------------------- |
| 流程强化型       | Superpowers、Skills/Rules | Agent 应按什么工程纪律做事     | 小任务可能过重，流程结束后资产未必进入项目 |
| 规范管理型       | OpenSpec                 | 先对齐规范，再实施变更          | 仍需机制保证规范被正确读取与遵守      |
| 多 Agent 编排型 | oh-my-claudecode         | 规划、执行、评审、调试分工        | 编排和排查成本较高             |
| 项目闭环型       | Trellis                  | 规范、任务、状态、记忆和经验持续进入项目 | 需要学习结构并持续维护 spec      |

这些类型并非互斥。例如，项目可以用 Trellis 保存项目状态，同时用 TDD 或代码审查 Skill 约束单个任务的执行方式。

## 适用边界

适合考虑 Trellis 或类似项目闭环的信号：

- 项目需要长期维护；
- 业务规则、架构约束或测试约定较多；
- 多人协作，且成员使用不同 AI 编程工具；
- 任务经常跨会话、跨天或需要中途交接；
- 希望历史决策和踩坑经验进入 Git，而不是只留在聊天记录中。

不适合或收益较低的场景：

- 一次性脚本、短实验和低风险任务；
- 项目没有长期维护价值，速度远比可追溯性重要；
- 团队没有意愿维护和评审 spec；
- 流程产物不会被后续任务实际读取。

## 对个人与团队的价值

对个人开发者，Spec Coding 的主要价值是给未来的自己留下一条能重新接上的线，减少“几个月后回来只能重新读代码和聊天记录”的成本。

对团队，价值更集中在共享事实源：规范和任务可进入 Git 并接受 review，不同成员可以继续使用各自熟悉的 Agent 工具，journal 则保留实际演进轨迹，帮助交接和新人理解项目。

## 值得保留的实践原则

- **沉淀稳定规则，不沉淀所有细节**：跨任务复用的接口约定、依赖边界和迁移规则适合写入 spec；一次性实现细节更适合留在 task。
- **规范也要像代码一样维护**：过时 spec 会比没有 spec 更危险，重要变更应经过 review。
- **按任务风险选择流程重量**：Spec Coding 不是把每件事都流程化，而是为值得追溯的工作留下必要材料。
- **验证不只等于测试通过**：检查还要覆盖架构约束、权限、错误格式、目录组织和文档影响。

## 阅读时需要注意

- 文章是个人实践总结，缺少量化对照，关于效率与维护性的结论更适合作为经验假设，而非普遍定律。
- 文中的 Trellis 命令、目录与 Codex Hook 配置具有版本时效性，实际采用前应以官方文档为准。
- “Spec Coding”在文中指向的不只是先写需求文档，还包括任务状态、工作记忆、检查与经验回流，范围比狭义的 Spec-Driven Development 更大。

## 来源

- 原始资料：[[2026-08-11 - 从 vibe coding 到 spec coding：我用 Trellis 的实践总结]]
- 微信原文：[从 vibe coding 到 spec coding：我用 Trellis 的实践总结](https://mp.weixin.qq.com/s/SPVVCoyWGp77h-DkPij3Uw)

## 相关页面

- [[Wiki/Wiki 索引|返回知识索引]] · [[Raw/index|原始资料索引]]
- [[Vibe Coding 与 Spec Coding]]
- [[Trellis：项目级 Spec Coding 闭环]]
- [[Claude code + Codex 项目最佳实践]]
- [[微信小程序 ai coding]]
