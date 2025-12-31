# Claude Code 项目维护指令

本项目是一个 Claude Code 插件市场仓库。在处理此项目时，请遵循以下指令。

## 项目概述

这是一个基于 Claude Code 插件系统的市场仓库，用于分发和管理插件。核心文件是 `.claude-plugin/marketplace.json`。

## 目录结构

```
claude-plugins-marketplace/
├── .claude-plugin/
│   └── marketplace.json      # 市场配置（核心文件）
├── plugins/                  # 插件目录
│   └── [plugin-name]/
│       ├── commands/         # 斜杠命令
│       ├── agents/           # AI 代理
│       ├── hooks/            # 钩子脚本
│       └── plugin.json       # 插件配置
├── docs/
│   └── MARKETPLACE_GUIDE.md  # 技术文档
├── CLAUDE.md                 # 本文件（项目指令）
└── README.md                 # 项目说明
```

## 开发规范

### 1. 命名约定

- **插件名称**: 使用 kebab-case（例如：`code-formatter`, `security-scanner`）
- **文件名**: 小写字母和连字符
- **目录名**: 与插件名称保持一致

### 2. marketplace.json 维护

当添加或修改插件时：

1. **必需字段验证**:
   - `name`: 确保使用 kebab-case，无空格
   - `source`: 验证路径或 URL 可访问

2. **可选字段建议**:
   - 始终添加 `description` 和 `version`
   - 为插件添加 `author` 和 `license`
   - 使用 `tags` 或 `keywords` 提高可发现性

3. **路径处理**:
   - 相对路径始终以 `./` 开头
   - 使用 `metadata.pluginRoot` 避免重复路径前缀
   - 不要使用 `..` 进行路径遍历

### 3. 插件开发

添加新插件时：

1. 在 `plugins/` 下创建插件目录
2. 创建必要的组件文件（commands、agents 等）
3. 添加 `plugin.json`（如果需要独立配置）
4. 更新 `.claude-plugin/marketplace.json`
5. 在 `README.md` 中更新插件列表

### 4. 版本管理

- 使用语义化版本（Semver）: `major.minor.patch`
- 在 marketplace.json 和插件自身同步版本号
- 提交时说明版本变更和破坏性更新

## 测试流程

在提交前执行以下测试：

```bash
# 1. 验证 marketplace.json 格式
/plugin validate .

# 2. 本地测试市场
/plugin marketplace add ./

# 3. 测试插件安装
/plugin install [plugin-name]@claude-plugins-marketplace

# 4. 验证插件功能
# 根据插件类型测试对应功能（命令、代理等）
```

## Git 提交规范

使用约定式提交（Conventional Commits）:

- `feat:` 新增插件或功能
- `fix:` 修复问题
- `docs:` 文档更新
- `refactor:` 重构代码
- `test:` 测试相关
- `chore:` 维护任务

示例：
```
feat: 添加代码格式化插件
fix: 修复 marketplace.json 路径错误
docs: 更新插件使用说明
```

## 常见任务

### 添加新插件

1. 创建插件目录和文件
2. 编辑 `.claude-plugin/marketplace.json` 添加条目
3. 验证配置: `/plugin validate .`
4. 更新 README.md 插件列表
5. 提交变更

### 更新插件

1. 修改插件文件
2. 更新版本号（plugin.json 和 marketplace.json）
3. 验证配置
4. 记录变更日志
5. 提交变更

### 删除插件

1. 从 `.claude-plugin/marketplace.json` 移除条目
2. 删除或归档插件目录
3. 更新 README.md
4. 提交变更

## 文件路径引用

在插件中引用文件时：

- 使用 `${CLAUDE_PLUGIN_ROOT}` 变量引用插件安装目录
- 确保所有引用的文件在插件边界内
- 避免引用插件目录外的文件（会导致缓存问题）

## 企业配置

维护团队配置示例时，使用以下模板：

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
    "plugin-name@company-plugins": true
  }
}
```

## 保留名称检查

避免使用以下保留名称：
- `claude-code-marketplace`
- `claude-code-plugins`
- `anthropic-marketplace`
- `anthropic-plugins`
- `agent-skills`
- `life-sciences`

## 参考文档

- 详细技术文档: `docs/MARKETPLACE_GUIDE.md`
- 官方文档: https://code.claude.com/docs/en/plugin-marketplaces
- 社区示例: https://github.com/ananddtyagi/claude-code-marketplace

## 注意事项

1. **不要**在未验证的情况下修改 `.claude-plugin/marketplace.json`
2. **务必**保持 JSON 格式正确（使用验证工具）
3. **确保**所有插件路径在提交前测试过
4. **记录**每次插件版本变更的原因
5. **维护**清晰的文档和示例
