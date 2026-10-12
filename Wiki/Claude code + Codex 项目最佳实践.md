
# **一、Claude Code \+ Codex = 完美互补**

| **Claude Code 擅长** | **Codex 擅长** |
| ------------------ | ------------ |
| 理解复杂需求             | 极致的代码细节      |
| 架构设计和规划            | 安全漏洞发现       |
| 多步骤复杂任务            | 对抗性思维        |
| 自然语言交互             | 测试用例生成       |

**形成一套工作流**：claude code 理解需求，规划好步骤并输出技术文档，由 codex 编码

**提问 case**：



# 项目目录结构全景

一个完整的 Claude Code 项目配置结构如下：

```Plain Text
your-project/
├── CLAUDE.md                    # 📋 项目级指令（团队共享，提交到 Git）
├── CLAUDE.local.md              # 👤 个人项目偏好（自动 gitignore）
├── .claude/
│   ├── settings.json            # ⚙️ 项目设置（团队共享）
│   ├── settings.local.json      # 👤 个人项目设置（gitignore）
│   ├── CLAUDE.md                # 📋 等效于根目录 CLAUDE.md
│   ├── rules/                   # 📏 模块化规则文件
│   │   ├── code-style.md        #    代码风格
│   │   ├── testing.md           #    测试规范
│   │   └── security.md          #    安全要求
│   ├── agents/                  # 🤖 自定义子代理
│   │   ├── code-reviewer.md
│   │   └── debugger.md
│   ├── skills/                  # ⚡ 自定义技能
│   │   └── fix-issue/
│   │       └── SKILL.md
│   └── worktrees/               # 🌳 Git Worktree 目录（加入 .gitignore）
├── .mcp.json                    # 🔌 项目级 MCP 服务器配置
└── .github/
    └── workflows/
        └── claude.yml           # 🔄 Claude Code GitHub Actions
```

> 新手提示：刚开始只需要 `CLAUDE.md` 和 `.claude/settings.json`。其他配置随着需求逐步添加。

## settings\.json 分层配置

Claude Code 使用分层配置系统，优先级从高到低：

| **优先级** | **层级** | **位置**                         | **作用范围** | **是否共享**  |
| ------- | ------ | ------------------------------ | -------- | --------- |
| 最高 🔴   | 托管策略   | 系统级路径（IT 部署）                   | 组织内所有用户  | 是         |
| ↓       | 命令行参数  | CLI flags                      | 当前会话     | 否         |
| ↓       | 本地项目设置 | \.claude/settings\.local\.json | 你在此项目    | 否         |
| ↓       | 共享项目设置 | \.claude/settings\.json        | 所有协作者    | 是（提交 Git） |
| 最低 🟢   | 用户设置   | \~/\.claude/settings\.json     | 你的所有项目   | 否         |

> 规则：高优先级覆盖低优先级。托管策略不可覆盖。

### 基础配置示例

```Plain Text
{
  "$schema": "https://json.schemastore.org/claude-code-settings.json",
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test *)",
      "Bash(git commit *)"
    ],
    "deny": [
      "Read(./.env)",
      "Read(./.env.*)",
      "Bash(curl *)"
    ]
  },
  "enabledPlugins": {
    "example-skills@anthropic-agent-skills": true,
    "context7@claude-plugins-official": true
  },
  "alwaysThinkingEnabled": true,
  "env": {
    "CLAUDE_CODE_EFFORT_LEVEL": "high"
  }
}
```

> 提示：添加 `$schema` 字段可以在 VS Code / Cursor 中获得自动补全。
> 
> 

## **写好CLAUDE\.md**

CLAUDE\.md 是一个特殊文件，Claude 在每次会话开始时读取它。写入 Bash 命令、代码风格、工作流规则等Claude 无法从代码中推断的信息。

### **什么该写，什么不该写**

|**该写 ✅**|**不该写 ❌**|
|---|---|
|Claude 猜不到的 Bash 命令|Claude 读代码就能知道的信息|
|与默认不同的代码风格规则|标准语言规范（Claude 已知）|
|测试指令和首选测试框架|详细的 API 文档（链接即可）|
|仓库约定（代码风格、规范、分支命名、PR 格式）|频繁变化的信息|
|项目特有的架构决策和注意事项|长篇教程或解释|
|开发环境怪癖（必需的环境变量）|逐文件的代码库描述|
|常见陷阱和非显而易见的行为|"写干净的代码"之类的废话|

### **避坑指南**

1. Prompt 过于模糊直接说“帮我优化这个模块”或“修复这个 Bug”，Claude 容易猜错意图。 

避坑：提供具体文件、场景、约束条件和验证标准。越具体越好。

2. 无限探索不设边界让 Claude “好好调研一下这个功能”，结果它读了几百个文件，上下文瞬间爆炸。

避坑：用 Plan Mode 限制范围，或直接使用 Subagents（子智能体）进行独立探索。

