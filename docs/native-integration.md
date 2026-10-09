# 原生接入调研

核查日期：2026-10-09。以下区分官方文档能力、当前机器观察和待验证问题；未安装插件、未升级客户端、未对真实会话执行输入实验。

## 初步选择：先验证 Claude Code Mods

官方文档提供以下组合：

- Mods 可以绘制 Pane 和输入组件，也可以参与 AssistantMessage 等消息界面的渲染。[概览](https://code.claude.com/docs/en/plugins/mods/overview)、[界面文档](https://code.claude.com/docs/en/plugins/mods/interface)
- `prompt.submit` 事件可以通过 `next({ ...e, text })` 修改本次用户输入。[事件文档](https://code.claude.com/docs/en/plugins/mods/events)
- API 也支持 `$.prompt.submit({ text, asUser: true })` 发起用户身份的输入；普通 `submit` 会附带 Mod 来源提示。因此若使用此入口，必须确认普通 user message 语义、触发时机和去重。[Mods API](https://code.claude.com/docs/en/plugins/mods/api)
- `session.append`、`prompt.fill`、`prompt.edit` 等事件是读取消息、草稿交互的候选入口；具体字段必须以目标版本类型定义为准。[参考](https://code.claude.com/docs/en/plugins/mods/reference)

这是“UI 批注 → 本地组装 → 本次普通用户输入”的直接候选路径。普通 settings hook 的 `additionalContext` 不能自动视为同等实现，因为它改变了消息进入上下文的渠道。

文档标明终端 Mods 从 v2.1.287 起正式支持，Desktop Code tab 的对应版本从 v2.1.286 起支持。CLI 和 Desktop 能显示 Mod UI，VS Code chat panel 与 headless SDK 不显示这些界面。来源：[运行环境](https://code.claude.com/docs/en/plugins/mods/overview#where-mods-run)。

当前机器 `claude --version` 为 **2.1.260**。因此文档中的能力尚不能推定为本机可用；后续 spike 需固定支持版本并记录验证结果。

### 尚未证实的关键部分

- 能否直接取得原生回复的任意文本选区及稳定消息身份。
- 是否能在保留原生消息渲染的情况下标记范围与批注。
- 是否能将批注草稿与用户原有输入、附件一起预览并单次提交。
- 重启、resume、branch、流式回复和窄终端下的行为。
- 多插件共存时，最终文本是否与预览一致。

“有侧栏 API”不等于已支持完整 Word 式选区。若这一部分不成立，应记录实际缺口，再评估原生段落级入口或向宿主提交改进。

## Codex

Codex app-server 提供 thread、turn、history 和 streaming 等接口，适合自定义客户端，但不等于获得原生桌面应用回复区的选区和渲染扩展能力。[官方文档](https://developers.openai.com/codex/app-server/)

OpenAI Plugin Extensions 支持 conversation panel 等界面入口。现有资料不足以证明目标 Codex 表面向插件开放原生回复任意选区、挂批注及组合输入框的全部能力。[官方文档](https://developers.openai.com/plugins/build/extensions)

当前机器 `codex --version` 为 **0.153.4**。不能用最新网页文档代替对该版本的验证。

Codex CLI 的开源组件也是原生修改的候选，但维护 fork、分发新 CLI 与插件安装的成本不同。除非扩展路线被证实不满足目标，暂不选择客户端 fork 或 app-server 自建完整 UI。

## 最小技术 spike 的通过条件

1. 从真实回复定位文字，无截图、OCR、坐标或 DOM 抓取。
2. 创建两条批注；草稿及记录只留在 UI/本地。
3. 普通输入框增加一条无关问题。
4. 查看合并后的确切文本；一次用户动作产生一条普通 user message。
5. 没有额外模型调用，原有工具、项目指令和权限保持原生行为。
6. 下一轮输入不自动重带历史批注。
7. 重启后仍能查看旧批注和当时原文。

每项保存宿主版本、实际接口、结果和复现步骤。只有通过后才将“Claude Code 优先”升级为“已支持”。

