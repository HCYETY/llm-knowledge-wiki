# Databricks Genie Ontology：从企业场景到解决方案与底层技术的自上而下分析

**作者：Manus AI**  
**版本日期：2026-08-24**

> **一句话结论：** Genie Ontology 不是一个单独的“知识图谱产品”，而是 Databricks Genie 的**统一企业上下文层**。它把企业显式治理的指标、概念和域结构，与从查询、仪表板、数据管道、Agent 及使用行为中推断出的知识片段合并起来，再通过权威排序、冲突消解、引用与权限过滤，为 Genie One、Genie Agents 和 Genie Code 提供“这个业务词是什么意思、应该到哪里找、什么来源更可信、用户有权看到什么”的共同地图。[1] [2] [3]

## 一、管理层摘要

传统 BI 解决的是“把已建好的报表给用户看”，传统语义层解决的是“让不同工具按同一口径计算指标”，而 Genie Ontology 试图再向前走一步：解决企业 Agent 的**上下文问题**。企业知识通常散落在表结构、SQL、Dashboard、Pipeline、Wiki、工单、文档和日常使用习惯中。若模型只看到 Schema 或文档向量，它可能知道“数据在哪里”，却不知道“公司实际上如何使用这份数据、哪个定义更权威、冲突时应该信谁”。Databricks 将 Ontology 定义为持续更新的企业上下文图，并使用类似 PageRank 的权威评分考虑来源、作者、使用频率、认证关联和新鲜度。[1] [2]

从企业应用角度看，Genie 的价值路径不是“问数聊天机器人”，而是一个七级阶梯：**看数 → 解释 → 产出 → 复用 → 主动监控 → 有限行动 → 规模化运营**。最成熟的首批场景通常是经营复盘、销售 Pipeline、财务差异分析、库存异常、客服主题分析和 IT/DataOps 故障调查；它们高频、答案可验证、价值可量化，也容易建立领域 Owner 与 Benchmark。[8] [9]

从技术角度看，Genie Ontology 的核心不应被理解为“更大的 Prompt”。其准确性来自复合系统：Unity Catalog Semantics 提供显式语义；Metric Views 把指标定义变成可执行、可授权、可复用的 SQL 对象；Genie Agents 提供领域数据、指令、示例 SQL、Trusted Assets 和 Benchmark；Unity Catalog 执行用户级权限、行过滤、列掩码、血缘与审计；Compound AI 系统完成上下文检索、Text-to-SQL、多步计划和结果解释。[3] [4] [5] [6]

截至 **2026-08-24**，Genie Ontology 及其 Ontology snippets 仍为 **Public Preview**。它已默认启用，且自 2026-08-13 起所有客户无需单独申请即可使用 snippets，但企业应继续关注接口、行为与定价变化，并保留试点隔离、版本回归和退出方案。[7]

## 二、先界定概念：Ontology、语义层与知识图谱是什么关系

### 2.1 Databricks 所说的 Genie Ontology

Genie Ontology 将两类上下文合并为一个统一层。第一类是企业主动定义和治理的 **Unity Catalog semantics**，包括 Metric Views、Domains、Pages，以及认证和弃用信号；第二类是 Genie 从现有资产与使用中自动抽取和维护的 **inferred context**，其来源包括 Metric Views、Dashboard、SQL 查询和 Genie Agents。推断片段可表达指标定义、权威来源和业务规则，并依据来源、使用和新鲜度获得权威评分。[2] [3]

