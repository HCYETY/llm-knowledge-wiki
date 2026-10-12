# 对话式 AI 助手 · 产品级开源前端选型指南

> 对标对象：Muse / ChatGPT / Codex 这一类**产品级对话式 AI 助手**（多轮会话、流式输出、工具调用、推理过程展示、多模型切换、会话管理）。
评选标准：**广受认可 + 流行度高 + 生产可用**。星标数据统一取自 GitHub API，采集时间 **2026-10-09**。

---

## 一、先分清四层，选型才不会错

大多数人选错，是因为把不同层级的东西放在一起比。这个领域其实分四层，**层级不同就不该直接对比**：

| 层级 | 作用 | 代表项目 |
| --- | --- | --- |
| **L0 基础 UI 层** | 原子组件（按钮、弹层、侧栏、滚动区）与设计系统底座 | `shadcn/ui`、Radix UI、Tailwind CSS |
| **L1 传输/运行时层（SDK）** | 流式协议、消息状态机、工具调用、多模型抽象 | **Vercel AI SDK**、TanStack AI |
| **L2 组件层（前端框架）** | 拿来即用的聊天 UI 组件 + 运行时绑定 | **assistant-ui**、**AI Elements**、**CopilotKit**、prompt-kit、Deep Chat |
| **L3 完整产品层** | 已经是一个可部署的完整助手产品（含后端、鉴权、多用户） | **Open WebUI**、**LobeHub**、**LibreChat**、**NextChat**、AnythingLLM、HuggingChat |

**一句话判断法：**

- 你要**做自己的产品**（自研后端、要品牌化 UI）→ 选 **L2 + L1**。

- 你要**立刻有一个能用的助手**（内部知识库、团队用）→ 选 **L3**。

- 你要**让 AI 操作你自己已有的应用界面**（Copilot 形态）→ 选 **CopilotKit**。

---

## 二、总览对比表（按层级）

### L2 组件层（做产品首选）

