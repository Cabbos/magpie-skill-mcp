# Magpie Skills 和 MCP 配置包

把常用的开发 Skill 和 MCP 配置迁移到新电脑上的 Magpie。Skill 保留 `SKILL.md` 和所需的配套文件，MCP 可逐项添加给指定的 Agent。

## 在新电脑上使用

1. 安装 Magpie，打开 **Library → Skills**。输入 `Cabbos/magpie-skill-mcp`，点 **查找技能（Find skills）**，勾选需要的 Skill 再安装。也可以下载并解压 ZIP，在同一个输入框选择解压后的文件夹。 [下载 ZIP](https://github.com/Cabbos/magpie-skill-mcp/releases/latest)
2. 按 [MCP 配置手册](mcp/README.md) 安装对应服务，在 Magpie 添加需要的 MCP。有登录要求的服务，使用新电脑上获准使用的账号或应用凭据。
3. 在 Magpie 勾选要接收各项配置的 Agent，重新启动该 Agent。在新会话里试用一个 Skill，并对启用的 MCP 发起一次低风险查询，确认新电脑的权限和连接都已就绪。

[Magpie 安装文档](https://usemagpie.ai/docs/start) · [Magpie Library 文档](https://usemagpie.ai/docs/library)

## 包内内容

- [`skills/`](skills/) 包含 86 个可直接导入的 Skill，按各自的上游许可证分发。[来源清单](SKILL_SOURCES.md) 记录固定版本、单独安装的上游入口和迁移差异。
- [`mcp/`](mcp/) 提供 7 项 MCP 接入说明，包含界面配置和终端命令。本地应用与设备型服务需要在新电脑上安装和授权。

本包从 Magpie `0.1.4404` 的 Library 整理。公开内容仅含可分发的 Skill 文件、公开上游来源及可复用的 MCP 配置说明。
