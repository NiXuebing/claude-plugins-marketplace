# Claude Plugins Marketplace

基于 Claude Code 插件系统构建的插件市场，为团队和社区提供集中式的插件发现、分发和管理平台。

## 特性

- **集中式发现**: 轻松浏览和发现可用插件
- **版本管理**: 自动跟踪和更新插件版本
- **多源支持**: 支持 GitHub、Git 仓库、本地路径等多种插件源
- **灵活安装**: 用户只安装需要的组件
- **企业友好**: 支持访问控制和插件白名单
- **社区驱动**: 鼓励社区贡献和共享

## 快速开始

### 1. 添加市场

在 Claude Code 中运行以下命令添加此市场：

```bash
/plugin marketplace add NiXuebing/claude-plugins-marketplace
```

### 2. 浏览插件

打开插件浏览界面：

```bash
/plugin
```

### 3. 安装插件

安装特定插件：

```bash
/plugin install <plugin-name>@community-plugins-hub
```

### 4. 更新插件

更新已安装的插件：

```bash
/plugin update <plugin-name>
```

## 市场结构

```
claude-plugins-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # 市场配置文件
├── plugins/                  # 插件目录
│   ├── example-plugin/
│   │   ├── commands/        # 斜杠命令
│   │   ├── agents/          # AI 代理
│   │   └── plugin.json      # 插件配置
│   └── ...
├── docs/
│   └── MARKETPLACE_GUIDE.md # 详细技术文档
├── CLAUDE.md                # Claude Code 项目指令
└── README.md                # 项目说明
```

## 可用插件

### 命令插件（Commands）
- 自定义斜杠命令，扩展 Claude Code 功能

### 代理插件（Agents）
- 专业领域 AI 助手
- 安全审计、文档生成、测试协调等

### 集成插件
- MCP Servers: Model Context Protocol 服务器
- LSP Servers: 语言服务器协议集成

## 文档

- [市场搭建完整指南](./docs/MARKETPLACE_GUIDE.md) - 详细的技术文档和最佳实践
- [官方文档](https://code.claude.com/docs/en/plugin-marketplaces) - Claude Code 插件市场官方文档

## 贡献插件

欢迎贡献新插件！请遵循以下步骤：

1. Fork 本仓库
2. 在 `plugins/` 目录下创建你的插件
3. 更新 `.claude-plugin/marketplace.json` 添加插件条目
4. 提交 Pull Request

详细的插件开发指南请参考 [MARKETPLACE_GUIDE.md](./docs/MARKETPLACE_GUIDE.md)。

## 本地测试

在提交前，可以在本地测试你的市场：

```bash
# 验证市场配置
/plugin validate .

# 添加本地市场
/plugin marketplace add ./path/to/claude-plugins-marketplace

# 测试安装插件
/plugin install test-plugin@community-plugins-hub
```

## 企业使用

在团队的 `.claude/settings.json` 中配置市场：

```json
{
  "extraKnownMarketplaces": {
    "company-plugins": {
      "source": {
        "source": "github",
        "repo": "NiXuebing/claude-plugins-marketplace"
      }
    }
  },
  "enabledPlugins": {
    "example-plugin@company-plugins": true
  }
}
```

## 许可证

MIT License

## 相关链接

- [Anthropic 插件公告](https://www.anthropic.com/news/claude-code-plugins)
- [社区市场示例](https://github.com/ananddtyagi/claude-code-marketplace)
- [Claude Code 官网](https://code.claude.com)

## 支持

如有问题或建议，请提交 [Issue](https://github.com/NiXuebing/claude-plugins-marketplace/issues)。
