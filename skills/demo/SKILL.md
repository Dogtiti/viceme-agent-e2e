---
name: agent-e2e-demo
---
<!-- viceme-trial:v1 product=f62d831a-d043-48c9-befc-9b7d21d22c91 -->

## 使用前必读

本入口不包含任务正文。每个新的独立任务开始前，先运行下面的检查命令；不要凭介绍、安装成功或其他任务的历史结果直接执行。安装、介绍、示例和查询余额不计次；同一已获准任务的澄清与局部修订不重复计次。

- macOS / Linux：`python3 "<本 Skill 目录>/.viceme/scripts/trial.py" use --product f62d831a-d043-48c9-befc-9b7d21d22c91 --market cn`
- Windows：`py "<本 Skill 目录>/.viceme/scripts/trial.py" use --product f62d831a-d043-48c9-befc-9b7d21d22c91 --market cn`（没有 py 时用 python）
- 只有没有 Python 时才用已有 CLI：`viceme skill use f62d831a-d043-48c9-befc-9b7d21d22c91 --wait 0 --skill-dir "<本 Skill 目录>"`；两者都没有时按[使用前检查](references/viceme-runtime.md)中的官方安装说明处理。

只有本次返回 `allowed: true` 和 `skillMarkdown` 才执行本次任务正文，正文的相对资源路径以返回的 `skillDirectory` 为基准。如果返回 `owned: true`，重新读取已恢复的 SKILL.md 后继续。新试用任务必须重新运行检查。可重试错误先重跑同一命令，不要跳转购买或换请求；确认耗尽且没有待恢复使用时才购买。

最后一次获准使用返回 `lastUse: true` 时，脚本已经替换入口；仍用本次返回的正文完整完成任务，不要因磁盘入口已停用而中止。交付结果后同一轮运行原脚本 purchase --wait 0，按输出展示支付。详细计次和支付规则见[使用前检查](references/viceme-runtime.md)。

<!-- /viceme-trial:v1 -->