| 项目 | Stars | 技术栈 | 定位 | 授权 |
| --- | --- | --- | --- | --- |
| [CopilotKit](https://github.com/CopilotKit/CopilotKit) | **37.9k** | React / Angular / Mobile / Slack | 前端 Agent 栈 + 生成式 UI，AG-UI 协议发起方 | MIT |
| [assistant-ui](https://github.com/assistant-ui/assistant-ui) | **12.4k** | React（TypeScript） | Headless 聊天组件 + 运行时，AI SDK 深度集成 | MIT |
| [prompt-kit](https://github.com/ibelick/prompt-kit) | 3.1k | React / shadcn / Tailwind | 高质量 AI 界面积木（消息、Prompt Input、推理块） | MIT |
| [Deep Chat](https://github.com/OvidijusParsiunas/deep-chat) | 3.7k | Web Component，React/Vue/Svelte/Solid/原生通用 | 单体可定制聊天组件，跨框架 | MIT |
| [Vercel AI Elements](https://github.com/vercel/ai-elements) | 2.5k | React / shadcn/ui registry | AI SDK 官方组件库（含推理链、工具调用 UI） | 见仓库 |

### L1 SDK 层

| 项目 | Stars | 说明 |
| --- | --- | --- |
| [Vercel AI SDK](https://github.com/vercel/ai) | **27.2k** | 事实标准的 TypeScript AI 工具包，月下载量千万级；`useChat` 处理流式生命周期；支持 React/Vue/Svelte/Solid |
| [TanStack AI](https://github.com/TanStack/ai) | 3.2k | TanStack 出品，类型安全的 provider-agnostic SDK，React/Vue/Svelte/Solid 适配。**仍是新项目，处于早期** |

### L3 完整产品层（开箱即用）

| 项目 | Stars | 技术栈 | 一句话定位 |
| --- | --- | --- | --- |
| [](https://github.com/langgenius/dify) |
| [Open WebUI](https://github.com/open-webui/open-webui) | **154.1k** | SvelteKit + Python | 全球最流行自托管 AI 聊天界面，生态与插件最广 |
| [OpenHands](https://github.com/OpenHands/OpenHands) | **90.3k** | TypeScript + Python | **AI 软件开发助手（Codex 类比）**，含完整 Web 工作台 |
| [NextChat](https://github.com/ChatGPTNextWeb/NextChat) | **88.8k** | Next.js / React / Tauri | 零配置多端聊天助手（Web + 桌面 + 移动） |
| [LobeHub（原 Lobe Chat）](https://github.com/lobehub/lobehub) | **83.1k** | Next.js / React | 设计最精致的 Agent 平台，插件/技能生态强 |
| [AnythingLLM](https://github.com/Mintplex-Labs/anything-llm) | 66.8k | JS + React | 本地优先的全能 Agent / RAG 桌面+服务端 |
| [LibreChat](https://github.com/LibreChat-AI/LibreChat) | 45.4k | TypeScript（MERN 系） | 多模型聚合 + 企业级多用户鉴权，集成度最全 |
| [Chatbox](https://github.com/chatboxai/chatbox) | 42.0k | TypeScript | 跨平台桌面 AI 客户端 |
| [Vercel Chatbot](https://github.com/vercel/chatbot) | 21.0k | Next.js + shadcn/ui | 官方出品的**可魔改全栈模板**，`is_template` |
| [Chainlit](https://github.com/Chainlit/chainlit) | 12.5k | **Python** | Python 路线最快搭出对话式 AI 前端的框架 |
| [HuggingChat chat-ui](https://github.com/huggingface/chat-ui) | 11.0k | SvelteKit | 驱动 HuggingChat 的开源界面代码库 |
| [big-AGI](https://github.com/enricoros/big-AGI) | 7.1k | Next.js / React | 多模型并排对比（Beam）、开发者向 AI 工作台 |

### 另有：老牌但已过维护期

| 项目 | Stars | 状态 |
| --- | --- | --- |
| [chatbot-ui](https://github.com/mckaywrigley/chatbot-ui) | 33.3k | **最后提交 2024-08**，作者已转向其他项目。星标高但属历史遗产，**新项目不建议作为底座** |

---

## 三、组件层详解（做自己的产品就重点看这节）

### 1. assistant-ui —— React 栈的默认答案

- **定位**：Headless（无样式）聊天组件 + 运行时。类似 Radix 的思路：给你 `Thread` / `Composer` / `Message` 等原语，样式完全自己写。

- **为什么排第一**：流式生命周期、消息状态、滚动锚定、附件、工具调用这些"看不见的脏活"它都替你处理了，同时不限制你的设计。与 Vercel AI SDK 的集成是该生态里最紧密的。

- **适合**：React / Next.js 团队，要做**有自己品牌设计**的对话产品。业界评价把它视为当前 React 生态构建 AI 聊天界面的首选。

- **代价**：React-only；"Headless"意味着**出好看 UI 的设计工作量仍然要你自己出**，上手曲线真实存在。

### 2. Vercel AI SDK + AI Elements —— 生态引力最强

- **定位**：AI SDK 是传输/运行时层（`useChat` 等 hooks）；**AI Elements** 是 2025 年 8 月新推出的官方组件库，构建在 shadcn/ui 的 custom registry 上。

- **为什么值得用**：AI SDK 已是事实标准，几乎所有的组件库（assistant-ui、AI Elements、prompt-kit）都优先适配它。AI Elements 里有**推理链 / 思维过程 / 工具调用 / Agentic 会话**这些专门组件，恰好是 Codex 类产品最需要的那部分。

- **风险提示**：AI SDK → AI Elements → Next.js → Vercel 托管是一条"路径依赖"链。每一层都不强制，但阻力最小的路总是往深处走，**要注意生态锁定**。

### 3. CopilotKit —— 要做"能操作我自己应用"的助手才选它

- **定位**：不是 UI 库，而是**Agent 前端应用框架**。`<CopilotSidebar />` / `<CopilotPortal />` 一放就能出完整 copilot，Agent 可以读你的应用状态、调用你的函数、动态生成 UI。

- **分量**：37.9k stars，是 AG-UI 协议的发起方（与 Google A2UI 有联动），React / Angular / 移动端 / Slack 多端覆盖，MIT。

- **适合**：从零开始的 **Agent-Native 产品**（AI 能操作你应用的 UI 与数据）。

- **代价**：如果你只是要一个好看的聊天窗，用它属于"用喷火器点蜡烛"。它锁定的是**架构世界观**（状态同步模型、生成式 UI 抽象、Action 体系），不是几个组件——棕地项目接入可能意味着推翻已有 Agent 基建。

### 4. prompt-kit / Deep Chat / TanStack AI —— 补位选项

- **prompt-kit**（3.1k，MIT）：shadcn/Tailwind 体系下的 AI 界面积木，适合已经用 shadcn 的团队"缺哪块补哪块"，比 assistant-ui 更轻。

- **Deep Chat**（3.7k，MIT）：**唯一真正做到跨框架的成品组件**（Web Component，React/Vue/Svelte/Solid/原生都能用）。牺牲的是细粒度组合能力——它是单体组件，深度定制会被 props 卡住。

- **TanStack AI**（3.2k，MIT）：框架无关 + 类型安全，是"反锁定"路线里最有分量的新玩家。但**只有 hooks 没有组件，且项目仍处于早期**，现在上生产要自己承担风险，建议列入观察名单。

---

## 四、完整产品层详解（要"立刻能用"就看这节）

### Open WebUI（154.1k）—— 综合最强、社区最大

自托管 AI 聊天界面的绝对领导者（容器下载量数亿级）。支持 Ollama / OpenAI 兼容 API、RAG、MCP。**要一个内部可用的 AI 门户，它是默认答案。** 代价：它是个成品而不是框架，想做深度 UI 品牌化改造会很吃力。

### LobeHub / Lobe Chat（83.1k）—— 设计最精致、最像消费级产品

前端观感最接近 ChatGPT / Muse 这一档消费级产品的开源项目，Agent、技能、知识库、多模型生态完善。**要"看起来就是个正经产品"的参考实现，优先读它的代码。**

### LibreChat（45.4k）—— 多模型聚合 + 企业就绪

模型厂商覆盖最全（Anthropic / AWS / Azure / Gemini / Groq / OpenRouter / Vertex AI…），自带的**多用户鉴权、预设、会话搜索、Code Interpreter、Artifacts**是企业落地的实际需求。适合"公司内部要一个多模型统一入口"。

### NextChat（88.8k）—— 零配置、多端

Web + iOS + macOS + Android + Linux + Windows 全覆盖（Tauri 打包）。适合要一个**跨端客户端**而不是 Web 产品的场景。

### NextChat 之外的低成本内部方案

- **AnythingLLM**（66.8k）：本地优先，桌面端 + 服务端双形态，文档工作区（RAG）体验成熟。

- **Chatbox**（42.0k）：纯客户端，适合个人/小团队日常使用。

- **big-AGI**（7.1k）：多模型**并排对比**（Beam）是它独有的杀手锏，适合做模型评测/选型。

### Vercel Chatbot（21.0k）—— 唯一官方"全栈可魔改模板"

Next.js + shadcn/ui + AI SDK + Postgres/Redis，自带 Artifacts、多模型、鉴权、AI Gateway。**如果你既想要产品级完整度，又要自己掌控每一行代码，这是起点最优的选项**（注意它是 `is_template`，官方定位就是模板而非长久产品）。

### HuggingChat chat-ui（11.0k）—— SvelteKit 路线的唯一主流选择

如果你的团队是 Svelte，这是绕不开的参考实现。

### Chainlit（12.5k）—— Python 团队的捷径

如果后端是 Python（LangChain / LlamaIndex 生态），Chainlit 能在几分钟内把对话式 AI 跑出一个像样的前端。**代价是前端定制能力有限**，它是"快速验证与内部工具"，不是"产品级前端底座"。

### OpenHands（90.3k）—— Codex / 编码 Agent 的直接参照

这是与 Codex 最对位的一档：完整的 AI 软件开发 Agent（TypeScript 前端 + Python 后端），含任务工作台、终端、文件浏览、diff 预览、Agent 轨迹回放。**要做编码类 Agent 产品，它的前端信息架构值得逐屏拆解。**

### Dify（157.9k）—— 平台向，慎当"前端框架"

Dify 是 Agent 工作流 / RAG 编排平台，自带一个对话应用前端。它的价值在于**编排能力**而非前端工程质量。如果你的目标是"我要一个聊天前端"，用 Dify 是拿错了工具。

---

## 五、按场景直接给答案

| 你的情况 | 推荐组合 |
| --- | --- |
| React/Next.js，做**自研品牌化对话产品** | **assistant-ui + Vercel AI SDK**（或 AI Elements 补推理链/工具调用 UI） |
| 想最快拥有一个能跑的**完整助手** | **Open WebUI**（通用）／**LibreChat**（企业多用户） |
| 想做**消费级观感**的产品，需要参考实现 | 读 **LobeHub** 代码，UI 层用 assistant-ui 自建 |
| 要做 **Codex 式编码 / Agent 工作台** | 参照 **OpenHands** 的信息架构；组件层用 AI Elements 的工具调用与推理组件 |
| 要让 AI **操作自己已有应用的 UI**（Copilot 形态） | **CopilotKit** |
| 团队是 **Vue / Svelte / Angular** | **Deep Chat**（成品组件，唯一跨框架）；SDK 层选 TanStack AI（尚早期） |
| 后端是 **Python** | **Chainlit** 快速验证；产品化仍建议前端与后端解耦 |
| 要**全栈代码完全自己掌控**的起点 | **Vercel Chatbot** 模板 |
| 主要目的是**对比评测多个模型** | **big-AGI** |

---

## 六、避坑清单（比选型更重要的部分）

1. **别只看星标。** `chatbot-ui` 有 33.3k 星但已 2 年无提交；星标是历史资产，**维护活跃度看 ****`pushed_at`**。

1. **区分"成品"与"框架"。** Open WebUI / LobeHub / LibreChat 星标最高，但它们是**应用**，不是给你二次开发的框架。想改 UI 到品牌级，成本远高于用 assistant-ui 自建。

1. **授权要单独核对。** MIT 的有：assistant-ui、CopilotKit、LibreChat、NextChat、Deep Chat、Chainlit（Apache-2.0）、chat-ui（Apache-2.0）等。但 Open WebUI、LobeHub、Dify、Vercel AI SDK/Chatbot 的 license 字段为 `NOASSERTION`（自定义条款），**商用前必须逐个读 LICENSE 全文**（例如常见做法是带品牌/多租户限制的类开源协议）。

1. **警惕双层锁定。** CopilotKit 锁定架构，Vercel 生态锁定路径。选之前想清楚三年后换栈的代价。

1. **"看不见的部分"才是工作量所在。** 欢迎页、建议提示、引用与来源、推理轨迹、工具执行日志、成本估算、无障碍、主题化、国际化——这些从零做要 2~4 周，也是组件库真正值钱的地方。

1. **协议层正在出现。** AG-UI（CopilotKit）与 Google A2UI 在做"Agent ↔ 前端"的标准通信格式，A2UI 已带 Lit/Angular/Flutter 渲染器但仍早期。**现在不要押注，但要跟踪**——它可能改变明年的选型逻辑。

---

## 七、Codex 自身的开源代码库（补充）

### 1. 结论：开源了，但开源的**不是前端**

| 仓库 | Stars | 语言 | 授权 | 说明 |
| --- | --- | --- | --- | --- |
| [openai/codex](https://github.com/openai/codex) | **128.3k** | **Rust** | Apache-2.0 | 只开源 **CLI**（终端 TUI）这一个形态 |
| [openai/codex-universal](https://github.com/openai/codex-universal) | 1.1k | Dockerfile | 无 | Codex 云端环境的基础镜像 |

**没有开源的部分**（全部闭源）：

- **Codex IDE 扩展**（VS Code / Cursor / Windsurf）

- **Codex Web**（chatgpt.com/codex）

- **Codex 桌面 App**（`codex app`）

另外注意一个容易被忽略的信号：`openai/codex` 的 `pull_request_creation_policy` 是 **`collaborators_only`**，且累计 **21,609 个 open issue**。它是"源码公开、可自建、可研读"，**不是**"社区共治、欢迎你提 PR"的项目——这一点和 OpenHands（90.3k，全开放 PR）、opencode（212.2k，MIT）性质完全不同。

### 2. 最有价值的部分：它的前端是可以整个换掉的

这是本节真正值得你带走的东西。Codex 把 **agent 运行时**和 **UI** 彻底解耦了：

- 内核是 Rust 写的 **core 会话循环 + 协议层**；

- 对外暴露 **Codex App Server**——一套**双向 JSON-RPC API**，覆盖流式进度、工具调用、**审批（approvals）**、diff 等全部交互；

- 官方明确推荐：**要自建自己的 UI，就对 App Server 开发，而不是去改官方 TUI**；

- 生态已有带类型的 Rust 绑定（如 `codex_codes` crate），第三方自定义控制台/远程 UI 已经在用它。

> 翻译成产品语言：**你可以用 assistant-ui / AI Elements 自己写 Web 前端，后端直接挂 Codex App Server。** 这就是"Codex 级 agent 体验 + 自研品牌前端"的正解，也是前面那份选型指南里"组件层自研"路线的一个高价值落点。

### 3. 实现语言：从 React 换成了 Rust

- Codex CLI 最初的栈是 **React + TypeScript + Node**，终端 UI 用 **Ink**（"React for CLI"）；

- **2025 年 6 月** OpenAI 宣布**用 Rust 重写**，现仓库主语言为 Rust。

所以：**想直接抄它的前端代码，现在没得抄**（是 Rust TUI，不是 Web 前端）。但如果你要自己写终端形态的 UI，Ink 及其同期设计仍然是最近的参照对象。

### 4. 想要"Codex 那样的开源前端"，真正该看这几个仓库

| 项目 | Stars | 语言 | 授权 | 为什么值得看 |
| --- | --- | --- | --- | --- |
| [opencode](https://github.com/anomalyco/opencode) | **212.2k** | TypeScript | **MIT** | 星标最高的开源编码 Agent。TUI + server + 分享页结构，**架构最值得学，授权最宽松** |
| [OpenHands](https://github.com/OpenHands/OpenHands) | 90.3k | TypeScript + Python | MIT | **唯一完整的开源 Web 工作台**（任务、终端、文件树、diff、轨迹回放） |
| [Cline](https://github.com/cline/cline) | 70.0k | TypeScript | Apache-2.0 | 以 SDK / IDE 扩展 / CLI 三形态分发，交互设计的标杆 |
| [Gemini CLI](https://github.com/google-gemini/gemini-cli) | 107.2k | TypeScript | Apache-2.0 | 官方对标 Codex CLI，**TypeScript 实现**，可直接读源码 |
| [Qwen Code](https://github.com/QwenLM/qwen-code) | 28.3k | TypeScript | Apache-2.0 | Gemini CLI 的技术栈分叉，中文团队维护 |
| [Continue](https://github.com/continuedev/continue) | 36.2k | TypeScript | Apache-2.0 | IDE 内 agent 的成熟实现 |
| [Crush](https://github.com/charmbracelet/crush) | 28.6k | Go | **NOASSERTION** | 终端 agent 的 UI 审美天花板，但授权需单独确认 |
| [Aider](https://github.com/Aider-AI/aider) | 49.4k | Python | Apache-2.0 | 经典终端结对编程，**2026-05 后基本停更**，只作参考 |

### 5. 三条路径，按你的目标选一条

| 你的目标 | 建议做法 |
| --- | --- |
| 做**自己的 Codex 式产品**，前端要自研品牌 | 前端 **assistant-ui / AI Elements** + 后端对接 **Codex App Server**（JSON-RPC） |
| 要**能直接改造**的全开源同类产品 | fork **opencode**（MIT + TypeScript + 星标最高）或 **OpenHands**（MIT + 完整 Web 工作台） |
| 只想**研究 agent 运行时怎么设计** | 读 **openai/codex** 源码——Rust 实现、分层最干净，是学习样本而非开发底座 |

**两个必须注意的坑：**

1. **Apache-2.0 只覆盖那一个 CLI 仓库。** 你可以商用、可以自建、可以改，但**"Codex" 商标与品牌不在授权范围内**——给你的产品起名带 Codex 是侵权风险。

1. **不要把 CLI 当框架用。** 想在它的基础上做产品，正确入口是 **App Server 协议**，不是 fork TUI 再往上堆功能。

## 八、数据说明

- 所有星标数、`pushed_at`、license 均通过 GitHub REST API 于 **2026-10-09** 采集，非二手报道。

- 部分项目仓库已更名，本指南使用**新路径**：`lobehub/lobe-chat` → `lobehub/lobehub`；`vercel/ai-chatbot` → `vercel/chatbot`；`danny-avila/LibreChat` → `LibreChat-AI/LibreChat`；`Bin-Huang/chatbox` → `chatboxai/chatbox`。

- NLUX 等其他曾出现在对比文章中的项目，因仓库当前无有效响应未纳入推荐。

- 星标为动态指标，建议在正式立项时复核一次。

## 待完善

- 总览表的 Dify 行缺少显示文字和其余列，需要补齐。
- 授权说明中“MIT 的有”混列了文内标为 Apache-2.0 的项目，需要核对原始来源并统一表述。

## 相关页面

- [[Wiki/Wiki 索引|返回知识索引]]
- [[微信小程序 ai coding]]：另一种应用形态下的原型、UI 与开发实践。
- [[Databricks Genie Ontology：从企业场景到解决方案与底层技术的自上而下分析]]：企业 AI 应用的数据语义与治理背景。