| 概念 | 主要回答的问题 | 建设方式 | 在 Genie 中的角色 |
|---|---|---|---|
| 传统数据目录 | 有什么数据、谁拥有、从哪里来 | 扫描元数据与人工维护 | 提供发现、血缘、Owner、标签与治理信号 |
| 传统语义层 | 指标怎么计算、维度怎么切 | 领域专家显式建模 | 提供可执行、可复用的指标主干 |
| 传统知识图谱/本体 | 实体、关系、类别和规则如何组织 | 通常需要正式建模 | 提供概念关系和推理结构 |
| Genie Agent Context | 某个业务域如何问、如何算、哪些逻辑已验证 | 指令、示例 SQL、函数、Trusted Assets | 提高领域问题的可控性与准确率 |
| **Genie Ontology** | **业务词是什么意思、去哪里找、信谁、如何按权限回答** | **显式语义 + 自动推断 + 权威排序** | **成为 Genie 体验共享的企业上下文层** |

因此，Genie Ontology 更接近“**可治理语义主干之上的持续学习上下文图**”，而不是要求企业先完成一项覆盖全公司的传统本体工程。自动推断降低了冷启动后的长尾维护成本，但也意味着它可能学习到组织中的错误习惯或过时口径，必须用认证、弃用、Pages、Metric Views 和人工评审来纠偏。独立分析也提出了类似警示：推断上下文反映的是“观察到的行为”，不一定天然等同于正式认可的定义。[13]

### 2.2 与 Genie 产品家族的关系

Databricks 将 Genie 产品家族分为三类体验：**Genie One** 是业务用户的统一入口；**Genie Agents** 是数据团队和领域专家配置可信数据、指标与业务规则的专域环境；**Genie Code** 是面向开发者和技术人员的工作区 AI 助手。三者建立在同一个受治理数据底座上，而 Ontology 为这些体验提供共享上下文。[8]

| 产品 | 面向角色 | 主要职责 | 与 Ontology 的关系 |
|---|---|---|---|
| Genie One | 高管、经理、业务用户 | 自然语言问数、浏览 Dashboard、生成文档、运行技能与计划任务 | 消费统一上下文，进行 Agent 路由、资产检索与业务协作 |
| Genie Agents | 数据团队、领域专家构建，业务团队消费 | 为单一领域配置数据、规则、示例、Trusted Assets、Benchmark | 是权威领域知识与可验证分析逻辑的重要来源 |
| Genie Code | 数据工程师、分析师、开发者 | 编写/运行代码，构建 Pipeline、Dashboard 和 Agentic 任务 | 搜索同一 Ontology，并帮助把对话修正沉淀为上下文 |

## 三、自上而下框架：场景—解决方案—底层技术

