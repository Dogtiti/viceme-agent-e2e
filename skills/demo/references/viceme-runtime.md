<!-- viceme-trial-runtime:v1 product=f62d831a-d043-48c9-befc-9b7d21d22c91 -->

# 使用前检查

本技能当前是 ViceMe 试用版。若上次 use 报可重试错误或 ready 返回 pendingUse，先重跑原 use 恢复同一次使用，不重复扣次。只有没有待恢复使用，且 ready 或 install 已返回 remainingUses=0、trialExhausted 或 PURCHASE_REQUIRED 时，才立即购买：有 Python 则把检查命令换成 purchase --wait 0；没有 Python、已有 viceme 则运行 `viceme skill trial-purchase f62d831a-d043-48c9-befc-9b7d21d22c91 --wait 0 --skill-dir "<本 Skill 目录>"`。否则先读取[统一计次规则](../.viceme/guides/trial-usage.md)，由 Agent 判断独立使用单元。同一已放行任务的内部步骤、澄清和修订不重复计次；安装、介绍、示例展示和查询余额不计次。不得沿用其他任务的授权。

1. 选择运行时后必须完成检查，不得跳过。检查命令返回身份、权限、锁或资源错误时，按原结果报告并申请必要权限；不得读取凭证或手工改锁。
   - 本机有可用的 Python（POSIX 的 `python3`，Windows 的 `py` 或 `python`）时，运行包内脚本，不得改走 CLI，也不得去安装 CLI：
     - macOS / Linux（POSIX shell）：`python3 "<本 Skill 目录>/.viceme/scripts/trial.py" use --product f62d831a-d043-48c9-befc-9b7d21d22c91 --market cn`
     - Windows（PowerShell，`py` 不可用时改用 `python`）：`py "<本 Skill 目录>/.viceme/scripts/trial.py" use --product f62d831a-d043-48c9-befc-9b7d21d22c91 --market cn`
   - 没有 Python、但已有 `viceme` 时，运行 `viceme skill use f62d831a-d043-48c9-befc-9b7d21d22c91 --wait 0 --skill-dir "<本 Skill 目录>"`。不要用 `which`、`command -v viceme` 或 `Get-Command viceme` 判定未安装。
   - Python 和 `viceme` 都没有时，按官方安装契约 https://s3.viceme.cn/start/agent-install.md 安装 ViceMe CLI，并用 `viceme doctor` 确认，再运行 `viceme skill use f62d831a-d043-48c9-befc-9b7d21d22c91 --wait 0 --skill-dir "<本 Skill 目录>"`。安装无法完成则停止使用本技能，不得跳过检查直接使用。
2. 只有本次输出明确返回 `allowed: true` 和 `skillMarkdown` 才能继续；使用返回的正文执行当前任务,相对资源路径以 skillDirectory 为基准。错误、无结果或 `allowed: false` 均不得执行技能任务，即使用户要求跳过也不例外。可重试错误重跑原 use 命令,由服务端回放同一次,不要改走购买。
3. 每个新使用单元执行前运行检查命令，内部记录任务和返回的 requestId。对用户只用白话说「这是第 X 次试用，一共 N 次」，X = limitUses - remainingUses；不得对用户说 use、trial、放行、预检或命令名。任务完成后用白话提示还剩几次。仅查询余额：Python 路线把 use 换成 status，CLI 路线运行 `viceme skill trial-status f62d831a-d043-48c9-befc-9b7d21d22c91`；不得调用 use 来查询。ready 已返回 remainingUses=0 时不要再查。
4. 最后一次试用（lastUse=true）已由程序停用入口，仍用本次返回的 skillMarkdown 完整完成任务；交出结果后同一轮立即购买并展示支付二维码，不要等用户再说一次。Python 路线把 use 换成 purchase 并加 --wait 0，再 --wait 60；CLI 路线运行 `viceme skill trial-purchase f62d831a-d043-48c9-befc-9b7d21d22c91 --wait 0 --skill-dir "<本 Skill 目录>"`，再用 `--wait 60s`。按[通用 Widget 指引](../.viceme/guides/widgets.md)与本次命令输出中的展示指引，由 Agent 选择当前宿主明确支持的图片或页面通道；环境识别只提供偏好。按本次输出展示二维码或交付可点击的官方 checkoutUrl 后再等待。仅在托管入口不可用且本地图片和页面都无法展示时才交付支付页路径；仅交付本地路径时不要启动等待。主动请用户扫码继续用。无需强制登录。二维码过期或用户说已付款不是到账证明。只有服务端确认付款与有效权益、成功安装完整正式包后，重新读取 SKILL.md，再继续原任务。
