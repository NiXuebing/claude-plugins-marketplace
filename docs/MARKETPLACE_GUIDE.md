# Claude Code 插件市场技术文档

## 概述

Claude Code 插件市场是一个用于在团队和社区中分发 Claude Code 扩展的系统。市场提供集中式发现、版本跟踪、自动更新，并支持多种源类型（Git 仓库、本地路径等）。

## 核心架构

### 1. 市场文件结构

插件市场使用位于仓库根目录的 `.claude-plugin/marketplace.json` 文件来定义市场目录。

**必需字段：**
- `name`: 市场标识符（kebab-case 格式，无空格）
- `owner`: 维护者信息，包含必需的 `name` 字段
- `plugins`: 可用插件数组

**可选字段：**
- `metadata.description`: 市场描述
- `metadata.version`: 市场版本
- `metadata.pluginRoot`: 基础目录，会添加到相对插件源路径前

### 2. 插件条目配置

每个插件条目必须包含：

**必需：**
- `name`: 插件标识符（kebab-case）
- `source`: 获取插件的位置（字符串路径或对象）

**可选：**
- `description`: 插件简要描述
- `version`: 插件版本
- `author`: 作者信息
- `homepage`: 文档 URL
- `repository`: 源代码 URL
- `license`: SPDX 许可证标识符
- `keywords`/`tags`: 搜索元数据
- `strict`: 布尔值，控制插件是否需要自己的 `plugin.json`

## 插件源类型

### 相对路径
```json
{
  "name": "my-plugin",
  "source": "./plugins/my-plugin"
}
```

### GitHub 仓库
```json
{
  "name": "github-plugin",
  "source": {
    "source": "github",
    "repo": "owner/plugin-repo"
  }
}
```

### Git 仓库
```json
{
  "name": "git-plugin",
  "source": {
    "source": "url",
    "url": "https://gitlab.com/team/plugin.git"
  }
}
```

## 搭建市场步骤

### 基础市场创建

1. 创建包含 `.claude-plugin/marketplace.json` 的目录结构
2. 定义带有源引用的插件条目
3. 托管在 GitHub 或 Git 服务上
4. 用户通过 `/plugin marketplace add owner/repo` 添加市场

### 组件配置

插件支持以下组件的自定义路径：

- **Commands（命令）**: `"commands": ["./commands/core/", "./commands/experimental/"]`
- **Agents（代理）**: `"agents": ["./agents/security.md"]`
- **Hooks（钩子）**: 自定义钩子配置
- **MCP Servers**: 服务器配置
- **LSP Servers**: 语言服务器配置

使用 `${CLAUDE_PLUGIN_ROOT}` 变量来引用插件安装目录中的文件。

## marketplace.json 完整示例

```json
{
  "name": "my-company-marketplace",
  "owner": {
    "name": "Your Company Name"
  },
  "metadata": {
    "description": "Internal plugin marketplace for development tools",
    "version": "1.0.0",
    "pluginRoot": "./plugins"
  },
  "plugins": [
    {
      "name": "code-formatter",
      "description": "Automatic code formatting tool",
      "version": "1.2.0",
      "author": "Dev Team",
      "source": "./code-formatter",
      "license": "MIT",
      "tags": ["formatting", "code-quality"]
    },
    {
      "name": "security-scanner",
      "description": "Security vulnerability scanner",
      "version": "2.0.1",
      "source": {
        "source": "github",
        "repo": "your-org/security-plugin"
      },
      "homepage": "https://docs.yourcompany.com/security-scanner",
      "keywords": ["security", "scanning"]
    }
  ]
}
```

## 分发与测试

### 本地测试
```bash
/plugin marketplace add ./my-local-marketplace
/plugin install test-plugin@my-marketplace
```

### 验证
```bash
/plugin validate .
```

### 团队集成

在 `.claude/settings.json` 中添加市场：

```json
{
  "extraKnownMarketplaces": {
    "company-tools": {
      "source": {
        "source": "github",
        "repo": "your-org/claude-plugins"
      }
    }
  },
  "enabledPlugins": {
    "code-formatter@company-tools": true
  }
}
```

## 企业限制

组织可以通过托管设置中的 `strictKnownMarketplaces` 限制插件源：

**禁用所有添加：**
```json
{
  "strictKnownMarketplaces": []
}
```

