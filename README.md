# dsh-plugins

DoiiarX 设计和维护的 DeepSeek Harness（DSH）插件合集。这里汇总所有自研插件，每个插件在自己的仓库中维护，并回链到本索引。

## 插件列表

| 插件 | 包名 | 说明 | 仓库 | 状态 |
| --- | --- | --- | --- | --- |
| Xget 加速 | `@local/dsh-xget` | 在设置页配置 xget 镜像，自动为 npm/npx、pip、git、Go、Hugging Face 注入加速代理环境变量 | [dsh-xget-plugin](https://github.com/DoiiarX/dsh-xget-plugin) | ✅ 已开源 |
| 应答语言 | `@local/dsh-user-language` | Web 设置页「用户语言」小节 + 系统提示词语言注入，避免中文提问得英文回复 | [dsh-user-language](https://github.com/DoiiarX/dsh-user-language) | ✅ 已开源 |
| Todo 门禁 | `@local/dsh-todo-continuation` | 停止门禁 + 无 Todo/过期 Todo 提示：未完成不放行结束，长期不用或不更新 Todo 时建议性提示，阈值可在设置页配置 | [dsh-todo-continuation](https://github.com/DoiiarX/dsh-todo-continuation) | ✅ 已开源 |
| 设置搜索 | `@doiiarx/dsh-settings-search-plugin` | 设置面板独立搜索：候选列表 + 点击跳转到所属小节并聚焦 | [dsh-settings-search-plugin](https://github.com/DoiiarX/dsh-settings-search-plugin) | ✅ 已开源 |
| Windows Shell | `@local/dsh-shell-plugin` | 可配置的 Windows shell：独立注册 cmd/bash/pwsh 工具，禁用系统 pwsh | [dsh-shell-plugin](https://github.com/DoiiarX/dsh-shell-plugin) | ✅ 已开源 |
| 天气 | `@local/dsh-weather-plugin` | 封装 wttr.in 天气 API 为模型工具 | [dsh-weather-plugin](https://github.com/DoiiarX/dsh-weather-plugin) | ✅ 已开源 |
| 网络代理 | `@local/dsh-proxy-plugin` | 提供网络代理能力（socks5h/http） | [dsh-proxy-plugin](https://github.com/DoiiarX/dsh-proxy-plugin) | ✅ 已开源 |
| 工作区 Board | `@local/dsh-board-plugin` | 工作区级持久共享工作台：多 agent 集群并行基础能力（8 个工具） | 待开源 | 📝 本地 |
| OpenAPI 工具 | `@local/dsh-oneapi-plugin` | 桥接 OpenAI tools/functions 与 OpenAPI 3.x 格式，注册为可搜索、可调用的函数 | [dsh-oneapi-plugin](https://github.com/DoiiarX/dsh-oneapi-plugin) | ✅ 已开源 |
| JSON 扁平化 | `@local/dsh-json-flat-plugin` | JSON 扁平视图/编辑工具（schema 推断、搜索、路径编辑） | [dsh-json-flat-plugin](https://github.com/DoiiarX/dsh-json-flat-plugin) | ✅ 已开源 |

## 安装

每个插件是独立的 DSH profile bundle，通过 profile 的 `package.json` 以
`link:` 方式链接，并在 `dsh.profile.bundles` 中列出。以 xget 插件为例：

```json
{
  "dependencies": {
    "@local/dsh-xget": "link:D:/Programs/dsh-xget-plugin"
  },
  "dsh": {
    "profile": {
      "bundles": ["@local/dsh-xget"]
    }
  }
}
```

## 通用约定

- 双面包：宿主端 `index.js`（能力/工具/settings）+ 浏览器端 `client.js`
  （设置页 UI，经 `window.__ModuleLoader__.load` 注册）。
- 失败隔离：`apply()` 内动态 import，任何失败降级为诊断日志，不拖垮 profile。
- 持久化配置走 `ctx.settings.register`（settingsScope 绑定命名空间）。
- 跨插件协作：`@local/dsh-xget` 依赖 `@local/dsh-tool-cmd` 的
  `shellMiddlewareSlot`（set 模式：`use(owner, fn)` 记录来源 + disposer 自清理）。
