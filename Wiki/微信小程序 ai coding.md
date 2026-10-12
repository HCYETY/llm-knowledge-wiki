
# 一、原型阶段

> **「brainstorming」 212\.9K installs**

对于一个新项目，在不了解怎么设计的时候，可以调用 **brainstorming** skill。它会对你的 **暂无明确要求** 的需求进行**头脑风暴**：
![[Attachments/image 2.png]]

安装命令：
```Plain Text
npx skills add https://github.com/obra/superpowers --skill brainstorming
```

它会启动 **plan** 模式，我们只需回答问题直到 **明确需求**：
![[Attachments/image 5.png]]

**明确需求后**，会输出**方案对比**：
![[Attachments/image 3.png]]

在**确定方案后**，会开始输出**原型设计**：
![[Attachments/image.png]]

如果有异议可以**提出修改意见**；确认无误后允许执行命令**生成设计规格文档**：
![[Attachments/image 1.png]]

设计规格文档审查没问题后，继续执行生成设计文件即可：
![[Attachments/image 4.png]]
# 二、UI阶段

此时脑子已经对产品有个概念了，但是原型设计还比较粗糙，我们还需要打磨一下界面，这是就设计到专业设计 ui/ux 的skill。我实践了几个相关skill：`UI UX Pro Max`、`Frontend-Design`，同样的命令出图效果感觉还是 `UI UX Pro Max` 更胜一筹。

安装命令：
```Plain Text
npx skills add https://github.com/nextlevelbuilder/ui-ux-pro-max-skill --skill ui-ux-pro-max
```

> 如果需要二次优化，可以使用 **impeccable**
> 
> impeccable 是"1 个 Skill、18 个命令，以及一组经过整理的前端设计反模式"。它不是只负责从零生成一个页面，而是更强调给 AI 一套设计语言和检查工具，用来审查、润色、压低 AI 常见的前端坏习惯。
> 
> 它明确提到自己是在 Anthropic Frontend\-Design 的基础上扩展出来的，提供更深的专业性和更多控制能力。对应的命令也很细，比如 `/audit` 做质量检查，`/critique` 做 UX 评审，`/polish` 做最终打磨，`/typeset` 修字体层级，`/layout` 修布局节奏，`/animate` 补有目的的动效。
> 
> 一句话评价：适合用来做二次优化，把"能用"修成"顺眼"。
> 
> 支持工具（2026 年最新）：
> - Cursor（强烈推荐，需开启 [Nightly 通道](https://zhida.zhihu.com/search?content_id=272578700&content_type=Article&match_order=1&q=Nightly+%E9%80%9A%E9%81%93&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODEzMjA0MTUsInEiOiJOaWdodGx5IOmAmumBkyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI3MjU3ODcwMCwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.kmWLG-T7RfgPLlD_bNHoFj_6on6qnpm0KUqicewGqXY&zhida_source=entity) \+ Agent Skills）
> - Claude Code / Claude Desktop
> - Gemini CLI、Codex CLI、Trae（国内/国际版）
> - OpenCode、Pi、Kiro、Rovo Dev、VS Code Copilot 等

安装命令：
```Plain Text
npx skills add https://github.com/pbakaus/impeccable --skill impeccable
```
# 三、开发阶段

## 搭建框架

在 `HBuilder` 中新建 **uni\-app** 项目：
![[Attachments/image 6.png]]
会自动生成项目基础模板

## 测试先行

采用 **测试驱动开发\(TDD\)** 的模式，这是因为小程序调试成本高，改完代码需要重新编译，真机测试还要扫码预览，因此这里使用 **「test\-driven\-development」“121\.7K installs”**

安装命令：
```Shell
npx skills add https://github.com/obra/superpowers --skill test-driven-development
```

**test\-driven\-development** 通过“红\-绿\-重构”的循环来驱动开发过程：

1. **红**：编写一个新的测试用例，初始状态下测试未通过。
2. **绿**：编写足够的代码使测试通过。
3. **重构**：在确保测试通过的基础上，优化和重构代码，以提高代码质量和可维护性

因此先写测试用例，再写功能代码。

## 开发功能代码
结合原型阶段的md文档和UI阶段的设计稿，开发小程序页面。

## 运行项目
需要有 `manifest.json` 文件说明项目类型。
> [HBuilderX: 项目类型运行说明](https://ask.dcloud.net.cn/article/35878)

# 四、后续阶段
我们需要有自己的【军火库】，以便 ai 在后续的开发中更加智能和高效。因此最后一个操作，就是继续在刚才的对话窗口中发送以下指令：

*基于我们刚才写小程序的对话，结合insights报告给我整理一下我以后可能需要生成的skills*


## 当项目进入长期维护

原型和早期页面可以用较轻的 Vibe Coding 快速推进；当小程序开始涉及长期迭代、跨页面状态、后端接口约定、权限边界和多人协作时，应把关键项目事实从聊天记录迁入仓库。

最低限度应保存需求与验收条件、技术方案、原子任务、测试约定和关键设计决策。若还存在跨会话恢复、多人交接或多种 AI 工具并用的问题，可以进一步采用 [[Trellis：项目级 Spec Coding 闭环]] 一类 Harness 管理任务状态与经验回流。

相关方法：[[Vibe Coding 与 Spec Coding]]、[[Claude code + Codex 项目最佳实践]]。

## 导航与资料状态

- [[Wiki/Wiki 索引|返回知识索引]]
- 2026-10-10 已从原始下载目录恢复原型阶段的 6 张截图，并按原始 Markdown 修正文件名对应关系；全部 7 张配图见 [[Attachments/附件索引|附件索引]]。