### CLAUDE\.md 通用项目模板

```Markdown
# 项目：[项目名]
[一句话描述]。技术栈：[列出关键技术]。

# 构建
- 安装：`pnpm install`
- 开发：`pnpm dev`
- 构建：`pnpm build`

# 测试
- 测试全部：`pnpm test`
- 测试单个：`pnpm test -- path/to/test`
- 类型检查：`pnpm typecheck`
- Lint：`pnpm lint`
- 优先运行单个测试文件
- UI 变更后截图对比验证

# 代码规范
- 使用 ES modules（import/export）
- 函数参数 >3 个时使用对象参数
- 错误处理使用 AppError 类（@src/lib/errors.ts）
- API 路径 kebab-case，JSON 属性 camelCase
- 函数式组件 + hooks
- Tailwind CSS（不用 CSS modules）
- 导入顺序：React → 第三方 → 本地 → 类型 → 样式

# 架构
- 状态管理：Zustand（不是 Redux）
- ORM：Drizzle（不是 Prisma）
- API：tRPC

# 工作流
- **IMPORTANT**: 修改代码后运行 `pnpm typecheck`
- **NEVER**: 不要修改 migrations/ 下的已有文件
- 提交遵循 Conventional Commits

# 压缩指令
When compacting, preserve:
- 修改过的文件完整列表
- 测试命令和结果
- 未完成的任务
```

# 常用的提问描述

