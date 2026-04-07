# PM-Skills

Claude Code 技能与 Agent 管理仓库，为产品经理和软件架构师提供可复用的提示模板和专业助手。

## 仓库结构

```
PM-Skills/
├── skills/                      # PM 技能集（17个）+ 专项技能
│   ├── pm-alpha-*/             # Alpha: 日常工作流（5个）
│   ├── pm-beta-*/              # Beta: 专项能力（5个）
│   ├── pm-gamma-*/             # Gamma: 医疗信息化（3个）
│   ├── pm-delta-*/             # Delta: 协作与成长（4个）
│   ├── weixin-writer/         # 微信公众号文章写作
│   └── wechat-mp-writer-skill/ # 旧版微信写作（legacy）
├── agents/                      # Claude Code Agents（4个）
├── plugins/                     # Claude Code 插件
└── PACS_RIS_PM_Daily_Workflow.md  # PACS/RIS PM 工作流文档
```

## Skills（技能）

### Alpha - 日常工作流

| 技能 | 说明 |
|------|------|
| `pm-alpha-requirement-analysis` | 需求分析四步法 |
| `pm-alpha-prd-writer` | PRD 文档撰写 |
| `pm-alpha-meeting-notes` | 会议纪要生成 |
| `pm-alpha-data-review` | 数据复盘 |
| `pm-alpha-task-prioritization` | 任务优先级排序 |

### Beta - 专项能力

| 技能 | 说明 |
|------|------|
| `pm-beta-competitive-analysis` | 竞品分析 |
| `pm-beta-user-research` | 用户研究 |
| `pm-beta-data-analysis` | 数据分析 |
| `pm-beta-product-planning` | 产品规划 |
| `pm-beta-prototype-review` | 原型评审 |

### Gamma - 医疗信息化

| 技能 | 说明 |
|------|------|
| `pm-gamma-pacs-ris-daily` | PACS/RIS 日常工作流 |
| `pm-gamma-medical-compliance` | 医疗合规管理 |
| `pm-gamma-healthcare-integration` | 医疗系统集成 |

### Delta - 协作与成长

| 技能 | 说明 |
|------|------|
| `pm-delta-presentation` | 演讲汇报 |
| `pm-delta-cross-team` | 跨部门沟通 |
| `pm-delta-upward-management` | 向上管理 |
| `pm-delta-career-growth` | 职业成长 |

### 专项技能

| 技能 | 说明 |
|------|------|
| `weixin-writer` | 微信公众号文章写作（包含内容方法论） |

## Agents（助手）

| Agent | 说明 |
|--------|------|
| `product-manager` | 产品经理助手 |
| `code-reviewer` | 代码审查专家 |
| `pacs-expert` | PACS/RIS 医疗影像专家 |
| `competitive-analyst` | 竞品分析专家 |

## Plugins（插件）

### architect-plugin

软件架构师工具包，包含 5 个架构技能和 1 个架构师 Agent：

- `architecture-design` — 系统架构设计
- `code-review` — 代码架构评审
- `technical-decision` — 技术选型决策
- `architecture-assessment` — 架构质量评估
- `api-design` — API 设计规范

## 使用方式

在 Claude Code 中通过以下方式调用：

```
# Agents
@product-manager, @code-reviewer, @pacs-expert, @competitive-analyst

# Skills
/architecture-design, /code-review, /pm-alpha-requirement-analysis, etc.

# 微信文章写作
/weixin-writer
```

## 贡献

创建新的 Skill 或 Agent：

1. 在对应目录下创建文件
2. 遵循现有格式（YAML frontmatter + 内容）
3. 包含 `_meta.json` 元数据文件
4. 可选：添加 `references/` 子目录存放参考资料

## 相关文档

- `PACS_RIS_PM_Daily_Workflow.md` — PACS/RIS 产品经理每日工作全流程
