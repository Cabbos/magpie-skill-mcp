# 在 Magpie 配置 MCP

以下 7 项对应原来 Library 中的 MCP。可以用 Magpie 界面配置，也可以在 Bash 或 Zsh 中运行各节命令。每条 `mcp add` 先登记到 Library，`agents=none` 让你稍后自行选择 Agent。如果库中已有同名条目，先在 **Library → MCP Servers** 查看，再决定是否更新。 [Magpie Library 文档](https://usemagpie.ai/docs/library)

## 用 Magpie 界面添加 · Windows、macOS

打开 **Library → MCP Servers → Add server**，按下方各节填写对应的名称和连接设置。

- HTTP 服务选择 **HTTP**，填写 **URL**，例如 `https://developers.openai.com/mcp`。
- stdio 服务选择 **Command**，在同一个输入框填可执行文件和参数。取 `mcp add` 中名称后的可执行文件，再接上 `--` 后的参数。将 `$(command -v …)` 换成本机安装的程序路径。Windows 上通过 npm 命令脚本启动时，在命令前加 `cmd.exe /c`，例如 `cmd.exe /c gitnexus mcp`。
- 命令标了 `--local` 的条目在单条表单中关闭 **同步到其他电脑**。飞书凭据只填在本机配置里。保存前检查 **Agents**，取消勾选用不到的 Agent。

也可以在添加窗口内直接粘贴标准 MCP JSON，例如下方的 OpenAI 文档接口配置。

```json
{
  "mcpServers": {
    "openaiDeveloperDocs": {
      "url": "https://developers.openai.com/mcp"
    }
  }
}
```

## OpenAI 开发者文档 · `openaiDeveloperDocs`

公开文档检索接口，可以第一个配置，无需 API Key。 [OpenAI Docs MCP](https://developers.openai.com/learn/docs-mcp)

```sh
magpie library mcp add openaiDeveloperDocs https://developers.openai.com/mcp agents=none
```

## GitNexus · `gitnexus`

按上游要求安装兼容的 Node.js，再执行 `npm install -g gitnexus`。在有权限分析的代码仓库根目录执行 `gitnexus analyze --index-only`，生成供 MCP 查询的索引。 [GitNexus 安装和索引](https://github.com/nxpatterns/gitnexus#quick-start)

```sh
magpie library mcp add gitnexus "$(command -v gitnexus)" agents=none --local -- mcp
```

## BrowserOS neo · `browseros-neo`

安装并启动 BrowserOS neo，在应用侧栏 **MCP** 中复制 URL。将下方示例替换成应用显示的本机地址，端口以当前安装为准。 [BrowserOS neo MCP 配置](https://docs.browseros.com/neo/mcp/manual)

```sh
BROWSEROS_MCP_URL='http://127.0.0.1:9200/mcp'
magpie library mcp add browseros-neo "$BROWSEROS_MCP_URL" agents=none --local
```

## 飞书 · `feishu`

需要 Node.js、`npx` 和有相应权限的飞书开放平台应用。以下命令从终端交互读取本机的 App ID 和 App Secret。请在个人本地终端输入，Magpie 和选中的 Agent 会在本机配置中保存启动参数。`--local` 让该条目留在这台机器。需要以用户身份访问时，按上游说明配置 OAuth。 [飞书 OpenAPI MCP 快速开始](https://github.com/larksuite/lark-openapi-mcp#quick-start)

```sh
printf '飞书 App ID: '
read -r FEISHU_APP_ID
printf '飞书 App Secret: '
read -r -s FEISHU_APP_SECRET
printf '\n'

if [ -n "$FEISHU_APP_ID" ] && [ -n "$FEISHU_APP_SECRET" ]; then
  magpie library mcp add feishu "$(command -v npx)" agents=none --local -- \
    -y @larksuiteoapi/lark-mcp mcp -a "$FEISHU_APP_ID" -s "$FEISHU_APP_SECRET" -m stdio
fi
unset FEISHU_APP_ID FEISHU_APP_SECRET
```

## Android Use · `android-use`

按上游说明安装 `au` 并完成本机设备登记，先用 `au doctor` 检查状态。下方用上游的 stdio 启动方式，适用于新机器上由一个 Agent 连接的情况。如果该设备已由 Android Use 后端管理，请沿用现有连接，每个设备仅启动一个后端。 [Android Use 客户端文档](https://github.com/austinintelligence/android-use/blob/main/docs/agents/clients.md)

```sh
magpie library mcp add android-use "$(command -v au)" agents=none --local -- serve --mcp
```

## iPhone Use · `iphone-use`

在 macOS 上按上游指南安装并配对 iPhone，使用 `iphone-use doctor` 或 `iphone-use status` 检查环境。将下方路径改为本机 `iPhoneUse.app` 内的 `iphone-use-mcp` 可执行文件。MCP 程序从已安装的本地服务读取连接配置。 [iPhone Use 安装指南](https://github.com/leeguooooo/iphone-use/blob/main/docs/guide.md#install-and-connect-the-first-phone)

```sh
IPHONE_USE_MCP_BIN='/path/to/iPhoneUse.app/Contents/MacOS/iphone-use-mcp'
magpie library mcp add iphone-use "$IPHONE_USE_MCP_BIN" agents=none --local
```

## Kimi Computer Use · `kimi-cu`

此模板适用于 macOS。按官方说明安装 Kimi Computer Use 并授予所需的辅助功能和录屏权限，再将下方路径改为本机 `KimiCU.app` 内的 `kimi-cu` 可执行文件。 [Kimi Computer Use 官方指南](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/plugins#kimi-computer-use-v0-5-4)

```sh
KIMI_CU_BIN='/path/to/KimiCU.app/Contents/MacOS/kimi-cu'
magpie library mcp add kimi-cu "$KIMI_CU_BIN" agents=none --local -- mcp
```

## 启用和验证

在 **Library → MCP Servers** 中打开刚添加的条目，勾选需要它的 Agent。也可以在终端指定 Agent，例如只给 Codex 添加 OpenAI 文档 MCP。

```sh
magpie library mcp agents openaiDeveloperDocs codex
```

重新打开目标 Agent，先检查该 MCP 是否已连接，再用一次只读操作验证。例如让 Agent 用 `openaiDeveloperDocs` 查询 Responses API 文档。设备、浏览器和团队服务还需要在目标机器完成对应的本机授权。取消分发时，将命令最后的 Agent 改为 `none`。