> [188工时压到149！揭秘我们如何用Cursor让前端效率狂飙](https://zhuanlan.zhihu.com/p/1953480000633299331)
> 
> [7 个 Cursor AI 极限省钱大法，别花冤枉钱！](https://www.cnblogs.com/yupi/p/19252660)
> 
> 

## 根据原型图截图生成页面路由实践

上传原型图并描述：

> “根据所提供的原型图，生成符合页面层级的路由目录结构和页面组件文件。要求组件命名语义清晰、与原型图功能对应，并自动生成基础 React 代码。”
> 
> 

## AI 生成mock数据

## 一次性代码审查

> “审查一下刚才生成的首页组件 @src/app/page\.tsx，看看有没有 TypeScript 类型问题、性能隐患、或者不符合 react 最佳实践的地方”
> 
> 

# 验证工作能力

Claude 在能够验证自己工作的时候表现更好——可以运行测试、对比截图、校验输出。

没有明确的验证手段时，它可能产出看起来对但实际不工作的代码。你成了唯一的反馈回路，每个错误都需要你来发现。

## 验证策略

|**策略**|**模糊做法 ❌**|**高效做法 ✅**|
|---|---|---|
|提供测试用例|"实现一个验证邮箱的函数"|"写一个 validateEmail 函数。测试: user@example\.com → true, invalid → false, user@\.com → false。实现后运行测试。"|
|UI 变更截图对比|"让 dashboard 更好看"|"\[粘贴截图\] 按这个设计实现。完成后截图对比原图，列出差异并修复。"|
|根因修复|"构建失败了"|"构建失败，错误信息: \[粘贴\]。修复它并验证构建成功。解决根本原因，不要抑制错误。"|
|指定验证命令|"修改认证流程"|"修改认证流程后，运行 npm test \-\- \-\-testPathPattern auth 验证所有认证测试通过。"|

## 验证工具链 

|**类别**|**工具**|**适用场景**|
|---|---|---|
|📝 测试套件|单元测试、集成测试、E2E 测试|功能正确性验证|
|🔍 静态分析|ESLint、tsc \-\-noEmit、mypy|代码质量、类型安全|
|🚀 运行验证|npm run build、截图对比、Bash 脚本|构建通过、UI 正确、自定义检查|

## 测试驱动开发（TDD）

一个完整的验证驱动示例：

```Markdown
为 @src/utils/date.ts 添加 formatRelativeTime 函数。

要求：
- 输入 Date，输出 "刚刚"、"3 分钟前"、"2 小时前"、"昨天"、"3 天前" 等
- 超过 7 天返回 YYYY-MM-DD 格式

验证：
1. 先为以上每种情况编写测试用例
2. 实现函数
3. 运行 `npm test -- formatRelativeTime`
4. 运行 `npm run typecheck`
5. 所有通过后告诉我结果
```

## 优化验证方式

- 写好 CLAUDE\.md 中的测试命令，让 Claude 知道如何运行测试

- 用 Hooks 自动验证（详见第 6 节），例如 Stop Hook 自动运行测试套件

- 在 Skills 中嵌入验证步骤，确保工作流最后一步总是验证

# 规范驱动开发（SDD）

面对较大的业务需求（新功能、新模块），直接在 Claude 的对话中口述需求往往不够——需求会散落在多轮对话中，Claude 的上下文窗口也可能装不下。

规范驱动开发（Spec\-Driven Development, SDD） 提供了一种更结构化的做法：**先把需求写成 Markdown 规范文件**，再用 Claude Code 逐步实现。

## 核心理念

|**传统做法**|**SDD 做法**|
|---|---|
|口述需求 → Claude 直接写代码|先写 spec\.md → Claude 按规范编码|
|需求散落在聊天记录中|规范集中在文件中，可版本管理|
|发现问题时改代码|发现问题时先改规范，再重新生成|
|Claude 的理解随上下文衰减|每次通过 **@spec\.md** 注入完整需求|

### spec 结构

```Shell
specs/001-feature-name/
├── spec.md      ← WHAT：功能需求（用户故事、验收标准、边界情况）
├── plan.md      ← HOW：技术方案（架构设计、数据结构、接口定义）
└── tasks.md     ← DO：原子任务（逐步执行指令，每个任务改一个文件）
```

|**产物**|**定位**|**核心内容**|**谁来写**|
|---|---|---|---|
|spec\.md|需求规范（WHAT/WHY）|用户故事、验收标准、边界情况、MVP 范围|你主导，Claude 辅助提问|
|plan\.md|技术方案（HOW）|技术选型、目录结构、数据模型、接口设计、风险评估|Claude 主导，你审核决策|
|tasks\.md|执行计划（DO）|原子化任务列表、依赖关系、TDD 顺序|Claude 生成，你审核完整性|

### spec 编写

1. 协作编写 **spec\.md**

```Plain Text
我想构建 [功能描述]。用 **plan模式 **对我进行详细提问，
帮我生成一份 **spec.md**，包含：用户故事、验收标准、边界情况。
不要写任何技术实现细节。
```

2. 生成 **plan\.md**

```Plain Text
@specs/001-feature/spec.md
基于这份需求规范，生成技术方案 plan.md。
技术栈约束：[TypeScript / React / PostgreSQL]
包含：目录结构、核心数据模型、接口定义、实施阶段。
```

3. 分解 **tasks\.md**

```Plain Text
@specs/001-feature/spec.md @specs/001-feature/plan.md
将技术方案分解为原子任务列表 tasks.md。
要求：每个任务只改一个文件，测试先行（奇数任务写测试，偶数任务写实现）。
```

4. 逐步执行

```Plain Text
@specs/001-feature/tasks.md
执行任务 T001-T006。严格按 TDD 顺序：先写测试（必须失败），再写实现（使测试通过）。
```

## TDD 防幻觉原则

不要让 Claude 同时写代码和测试。如果它先写了有 Bug 的代码，再写测试，它会写出"验证错误逻辑"的测试，即一切绿色但全是错的。

**正确做法：分布进行**

1. 编写测试用例，覆盖正常流程和边界情况。

2. 实现代码，使所有测试通过。【不要修改测试】

**禁止修改测试**

不要为了让实现的代码通过测试用例而修改测试。

**防止 AI 修改测试：**

1. 提示词约束：在 `CLAUDE.md` 中写明` 严禁修改*_test.go / *.test.ts 文件`

2. **权限控制**：在 `.claude/settings.json` 中配置 `"deny": ["Write(*_test.go)", "Write(*.test.ts)"]`

3. Git 兜底：先提交测试代码，实现完成后用 `git diff` 检查测试文件是否被意外修改

> **权限控制**是关键——即使 AI 忽略提示词，权限系统也会硬性阻止。
> 
> 

## 从规范文件扩展到项目级闭环

`AGENTS.md`、`CLAUDE.md` 和 rules 适合保存长期、静态的项目约束，但它们通常不负责回答：当前任务做到哪一步、这次变更依据哪份设计、上个会话做过哪些调查、哪些新经验应该回流到长期规范。

对于跨会话、多人协作或长期维护的项目，可以在现有 SDD 流程上补齐四类项目资产：

1. 当前任务的需求、设计、实施计划与验收条件；
2. 可恢复的工作流状态；
3. 对照任务和规范执行的检查结果；
4. 收尾后的 journal，以及经过筛选后写回规范的稳定经验。

这形成了比“先写 spec 再编码”更完整的闭环：

```text
规范 → 任务 → 实现 → 对照规范检查 → 经验回流 → 归档
```

[[Trellis：项目级 Spec Coding 闭环]] 是这种设计的一种实现。是否采用完整 Harness，应依据任务风险、维护周期和协作复杂度判断；短任务仍可使用更轻的 [[Vibe Coding 与 Spec Coding|Vibe Coding]] 流程。

## 待完善

“提问 case”和“AI 生成 mock 数据”目前只有标题，后续补充实际案例。

## 相关页面

- [[Wiki/Wiki 索引|返回知识索引]]
- [[代码分支管理规范]]：工程协作与发布流程参考。
- [[Vibe Coding 与 Spec Coding]]
- [[Trellis：项目级 Spec Coding 闭环]]
- [[来源总结 - 从 vibe coding 到 spec coding：我用 Trellis 的实践总结]]