**仅允许特定源：**
```json
{
  "strictKnownMarketplaces": [
    {
      "source": "github",
      "repo": "acme-corp/approved-plugins"
    }
  ]
}
```

## 插件类型

### 1. Commands（命令）
自定义斜杠命令，为 Claude Code 添加特定功能。

示例：`/lyra`, `/audit`, `/ultrathink`

### 2. Agents（代理）
具有领域专业知识的专门 AI 助手。

示例：安全审计代理、文档生成代理、测试协调器

### 3. Hooks（钩子）
在特定事件时执行的 shell 命令。

### 4. MCP Servers
Model Context Protocol 服务器集成。

### 5. LSP Servers
语言服务器协议集成。

## 最佳实践

### 1. 目录组织
```
your-marketplace/
├── .claude-plugin/
│   └── marketplace.json
├── plugins/
│   ├── plugin-a/
│   │   ├── commands/
│   │   ├── agents/
│   │   └── plugin.json
│   └── plugin-b/
│       ├── commands/
│       └── plugin.json
├── scripts/
│   └── sync.sh
└── README.md
```

### 2. 插件独立性
- 每个插件应独立安装，无需不必要的依赖
- 使用清晰的命名约定（kebab-case）
- 提供详细的描述和标签以便发现

### 3. 版本管理
- 使用语义化版本（Semver）
- 在 marketplace.json 和插件自身中跟踪版本
- 记录变更和破坏性更新

### 4. 文档
- 为每个插件提供 README
- 包含使用示例
- 记录配置选项
- 提供故障排除指南

## 常见问题排查

### 市场未加载
- 验证 `.claude-plugin/marketplace.json` 存在且可访问
- 使用 `/plugin validate .` 验证 JSON 语法
- 检查私有仓库的访问权限

### 常见验证错误
- 重复的插件名称
- 使用 `..` 的路径遍历尝试
- 缺少必需字段（`name`, `source`）
- 无效的 JSON 格式

### 插件安装失败
- 验证源 URL 可访问
- 确认插件目录包含所需文件
- 对于 GitHub 源，确保公开访问或正确身份验证
- 检查引用的文件在插件边界内存在

### 文件解析问题
插件在安装后会被缓存，因此对插件目录外文件的引用会失败。解决方案：
- 使用符号链接
- 重构目录以将共享文件放在插件源路径中

## 保留的市场名称

以下名称被保留用于官方用途：
- `claude-code-marketplace`
- `claude-code-plugins`
- `anthropic-marketplace`
- `anthropic-plugins`
- `agent-skills`
- `life-sciences`
- 其他冒充官方市场的名称

## 社区市场示例

### cc-marketplace（ananddtyagi）
- 社区驱动的插件仓库
- 从实时数据库自动同步
- 包含文档生成器、Lyra（提示优化）、代码库分析等插件
- GitHub: `ananddtyagi/cc-marketplace`

## 使用工作流

### 添加市场
```bash
/plugin marketplace add user-or-org/repo-name
```

### 浏览插件
```bash
/plugin
```

### 安装插件
```bash
/plugin install plugin-name@marketplace-name
```

### 更新插件
```bash
/plugin update plugin-name
```

## 发布时间线

- **2025年10月9日**: Anthropic 推出插件支持
- **2025年9月29日**: Claude Code 2.0 发布
- **2025年**: Claude Code 2.0.13 引入插件市场系统

## 技术优势

1. **集中式发现**: 用户可以轻松浏览和发现插件
2. **版本跟踪**: 自动跟踪和更新插件版本
3. **灵活的源**: 支持 GitHub、Git、本地路径等多种源
4. **细粒度控制**: 用户只安装需要的组件
5. **企业友好**: 支持访问限制和批准的插件列表
6. **社区驱动**: 鼓励社区贡献和共享

## 参考资源

- [官方文档](https://code.claude.com/docs/en/plugin-marketplaces)
- [Anthropic 官方公告](https://www.anthropic.com/news/claude-code-plugins)
- [社区市场示例](https://github.com/ananddtyagi/claude-code-marketplace)
- [Claude Code 插件指南](https://composio.dev/blog/claude-code-plugin)

## 下一步

要开始构建你的插件市场：

1. 创建 `.claude-plugin/marketplace.json` 文件
2. 组织你的插件到 `plugins/` 目录
3. 在 GitHub 上发布仓库
4. 测试本地安装和验证
5. 分享给团队或社区使用
