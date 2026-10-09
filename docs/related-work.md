# 同类项目与差异

核查日期：2026-10-09。基于各项目自己的文档，尚未本地安装或做功能评测。未检索到某项证据不等于该项目没有该能力。

| 项目 | 已公开描述的相关能力 | 与 TurnAnnotation 的关系 |
| --- | --- | --- |
| [Plannotator](https://github.com/backnotprop/plannotator) | 本地浏览器评审界面，批注计划、文档和 agent 消息并回传；`plannotator-last` 可批注最后一条回复，历史本地保存；支持多个 harness | 高度重合，必须作为直接对照；不能将其概括为仅计划评审 |
| [Herdr Annotate](https://github.com/plannotator/herdr-annotate) | 在 Herdr 中批注终端文本、文档和 agent 回复，反馈作为下一条消息发送，支持 Codex/Claude Code 等 | 同样高度重合；依赖 Herdr 这一宿主，其选区机制不等于 Codex/CC 自身的原生扩展 |
| [VSCode Agent Annotator](https://github.com/etsd-tech/vscode-agent-annotator) | VS Code 原生代码评论批量格式化，经 Claude Code Channels 发回会话 | 很接近“便利化批注”的思路，但主要目标是文件/代码行，且需要检查 Channels 的消息身份及环境限制 |

Plannotator 的当前 README 还描述了在较新 Claude Code 上通过 Mod 异步送回消息的路径，因此不能把“回传原生会话”当作 TurnAnnotation 独有能力。

## 本项目选择的研究重点

- 在原生回复表面批注，并保留正常输入框的一次提交体验。
- 反馈组装透明、确定、尽量短，保持普通 user message 语义。
- 历史用于人查看，不额外注入上下文。
- 用可复现的配对实验衡量质量非劣效、请求落实和 token 开销。

以上是本项目的目标，不是对其他项目缺失功能的断言。当前没有足够证据声称这些项目已经或尚未证明质量/token 优势。

## 后续对照建议

先用同一份“原文＋批注＋自由输入”记录各项目实际产出的用户反馈。若能复用其格式或原生接入方式，应优先考虑复用/贡献，而不是为独立项目重复实现所有 UI。未核对具体文件许可证前，不复制代码。

反馈组装实验仍以高质量手工短引用作为主对照；其他工具可作为额外基线，但不能替代主对照。

具体可交付工作与优先级见 [研究与工程工作](research-agenda.md)。不将“现有项目没有该能力”作为前提，而是对相同案例采集实际输出并检验。
