# academic-peer-review Skill 安装说明

版本：1.0.0  
生成日期：2026-07-13  
适用范围：OpenClaw 及兼容 AgentSkills 文件夹规范的 Claw/Agent 运行时

## 1. Skill 功能

`academic-peer-review` 用于对以下学术稿件进行结构化同行评审：

- 原创研究、实验研究、观察性研究、建模与方法论文；
- 叙述性综述、系统综述、范围综述和证据综述；
- 含荟萃分析、元回归或其他定量证据综合的综述。

Skill 会自动识别稿件类型，选择对应检查表，并按“稿件位置—观察—影响—修改建议”组织审稿意见。它保留了两份原始 Prompt 的全部核心评审维度，并增加证据定位、问题分级、科研诚信审慎表述和标准化审稿报告模板。

## 2. 包内结构

解压后应看到以下结构：

```text
academic-peer-review/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── original-research.md
    ├── review-article.md
    └── review-report-template.md
```

安装时必须复制整个 `academic-peer-review` 文件夹。不要只复制 `SKILL.md`，否则两类检查表和输出模板无法加载。

Skill 本体只包含 Markdown 和 YAML，无脚本、可执行文件、二进制文件、密钥或强制联网依赖。`agents/openai.yaml` 是可选界面元数据；不支持它的运行时可以直接忽略，不影响核心功能。

## 3. OpenClaw 安装（推荐）

### 3.1 使用本地文件夹安装

OpenClaw 官方 CLI 从本地安装时要求源目录根部直接存在 `SKILL.md`。官方 CLI 不接受 ZIP 文件路径，因此请先解压安装包，再执行：

Windows PowerShell：

```powershell
openclaw skills install ".\academic-peer-review" --as academic-peer-review
```

macOS 或 Linux：

```bash
openclaw skills install ./academic-peer-review --as academic-peer-review
```

该命令默认安装到当前 OpenClaw 工作区的 `skills/` 目录。若要供本机所有 OpenClaw Agent 共用，在命令末尾加 `--global`：

```bash
openclaw skills install ./academic-peer-review --as academic-peer-review --global
```

### 3.2 手动复制

无法使用 CLI 时，把整个文件夹复制到以下任一位置：

- 当前工作区优先：`<workspace>/skills/academic-peer-review/`
- 项目级通用目录：`<workspace>/.agents/skills/academic-peer-review/`
- 用户级 AgentSkills 目录：`~/.agents/skills/academic-peer-review/`
- OpenClaw 全局目录：`~/.openclaw/skills/academic-peer-review/`

工作区目录优先级高于用户级或全局目录。同名 Skill 同时存在时，OpenClaw 使用优先级更高的副本。

官方参考：[OpenClaw Skills 系统](https://docs.openclaw.ai/tools/skills)；[OpenClaw skills CLI](https://docs.openclaw.ai/cli/skills)。

## 4. 其他 Claw / AgentSkills 兼容运行时

对于支持 AgentSkills 文件夹规范的其他运行时：

1. 找到该运行时配置的 Skill 根目录。
2. 把整个 `academic-peer-review` 文件夹复制进去。
3. 确认最终路径为 `<skill-root>/academic-peer-review/SKILL.md`，而不是多套一层目录。
4. 重新开始一个会话，或使用该运行时的“刷新 Skills / Reload”功能。

如果运行时支持额外 Skill 搜索目录，也可把本文件夹所在父目录加入其配置。核心兼容条件只有两个：能够读取带 YAML frontmatter 的 `SKILL.md`，并允许 Skill 从相对路径加载 `references/` 文件。

## 5. 安装验证

OpenClaw 可执行：

```bash
openclaw skills info academic-peer-review
openclaw skills check
```

然后新建会话，测试以下任一指令：

```text
使用 $academic-peer-review 对这篇原创研究论文进行完整同行评审，按关键问题和一般问题输出，并标注页码或章节依据。
```

```text
使用 $academic-peer-review 评审这篇系统综述，重点检查检索可重复性、偏倚风险、证据综合和统计分析。
```

正常行为应包括：先识别论文类型；原创研究加载 `original-research.md`；综述加载 `review-article.md`；完整报告按 `review-report-template.md` 组织。

## 6. 常用用法

快速评审：

```text
用 $academic-peer-review 快速评审这篇稿件，只给研究概述、最重要的 5 个问题和总体判断。
```

完整审稿：

```text
用 $academic-peer-review 做完整审稿。逐章节报告“符合、部分符合、不符合或无法判断”，每条关键问题都给出位置、影响和修改建议。
```

英文审稿：

```text
Use $academic-peer-review to prepare a journal-style peer-review report in English. Separate major and minor comments and ground each major comment in manuscript evidence.
```

作者回复清单：

```text
用 $academic-peer-review 审查稿件，并把所有意见转换成作者可以逐条回复的行动清单。
```

建议同时提供：论文全文、补充材料、目标期刊、文章类型、期刊审稿表或评分尺度。未提供这些材料时，Skill 会按通用学术标准评审，并明确无法判断的项目。

## 7. 更新与卸载

本地文件夹安装不会被 `openclaw skills update` 自动跟踪。更新时使用新版文件夹重新安装并加 `--force`，或手动替换旧文件夹：

```bash
openclaw skills install ./academic-peer-review --as academic-peer-review --force
```

卸载时删除对应 Skill 目录即可。删除前确认路径确实是 `academic-peer-review`，避免误删同一 Skill 根目录中的其他文件夹。

## 8. 故障排查

### 找不到 Skill

- 检查 `SKILL.md` 是否位于 `academic-peer-review` 文件夹根部。
- 检查是否错误形成 `academic-peer-review/academic-peer-review/SKILL.md`。
- 新建会话或刷新 Skill 快照。
- 若 Agent 配置了 Skill 白名单，确认 `academic-peer-review` 已获允许。

### 报告内容过于简略

在指令中明确要求“完整审稿”，并要求覆盖对应检查表全部栏目。快速评审模式默认只输出最重要的问题。

### 无法加载检查表

确认 `references/` 中三个 Markdown 文件均已复制，且文件名未改动。不要单独移动 `SKILL.md`。

### ZIP 安装报错

OpenClaw 的常规 `skills install` 不支持直接传入 ZIP 或其他归档路径。先解压，再把解压后的 Skill 目录传给安装命令。

### 无法准确评审

确认论文正文、图表和补充材料已作为当前会话可访问的文件提供。Skill 不会猜测未提供的内容，也不会自动制造缺失的页码、引用或数据。

## 9. 安全与隐私

该 Skill 不包含执行代码，也不会自行上传论文。论文是否会被发送到外部服务，取决于所使用 Claw/Agent 运行时、模型提供方和用户启用的工具。处理未公开稿件前，请遵守期刊保密要求、机构数据政策和作者授权范围。
