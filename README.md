# 插件清单

| 插件 | 为什么需要它 | profile 兼容性 |
|---|---|---|
| [`dsh-local-policy`](https://github.com/xuan25/dsh-local-policy) | 它以纯 patch 捆绑（无代码）施加本地部署策略：把框架默认的 web 搜索提供方从 DeepSeek 切换为通用 HTTP 提供方，并禁用 DeepSeek 官方集成与 OTel 会话上报。 | ✅ web/headless/sdk/acp；⚠️ sdk-minimal（patch 引用的 entry id 在该独立树中不存在） |
| [`dsh-logger-channels`](https://github.com/xuan25/dsh-logger-channels) | 它给操作者提供框架与全部插件 `ctx.logger` 输出的可见出口：向 cordis 注册命名通道（stderr/stdout/file，file 通道纯大小轮转），因为宿主只自带内存环形缓冲，没有它日志只进内存。 | ✅ web；其余长驻 profile 原理上可兼容——未验证 |
| [`@openviking/dsh-memory-plugin`](https://github.com/volcengine/OpenViking) | 它给 agent 提供由 OpenViking 支撑的长期记忆：跨会话读写记忆，并在共享同一 OpenViking 的 profile 之间传递上下文。 | ✅ web/headless/sdk/acp；❓ sdk-minimal（MCP client 可用性未验证） |
| [`dsh-timer`](https://github.com/xuan25/dsh-timer) | 它给 profile 提供进程内定时任务，语义对齐 systemd.timer，每次运行创建一个新 agent 会话执行任务 prompt。 | ✅ web；⚠️ headless（30s tick 调度器在一次性进程中无意义）；✅ sdk/acp（长驻进程）；❌ sdk-minimal |
| [`dsh-email`](https://github.com/xuan25/dsh-email) | 它让 agent 能读写邮件：对 process env 配置的 IMAP/SMTP 账号提供列目录、搜索、读取、标记、保存附件与发送/回复/转发，并支持 IDLE 新邮件触发，使 agent 能实时响应来信。 | ✅ 全部 profile（软读取、优雅降级）；⚠️ headless（IDLE 触发在一次性进程中无意义） |
| [`dsh-ntfy`](https://github.com/xuan25/dsh-ntfy) | 它给 agent 提供推送通知出口：一次工具调用即把消息发布到 ntfy topic，使作业结果、告警与报告能送达操作者设备或任意订阅该 topic 的应用。| ✅ web/headless/sdk/acp；❌ sdk-minimal（硬依赖 skills 服务） |
| [`dsh-webhook-dispatcher`](https://github.com/xuan25/dsh-webhook-dispatcher) | 它给 profile 提供入站 webhook 入口：端点路由挂在框架主 web server 上，外部投递经 method 门控、token 鉴权、请求体大小限制与每端点队列/并发约束后，派发为受控 trigger 会话，使外部系统能触发 agent。 | ✅ web；其余 profile 需组合 `dsh-host-webserver` |
| [`dsh-surfing-plugin`](https://github.com/xuan25/dsh-surfing-plugin) (forked) | 它为 agent 的联网能力提供后端：基于 SearXNG 的搜索与基于 Crawl4AI 的页面抓取，双组装可按部署切换。| ✅ web/headless/sdk/acp；⚠️ sdk-minimal（无 base web_search） |
| [`dsh-tts`](https://github.com/xuan25/dsh-tts) | 它让 agent 能合成语音：单个 `tts` 工具（synth/voices/providers/models/status/phonemize 动词）对接任意 OpenAI 兼容 TTS 后端。 | ✅ web/headless/sdk/acp；❓ sdk-minimal |
| [`@tencent-connect/dsh-qqbot`](https://github.com/xuan25/dsh-qqbot) (forked) | 它给 profile 提供 QQ IM 通道：维持与 QQ 开放平台的 WS 长连接，处理 QQ 消息的收发与 bot 生命周期，使 agent 能经 QQ 与用户实时交互。 |  ✅ web；❌ headless（一次性进程 + 交互式 QR 绑定）；✅ sdk/acp/sdk-minimal |
| [`dsh-qqbot-send`](https://github.com/xuan25/dsh-qqbot-send) | 它让 agent 能主动把文本、Markdown 或文件附件发送到任意 QQ 目标（私聊/群/频道/公会 DM）；直接调用 QQ 开放平台 REST API，不依赖网关插件的长连接，因此可以在没有网关的 profile 中单独挂载。 |  ✅ web/headless/sdk/acp；❓ sdk-minimal |

图例：✅ 兼容 / ⚠️ 可组成功能降级 / ❌ 不兼容 / ❓ 未验证。(forked) = 个人分叉版本。