![Genie Ontology 自上而下七层架构](https://files.manuscdn.com/user_upload_by_module/session_file/310519663225719844/FLdizQMWdwrzVEsg.png)

### 3.1 第一层：企业场景与用户体验

企业用户通常不会以“我要调用 Ontology”为起点，而是从一个业务任务开始，例如“为什么收入低于计划”“哪些订单本周可能延期”“客服满意度下降由什么驱动”“今天哪条数据管道影响最大”。Genie One 将自然语言问数、Dashboard、文档、技能、计划任务和连接工具放在统一体验中；也可通过移动端、Teams、Databricks Apps、Conversation API 或 MCP 类入口嵌入现有工作流。[2] [8] [11]

### 3.2 第二层：领域 Agent 与工作编排

Genie Agents 不是单一 LLM，而是复合 AI 系统。领域专家可配置 Knowledge Store、Instructions、示例 SQL、SQL Functions、Trusted Assets 和 Benchmarks。Agent mode 能把复杂问题拆成多个子任务，执行多次 SQL，并输出带发现、表格、引用和可视化的结构化报告。[5]

### 3.3 第三层：Ontology 上下文层

Ontology 在运行时要完成五件事：识别与问题相关的业务术语；找到可能的指标、表、Agent 和规则；判断来源的权威性与新鲜度；解决不同来源之间的冲突；按用户权限筛选并给出引用。其意义在于让模型不必每次对整个数据资产进行盲目搜索，也不必仅凭语言相似度猜测。[1] [2]

### 3.4 第四层：可治理、可执行的语义主干

Metric Views 是 Unity Catalog Semantics 的核心实现。它把度量定义与用于分组、筛选和聚合的维度分开，使企业可以“指标定义一次，运行时按不同维度查询”。它支持 SQL DDL 或 UI 创建、YAML 校验、星型/雪花关系、多级 Join、可组合和窗口指标、参数、物化与 Agent metadata；也可被 SQL、Notebook、Dashboard、Genie Agents 和外部 BI 工具复用。[4]

Domains 负责按业务目的组织资产，Pages 提供可治理的业务概念说明，Certification/Deprecation 表达“组织认可什么、什么已经过时”。这些显式语义组成 Ontology 的 human-modeled layer，决定了企业知识的主干和权威锚点。[3]

### 3.5 第五层：治理、安全与质量运营

Unity Catalog 在数据与 AI 交互下执行统一治理，包括可安全授权对象、细粒度权限、行过滤、列掩码、工作区绑定、血缘、审计、分类和数据质量监控。[10] Genie Agents 的计算访问与数据访问可以分离：SQL 使用 Agent 作者嵌入的计算凭据运行，但 Unity Catalog 仍按终端用户身份判断数据访问，并将查询归因到终端用户。[5]

质量方面，用户反馈不会自动改变 Agent 行为。编辑者需要审阅错误、修正 SQL，并把正确逻辑保存为示例或 Trusted Asset；Benchmarks 用于批量评估和回归。也就是说，Ontology 会自动演化，但企业知识仍需要明确的 Owner、评审和发布流程。[6]

### 3.6 第六层：查询与 AI 执行

一个典型执行链是：**意图理解 → 相关上下文筛选 → 计划或 SQL 生成 → SQL Warehouse 执行 → Unity Catalog 按用户授权 → 结果解释与可视化 → 引用、反馈与监控**。Databricks 平台底层通常可依托 Databricks SQL、SQL Warehouses、Photon 和 Spark 完成查询计算，但公开材料并未声明每次 Ontology 请求固定经过所有平台组件，因此这些应被视为“通常依托的技术栈”，而非未经公开证实的内部实现细节。[5] [12]

### 3.7 第七层：数据接入与存储

底层数据可能来自 ERP、CRM、WMS/TMS、工单、日志、文档和协作应用。Databricks 平台可通过 Lakeflow Connect、Auto Loader、Lakeflow Pipelines 与 Lakehouse Federation 进行汇聚或就地访问，并以 Delta Lake、对象存储及统一治理资产作为分析底座。[1] [12]

## 四、面向企业的常见应用方式

### 4.1 按应用形态分类

| 应用方式 | 典型用户与问题 | 主要产出 | 成功前提 |
|---|---|---|---|
| 自助问数与探索 | 高管、经理、分析师查询 KPI、趋势和明细 | 文本、表格、图表、生成 SQL | 认证指标、清晰描述、用户权限正确 |
| 领域专家 Agent | 财务、销售、供应链、HR、客服等高频专域问题 | 可追问的受治理答案 | 领域边界、Owner、示例 SQL、Trusted Assets、Benchmark |
| 复杂调查/研究报告 | 分析异常、跨维度验证多个假设 | 带引用、图表和支持表格的报告 | Agent mode、可用关系、多步 SQL、人工核验 |
| 周期化 AI Cowork | 周报月报、会议 Brief、会后行动项 | 固定模板报告、决策摘要、责任人/截止日期 | 模板稳定、时间口径清楚、连接器可用 |
| 主动监控与预警 | 收入、库存、工单、失败作业、SLA | 异常摘要、Top drivers、消息/邮件提醒 | 3–5 个关键指标、正常基线、告警责任人 |
| 嵌入式问数 | 在 CRM、门户、Chatbot、Teams 或 App 内提问 | 不切换工具的上下文分析 | OAuth、身份透传、API 交互与 Agent 质量 |
| Agent 工程化交付 | 数据平台或 AI CoE 跨环境发布 | 配置即代码、CI/CD、版本化 Agent | 环境隔离、管理 API、回归测试、变更审批 |
| 多 Agent 跨域路由 | 财务+销售+供应链等跨域问题 | Supervisor 统一入口与专域协同 | 小而专 Agent、路由、跨域权限、统一可观测性 |
| 洞察到有限行动 | 创建工单、更新 CRM、触发审批 | 写操作与业务流程推进 | 人工确认、幂等、审计、回滚和最小权限 |

Databricks 官方列出的高价值 Genie One 工作流包括自动化经营复盘与报告、会前准备和会后跟进、动态知识/文档生成，以及对收入、错误率、工单积压、失败作业和库存等关键信号进行主动监控。[9]

### 4.2 按企业职能分类

| 职能 | 典型故事 | 语义与数据重点 | 输出或行动 |
|---|---|---|---|
| 高管经营 | “收入为何低于计划，哪个区域/产品驱动？” | 收入、毛利、预算、组织层级、财务日历 | 经营摘要、贡献拆解、董事会叙事 |
| 销售 | “Pipeline 是否健康，哪些 Deal 风险最高？” | CRM、活动、报价、订单、赢单/承诺口径 | Forecast、风险清单、客户 Brief、跟进任务 |
| 市场 | “活动 ROI 和转化下降由什么驱动？” | Campaign、媒体、CRM、归因规则、受众 | 预算优化、分群、漏斗与活动复盘 |
| 财务/采购 | “预算差异、应收可收回性、现金流风险如何？” | ERP/EPM、GL/AP/AR、科目、期间与合并口径 | Variance narrative、Scenario、Board-ready report |
| 供应链 | “哪里会缺货，哪些订单有 OTIF 风险？” | ERP/WMS/TMS、库存、供应商、物流、地区/品牌权限 | 风险排序、根因、调拨/补货候选方案 |
| HR | “人员流动和学习成效有什么变化？” | HRIS、薪酬、学习、调查、组织结构、敏感字段 | Workforce insight、培训效果、主管自助 |
| 客服 | “CSAT/FCR 下降原因是什么？” | 会话转写、工单、客户主数据、服务指标 | 主题/根因、QA、坐席提示、改进清单 |
| IT/DataOps | “本次 Incident 的根因和影响范围是什么？” | 日志、工单、Pipeline、血缘、SLA、运行历史 | 故障分类、影响分析、处置步骤、MTTR 改善 |

合作伙伴方案已将 Genie 用于销售 Pipeline、营销 ROI、HR/L&D、财务 What-if、供应链风险、呼叫中心分析和 IT Incident 等场景。不过这些案例中包含第三方加速器、预测模型和多 Agent 编排，不能全部等同于 Databricks 原生开箱即用能力。[14]

### 4.3 企业采用成熟度

| 成熟度 | 用户体验 | 平台能力 | 组织能力 |
|---|---|---|---|
| L1 看数 | 查询已认证 KPI | Text-to-SQL、Metric Views | 指标 Owner 与基础权限 |
| L2 解释 | 追问驱动与明细 | 多维分析、SQL/引用 | 领域专家能审查答案 |
| L3 产出 | 生成图表、报告和文档 | Agent mode、模板 | 输出评审与业务验收 |
| L4 复用 | 把正确流程保存为 Agent/Skill | Trusted Assets、Memory、Benchmarks | 版本和变更管理 |
| L5 主动 | 定期运行、异常预警 | Schedules、Monitoring | 告警责任人和处置闭环 |
| L6 行动 | 在审批后更新外部系统 | MCP/连接器/Apps/API | 写操作控制、审计与回滚 |
| L7 规模化 | 多域统一入口 | Supervisor、CI/CD、成本/质量监控 | CoE + 业务域双层治理 |

## 五、框架亮点

| 亮点 | 关键机制 | 为什么重要 |
|---|---|---|
| 从语义层升级为上下文层 | 显式语义 + 自动推断知识 + 来源关系 | Agent 不仅知道怎么算，还知道去哪找、信谁、规则如何关联 |
| 降低纯手工本体建设成本 | 从资产和使用中持续抽取片段 | 更快覆盖长尾知识，并随组织实际使用演化 |
| 权威排序与冲突消解 | 类 PageRank 权威评分 | 当多个 Dashboard、SQL 或 Agent 给出不同定义时，不只按语言相似度选答案 |
| 语义可执行 | Metric Views、SQL Functions、Trusted Assets | 业务口径既是文档，也是可重复执行的计算对象 |
| 治理内生 | Unity Catalog 用户权限、行列安全、血缘、审计 | 不需要为 Agent 再造独立安全模型 |
| 复合 AI 而非单一 LLM | 检索、规划、Text-to-SQL、查询、解释与可视化协同 | 提高复杂任务的可控性、可观测性和扩展性 |
| 从回答到协作/行动 | Skills、Schedules、API、MCP、Apps | 缩短“发现—解释—沟通—执行”的链路 |
| Agent 质量可运营 | Benchmarks、反馈、人工修正、CI/CD | 将 Agent 当成持续演进的数据产品，而不是一次性 Demo |

Databricks 在内部企业数据任务基准中宣称，Genie 首次回答正确率为 84.5%，强于其对比的通用编码 Agent，并且延迟更低；但这是厂商内部测试，而非独立第三方评测，企业仍应使用自己的真实问题集进行验证。[1]

## 六、边界、风险与治理重点

| 风险 | 形成原因 | 典型后果 | 建议控制 |
|---|---|---|---|
| 错误习惯被放大 | 热门资产或查询未必正确 | 错误口径得到更高权威 | 认证/弃用、Metric Views、Owner 审核 |
| 冷启动信号稀疏 | 新业务域缺少元数据和使用历史 | 推断片段少、问答不稳定 | 先建核心指标、关系、示例 SQL 和 Benchmark |
| 部门口径冲突 | 同一术语在财务、销售、运营含义不同 | 同一句问题跨域得到不同答案 | 域隔离、Pages、命名规范、权威 Agent 路由 |
| LLM 非确定性 | 同一问题可能生成不同 SQL 或解释 | 难复现、难审计 | Trusted Assets、固定 Metric Views、回归测试 |
| 权限正确但仍有语义敏感性 | 汇总或解释可能泄露业务意图 | 最小知情与合规风险 | 敏感域隔离、输出红队、聚合控制与人工评审 |
| 多 Agent 复杂度 | 跨域路由、权限、延迟和责任边界增加 | 路由失败、答案拼接矛盾 | 小而专 Agent、Supervisor、超时降级、统一监控 |
| 写操作风险 | 外部工具可执行更新或触发流程 | 越权、重复执行、误操作 | 人工确认、最小权限、幂等、审计、回滚 |
| 上下文岛 | 大型企业存在多数仓、多 BI、多 Agent 平台 | 重复语义与上下文不一致 | 明确语义主权、跨平台交换和系统边界 |
| 产品成熟度 | Ontology 仍为 Public Preview | 行为、接口或定价变化 | 试点隔离、版本锁定、Release Notes、退出方案 |

Databricks Solutions 的 Genie Ontology Readiness 应用将准备度归纳为七个支柱：**Unity Catalog、Metadata、Relationships、Metrics/Metric Views、Genie Agents、Domains 和 Adoption**。这一划分说明，上线准备不只是技术接通，还包括治理覆盖、语义建模和真实采用。[15]

## 七、费曼学习法：面向用户全景地图的典型故事线

费曼学习法强调：先用初学者能听懂的语言解释；通过“为什么”暴露理解缺口；回到材料补齐；再简化并重复。[16] [17] 将其用于 Genie，可以同时成为**用户解释框架**和**Agent 产品运营框架**。

![费曼学习闭环与用户故事](https://files.manuscdn.com/user_upload_by_module/session_file/310519663225719844/EBIFDvyFQbxGMGhp.png)

### 7.1 用户全景地图

| 用户角色 | 他们提出的问题 | Genie/Ontology 隐藏的复杂性 | 他们仍需负责的部分 |
|---|---|---|---|
| 高管/业务负责人 | 发生了什么、为什么、下一步做什么 | 跨资产查找、口径匹配、多步分析与摘要 | 定义阈值、做业务判断、承担行动责任 |
| 业务经理/一线运营 | 今天哪些客户、订单、库存需要处理 | 过滤、排名、权限、趋势和异常解释 | 核验现场、执行或审批 |
| 业务分析师/领域专家 | 指标口径是否正确，SQL 为什么这样写 | 语义映射、SQL 生成、图表和引用 | 定义口径、审查 SQL、维护示例与 Benchmark |
| 数据产品 Owner | 哪些数据和语义支持该域 | 资产发现、关系与使用信号 | 资产质量、认证/弃用、SLO 和变更管理 |
| 数据工程师 | 数据如何接入、刷新、关联 | 对业务用户隐藏表和 Pipeline 复杂度 | Pipeline、质量、主外键、新鲜度和性能 |
| 平台/AI CoE | 如何统一发布、监控和控成本 | Agent/API 的平台能力 | 架构标准、CI/CD、预算、可观测性和退出机制 |
| 安全/合规 | 为什么用户能看到这个答案 | 权限继承、血缘和审计 | 策略设计、敏感域评审和写操作审批 |

### 7.2 典型故事：从“收入为什么下降”到组织学习

**第一幕：一句普通人的问题。** 区域总经理在周一早晨问：“为什么华东区本月收入比计划低？” Genie 先把“本月”解释为公司财务月，把“收入”映射到已认证的 Revenue Metric View，并选择财务域 Agent 作为权威来源。

**第二幕：接受孩子式追问。** Genie 回答收入差异主要来自 A 产品销量下降和两笔订单延迟，并给出产品、渠道、客户的贡献拆解、生成 SQL 和来源引用。总经理继续问：“为什么不是价格？为什么相信这两笔订单？”系统必须给出价格影响较小的证据，并链接订单事实、CRM 阶段定义与财务确认规则。

**第三幕：讲不清就暴露缺口。** 当用户追问“延迟订单是否已经计入承诺收入”时，系统发现销售 Agent 与财务 Agent 对 committed revenue 的定义不同。正确行为不是强行给出一个听起来合理的答案，而是展示冲突、两个定义及来源，并请求领域负责人确认。

**第四幕：回到知识资产补课。** 财务分析师与销售运营确认董事会口径：只有已签合同且满足履约条件的订单才进入确认收入。数据团队更新 Revenue Metric View，业务负责人在 Page 中写明定义，旧 Dashboard 标记 deprecated，正确 SQL 保存为 Trusted Asset，并把争议问题加入 Benchmark。

**第五幕：再讲一遍。** 更新后 Genie 重答：“本月收入少 8%，其中 5 个百分点来自 A 产品销量，2 个百分点来自两笔尚未满足确认条件的订单，价格影响不足 1 个百分点。”每句话附证据，用户无需知道底层表名，也能继续钻取。

**第六幕：从解释到行动。** 总经理要求生成本周经营简报，并设置每日监控；若确认收入较 7 日均值下降超过阈值，则发送预警并附 Top drivers。销售经理收到风险订单清单后，在人工确认下创建跟进任务。

**第七幕：组织真正学会了什么。** 这次对话没有只产生一个答案，而是把一次“讲不清”转化为更好的 Metric View、Page、认证资产、Agent 规则和 Benchmark。下一位用户从一开始就会得到更可靠的解释。

> **费曼式运营原则：每次讲不清的地方，都是 Ontology 应补的知识；每次用户纠正，最终都应尽量沉淀为可治理、可验证、可回归的语义资产。**

### 7.3 费曼式提问模板

| 层次 | 用户问题 | 系统应给出的证据 |
|---|---|---|
| 现象 | 发生了什么？ | KPI、时间范围、对比基准、显著变化 |
| 定义 | “收入/活跃用户/缺货”到底是什么？ | 指标公式、维度、排除项、业务日历 |
| 来源 | 这个数从哪里来？ | 权威资产、血缘、SQL 或文档引用 |
| 权威 | 为什么相信它而不是另一个来源？ | 认证、Owner、使用、新鲜度和冲突说明 |
| 原因 | 为什么会发生？ | 多维贡献、假设、证据与不确定边界 |
| 缺口 | 哪些地方你不知道？ | 数据缺失、语义冲突、权限限制、模型不确定性 |
| 行动 | 现在能做什么？ | 候选动作、影响、责任人和需审批项 |
| 学习 | 如何避免下次再问不清？ | 更新 Metric View、Page、Agent、Trusted Asset 和 Benchmark |

## 八、企业落地路线图

| 阶段 | 目标 | 关键交付物 | 闸门标准 |
|---|---|---|---|
| 0. 定义价值与边界 | 选择一个业务域和 3–5 个可量化问题 | 用例章程、Owner、风险分级、成功指标 | 问题可验证，不以高风险自动写入为首批场景 |
| 1. 治理底座 | 让数据可找、可懂、可控 | UC 权限、Owner、描述、关系、血缘、认证资产 | 核心数据描述与敏感策略通过检查 |
| 2. 语义主干 | 固化 KPI 和业务概念 | Metric Views、Domains、Pages、同义词、业务日历 | 与现有报表对账通过 |
| 3. 领域 Agent | 把高频问法和可信逻辑装入 Agent | Instructions、示例 SQL、Trusted Assets、Benchmark | 代表性问题达到约定准确率 |
| 4. 用户试点 | 在真实工作流中验证采用与价值 | 试点用户、反馈队列、监控看板 | 使用率、正确率、分析时长改善达标 |
| 5. Cowork 自动化 | 从问答升级为报告、技能与预警 | Skills、Schedules、模板、异常规则 | 输出审阅和告警责任闭环达标 |
| 6. 嵌入与行动 | 进入 Teams/App/API/MCP，开放有限写操作 | OAuth、身份透传、审批、幂等、审计、回滚 | 安全与业务 Owner 双重批准 |
| 7. 规模化运营 | 多域复制和统一治理 | CI/CD、Agent Registry、Supervisor、成本/质量 SLO | 每个域都有 Owner、Benchmark 和停用机制 |

可用一句治理原则概括实施方法：**显式语义管主干，推断上下文补长尾，认证资产定权威，Benchmark 管质量，Unity Catalog 管权限，人工审批管行动。**

## 九、建议衡量指标

| 维度 | 指标示例 |
|---|---|
| 业务价值 | 报告准备时间、平均找数时间、决策周期、人工数据请求量、异常发现提前量 |
| 答案质量 | 首答正确率、SQL 正确率、引用覆盖率、澄清率、人工修正率、Benchmark 通过率 |
| 用户采用 | 周活跃用户、重复使用率、每用户有效问题数、Skill/Agent 复用率、业务域覆盖 |
| 平台运营 | P50/P95 延迟、失败率、SQL Warehouse 成本、LLM/DBU 成本、告警噪声率 |
| 治理 | 认证资产命中率、敏感策略覆盖率、权限拒绝准确性、审计完整性、过时定义清理时长 |
| 行动安全 | 人工确认率、误操作率、重复执行率、回滚成功率、未授权写入事件数 |

## 十、最终判断

Genie Ontology 最值得关注的，不是“自动生成了一个知识图谱”这个产品表述，而是 Databricks 把**语义、治理、使用信号、Agent 上下文和执行引擎**放进了同一个反馈闭环。它把企业 AI 的竞争焦点从“模型会不会写 SQL”推向“企业是否拥有可治理、可执行、可持续学习的业务上下文”。

对已将核心数据、权限和分析工作负载集中在 Databricks/Unity Catalog 的企业，这种原生耦合能显著降低跨平台拼装成本。对高度异构、存在多个数据目录、BI 语义层和 Agent 平台的企业，则应在试点前明确：哪个系统是指标主权源，哪些上下文可跨平台交换，哪些写操作必须保留人工确认。

最稳妥的起点不是建设“全公司万能 Agent”，而是选择一个数据较成熟、口径明确、问题高频、风险可控的业务域，先做出一个**讲得清、证据足、纠错后能沉淀、下次能复用**的闭环。只要这个闭环成立，Ontology 才真正从产品能力变成企业能力。

## References

[1]: https://www.databricks.com/blog/introducing-genie-one-genie-ontology-and-genie-agents "Databricks — Introducing Genie One, Genie Agents, and Genie Ontology"
[2]: https://docs.databricks.com/aws/en/genie-one/chat "Databricks Docs — Chat in Genie One"
[3]: https://docs.databricks.com/aws/en/uc-semantics/ "Databricks Docs — Unity Catalog semantics"
[4]: https://docs.databricks.com/aws/en/uc-semantics/metric-views/ "Databricks Docs — Unity Catalog metric views"
[5]: https://docs.databricks.com/aws/en/genie-agents/concepts "Databricks Docs — Genie Agents concepts"
[6]: https://docs.databricks.com/aws/en/genie-agents/monitor "Databricks Docs — Test and monitor a Genie Agent"
[7]: https://docs.databricks.com/aws/en/ai-bi/release-notes/2026 "Databricks Docs — AI/BI and Genie One release notes 2026"
[8]: https://docs.databricks.com/aws/en/genie/ "Databricks Docs — Genie product family"
[9]: https://www.databricks.com/blog/get-started-genie-one-top-ai-cowork-use-cases-business-users "Databricks — Top AI Cowork Use Cases for Business Users"
[10]: https://docs.databricks.com/aws/en/data-governance/unity-catalog/ "Databricks Docs — What is Unity Catalog?"
[11]: https://docs.databricks.com/aws/en/genie-agents/conversation-api "Databricks Docs — Use the Genie Agents API"
[12]: https://docs.databricks.com/aws/en/lakehouse-architecture/scope "Databricks Docs — The scope of the Databricks platform"
[13]: https://datahub.com/blog/what-is-databricks-genie-one/ "DataHub — What Is Databricks Genie One?"
[14]: https://www.databricks.com/blog/cross-industry-technology-and-functional-genie-partner-solutions "Databricks — Cross-industry Technology and Functional Genie Solutions"
[15]: https://github.com/databricks-solutions/genie-ontology-readiness/blob/main/README.md "Databricks Solutions — Genie Ontology Readiness"
[16]: https://subjectguides.york.ac.uk/study-revision/feynman-technique "University of York — The Feynman Technique"
[17]: https://www.colorado.edu/artssciences-advising/resource-library/life-skills/the-feynman-technique-in-academic-coaching "University of Colorado Boulder — The Feynman Technique"

## 相关页面

- [[Wiki/Wiki 索引|返回知识索引]]
- [[对话式AI助手-开源前端框架选型指南]]：AI 应用交互层的选型资料。
- [[Projects/系统搭建助手/SYSTEM_BUILDER_E2E_RUN_20260827|系统搭建助手 E2E 执行记录]]：另一产品的本体生成实测，供比较测试方法；不构成本文 Databricks 产品结论的验证证据。
