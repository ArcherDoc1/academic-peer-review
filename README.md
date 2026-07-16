# academic-peer-review

一个面向原创研究论文、叙述性综述、系统综述、范围综述和荟萃分析的结构化同行评审 Skill。

它会先识别稿件类型，再按相应检查表生成基于稿件证据、分级明确且可执行的审稿意见。每条关键意见尽量包含“位置—观察—影响—修改建议”。

## 功能

- 原创研究：检查摘要、引言、方法、统计、结果、讨论、参考文献、伦理与可重复性。
- 综述论文：检查综述必要性、证据覆盖、检索与筛选透明度、偏倚风险、结果综合、报告规范与语言质量。
- 荟萃分析：额外检查效应量、模型选择、异质性、敏感性分析、发表偏倚与结论边界。
- 支持快速评审、完整审稿、逐章节检查、英文审稿与作者回复清单。

## 安装

### OpenClaw

```bash
openclaw skills install git:ArcherDoc1/academic-peer-review
```

也可以下载本仓库 ZIP，解压后安装本地文件夹：

```bash
openclaw skills install ./academic-peer-review --as academic-peer-review
```

详细步骤见 [安装说明](academic-peer-review-Skill安装说明.md)。

## 使用

上传论文全文、图表和补充材料后，在会话中输入：

```text
使用 $academic-peer-review 对这篇论文进行完整同行评审。请先识别稿件类型，再按关键问题、一般问题和分章节检查输出；每条关键问题都请注明位置、影响和可执行的修改建议。
```

### 常用示例

**原创研究论文**

```text
使用 $academic-peer-review 评审这篇原创研究论文。重点检查研究设计、样本量与抽样、控制变量、统计方法、效应量和置信区间、图表与正文一致性，以及结论是否超出数据支持范围。
```

**系统综述或范围综述**

```text
使用 $academic-peer-review 评审这篇系统综述。重点检查检索数据库和检索式、纳入排除标准、筛选流程、偏倚风险、数据提取、证据综合，以及报告规范。
```

**英文期刊审稿**

```text
Use $academic-peer-review to write a concise journal-style peer-review report in English. Separate major and minor comments, cite manuscript sections or page numbers, and give actionable recommendations.
```

完整用法见 [使用说明](academic-peer-review-使用说明.md)。

## 文件结构

```text
.
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── original-research.md
│   ├── review-article.md
│   └── review-report-template.md
├── academic-peer-review-Skill安装说明.md
└── academic-peer-review-使用说明.md
```

## 使用边界

Skill 只依据当前可访问的稿件和材料进行评审；不会猜测未提供的数据、结果、页码或参考文献。它可辅助同行评审，但不能替代领域专家、统计学专家、伦理委员会或期刊编辑的最终判断。