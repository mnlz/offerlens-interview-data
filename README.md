# OfferLens Interview Data

[![Agent Skill](https://img.shields.io/badge/Agent-Skill-1769e0)](SKILL.md)
[![Data](https://img.shields.io/badge/Data-Real%20Interviews-0f766e)](#真实调用示例)
[![License](https://img.shields.io/badge/License-MIT-black.svg)](LICENSE)

**把近期真实面经接入 Codex、Claude Code、Cursor 等本地 Agent。**

按公司、岗位方向、关键词和时间范围查询完整面经原文，让 Agent 基于真实样本总结高频八股、项目追问和算法题，并开展更贴近目标岗位的模拟面试。

[产品网站](https://skill.mnls.cloud/) · [安装文档](install.md) · [官方岗位 Skill](https://github.com/mnlz/offerlens-jobs)

## 为什么需要这个 Skill

常见的面试准备流程主要依赖简历或通用知识，难以回答两个关键问题：目标公司最近在问什么，目标岗位实际看重什么。

`offerlens-interview-data` 为本地 Agent 补充近期真实面经原文。Agent 可以基于明确的公司、岗位和时间范围读取样本，再完成频次统计、问题归类、准备清单和模拟面试。

## 快速安装

### 方式一：让 Agent 安装

将下面这句话发送给支持 Skill 的 Agent：

```text
帮我安装 Skill：https://github.com/mnlz/offerlens-interview-data/blob/master/install.md
```

### 方式二：命令行安装

安装到当前机器检测到的全部 Agent：

```bash
npx skills add mnlz/offerlens-interview-data --all
```

只安装到 Codex：

```bash
npx skills add mnlz/offerlens-interview-data --agent codex --yes
```

Claude Code、Cursor、OpenCode、OpenClaw、Gemini CLI 和 GitHub Copilot 的命令见[中文安装文档](install.md)。

## 直接开始使用

无需学习命令，向 Agent 描述目标即可：

```text
帮我总结字节最近 AI Agent 岗位最常问的八股，并标出频次和样本范围
```

```text
统计美团后端最近常见的算法题，按题型整理准备清单
```

```text
结合腾讯真实 JD、近期面经和我的简历，预测项目深挖方向
```

```text
参考目标岗位的真实面经，进行一场包含八股、项目和算法题的模拟面试
```

## 核心能力

| 能力 | 说明 |
| --- | --- |
| 真实面经原文 | 返回可供 Agent 阅读和引用的完整面经文本 |
| 精确筛选 | 支持公司、岗位、岗位族、关键词和日期范围 |
| 近期样本 | 默认关注近 90 天，可按 90 → 180 → 365 天自适应扩大窗口 |
| 频次总结 | 基于实际样本归并同义问题，并说明时间范围与样本量 |
| 专项准备 | 支持八股、项目深挖、算法题、面试流程和模拟面试 |
| 本地组合 | JD、面经与简历由本地 Agent 组合分析，API 不接收简历 |

## 真实调用示例

以下为 **2026-08-22** 对字节 AI Agent 方向面经进行分析的一次真实调用快照：

```text
匹配面经：100 篇
可分析样本：93 篇

高频主题：
1. RAG 与检索增强        45 篇（48%）
2. 上下文管理与压缩      28 篇（30%）
3. 工具调用、MCP、Skills 27 篇（29%）
4. Agent 架构与工作流    24 篇（26%）
5. 记忆系统              23 篇（25%）
```

频次由 Agent 基于返回原文在本地统计。不同筛选条件和数据更新时间会产生不同结果，使用时应同时保留时间窗口、样本量和统计口径。

## 推荐工作流

`offerlens-interview-data` 可独立使用，也可以与 [offerlens-jobs](https://github.com/mnlz/offerlens-jobs) 配合：

```text
offerlens-jobs            → 当前官方校招 JD
offerlens-interview-data  → 近期真实面经原文
本地 Agent                → 读取用户简历并完成分析
                              ↓
          选岗、题目总结、项目追问、算法准备、模拟面试
```

没有安装岗位 Skill 时，可以直接把目标 JD 提供给 Agent。

## 免费额度与完整访问

首次安装无需配置 API Key，客户端会自动使用免费访问：

- 同一 IP 最多 10 次成功且非空的请求；
- 每次最多返回 10 篇完整面经；
- 空结果和失败请求不消耗额度。

完整访问通过环境变量 `OFFERLENS_API_KEY` 配置，取消 10 次免费额度限制，支持分页，单次最多返回 100 篇：

```bash
export OFFERLENS_API_KEY="你的 API Key"
```

请勿把 API Key 写入提示词、代码仓库或命令参数。其他系统的配置方法见[安装文档](install.md#配置完整访问-key)。

## 命令行使用

```bash
python3 scripts/query_interviews.py \
  --company 字节跳动 \
  --keyword "AI Agent" \
  --adaptive-recent \
  --limit 10 \
  --format markdown
```

常用筛选项包括：

- `--company`：公司；
- `--post`：岗位名称；
- `--role-group` / `--role-family`：岗位方向；
- `--keyword`：内容关键词；
- `--date-from` / `--date-to`：日期范围；
- `--days` / `--adaptive-recent`：近期窗口；
- `--limit` / `--offset`：数量与分页。

完整参数见 [references/api.md](references/api.md)，分析方式见 [references/workflows.md](references/workflows.md)。

## 数据与隐私边界

- 查询客户端只向 OfferLens API 发送筛选条件，不发送简历内容。
- 简历解析、岗位匹配和面试编排由用户设备上的 Agent 完成。
- API 返回面经原文，但不返回内部来源 ID 或原始来源链接。
- 频次、趋势和结论应由 Agent 根据实际返回样本计算，不应脱离样本推断。

## 项目结构

```text
offerlens-interview-data/
├── SKILL.md                    # Agent 行为与证据边界
├── install.md                  # 中文安装与 API Key 配置
├── scripts/
│   └── query_interviews.py     # 查询客户端
└── references/
    ├── api.md                  # 参数与响应说明
    └── workflows.md            # 推荐分析工作流
```

## 常见问题

<details>
<summary>Skill 会读取或上传我的简历吗？</summary>

API 不接收简历。若你让 Agent 结合简历分析，简历内容只由本地 Agent 处理。
</details>

<details>
<summary>为什么同一个问题的频次会变化？</summary>

面经数据每日更新，筛选条件、时间窗口、去重方式和主题归并口径也会影响统计结果。输出时应保留这些信息。
</details>

<details>
<summary>免费额度用完后怎么办？</summary>

API 会返回购买提示。获得完整访问 Key 后，将其配置到 `OFFERLENS_API_KEY`，无需修改 Skill 文件。
</details>

## License

[MIT](LICENSE)
