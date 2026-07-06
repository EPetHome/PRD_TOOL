# AI PRD Suite

> 一个 AI 产品从"想法"到"可交付"的三模块工作流 —— 作为 Claude Code Skill 运行，也支持任何 AI 工具直接使用。

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-V1.0-green.svg)]()

---

## 为什么需要这个套件

一个 AI 产品需要**三类产出**，面向**三类完全不同的受众**：

| 受众 | 需要什么 | 本套件的产出 |
|------|---------|------------|
| 🧑‍💻 工程师（前端/后端/算法） | 可点击、可交互的原型 | **模块 1** · 单文件 HTML 原型 |
| 👥 所有人类（PM/运营/法务/客户） | 完整、可读的产品说明 | **模块 2** · 叙事性 Markdown PRD |
| 🤖 AI 工具（Cursor/Claude Code/v0） | 结构化、可执行的构建指令 | **模块 3** · AI 构建包 |

三类产出**必须物理分开**才能各自做到位。额外还有一份**综合评审页**，把原型 + PRD 放在同一页，用于评审会 / 汇报 / 跨团队对齐。

---

## 三模块架构

```
┌─────────────────────────────────────────────────────┐
│                   Module 2                          │
│              人类可读 PRD (Markdown)                  │
│         8 章叙事 · 第 8 章是下游转译输入                  │
└──────────┬────────────────────┬─────────────────────┘
           │                    │
           ▼                    ▼
┌──────────────────┐  ┌──────────────────────────────┐
│    Module 1       │  │        Module 3               │
│  HTML 交互原型     │  │     AI 构建包 (Markdown)        │
│ 左控制台+右手机     │  │  9 章结构化指令 · 直接喂 AI      │
│ 双击即可打开       │  │  → Cursor/Claude Code/v0      │
└────────┬─────────┘  └──────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────┐
│            Module 1+2 综合评审页                      │
│          三栏：控制台 | 手机原型 | PRD                 │
│          评审 / 汇报 / 跨团队对齐使用                    │
└─────────────────────────────────────────────────────┘
```

---

## 文件结构

```
PRD_TOOL/
├── SKILL.md                          # Claude Code Skill 入口（任务路由）
├── agents/
│   └── openai.yaml                   # Agent 配置
├── references/
│   ├── readme-quickstart.md          # 三分钟上手指南（PM 必读）
│   ├── module-2-doc-template.md      # 模块 2 文档填空模板
│   ├── module-3-output-spec.md       # 模块 3 AI 构建包产出物规范
│   ├── module-3-conversion-prompt.md # 模块 3 转换 Prompt（可直接复制粘贴）
│   ├── module-1-prototype-scaffold.md# 模块 1 原型脚手架 Prompt
│   └── module-1-2-review-page-scaffold.md # 综合评审页脚手架 Prompt
└── README.md                         # 本文件
```

---

## 安装为 Claude Code Skill

### 方式一：直接克隆到全局 Skills 目录

```bash
git clone https://github.com/EPetHome/PRD_TOOL.git ~/.claude/skills/ai-prd-suite
```

### 方式二：Symbolic link

```bash
git clone https://github.com/EPetHome/PRD_TOOL.git /your/path/PRD_TOOL
ln -s /your/path/PRD_TOOL ~/.claude/skills/ai-prd-suite
```

安装后，在 Claude Code 中直接说需求即可触发，例如：

- "帮我生成这个产品的 PRD 文档"
- "把这份模块 2 文档转成 AI 构建包"
- "验证这个 AI 构建包能不能直接交给 Cursor"

---

## PM 工作流（6 步 · 1 天内完成）

### 第 1 步 · 想清楚产品（0.5-1h）

回答三个问题再动笔：

- 产品给谁用？解决什么问题？
- 核心功能 3-5 个是哪些？
- 商业形态是什么（MVP / 内测 / 规模化 / 成熟期）？

> ⚠️ **想不清楚 → 停下找利益相关方对齐，不要硬填。**

### 第 2 步 · 填写模块 2 文档（2-4h）

拷贝 `references/module-2-doc-template.md` 到项目目录，按模板填空。

**关键：第 8 章"下游转译输入"必须填** —— 模块 1 原型和模块 3 AI 构建包都从这一章抽取数据。

### 第 3 步 · 人工评审（0.5-1h）

让**至少一位非技术同事**通读，确认"能完整复述产品做什么"。通过后模块 2 进入交付状态。

### 第 4 步 · 生成模块 3 AI 构建包（0.5h）

用 Claude Code 直接说"帮我生成模块 3 构建包"，或手动复制 `references/module-3-conversion-prompt.md` 中的 Prompt 到任意 AI 工具。

### 第 5 步 · 生成模块 1 原型（0.5h）

用 Claude Code 直接说"帮我生成模块 1 原型"，或手动复制 `references/module-1-prototype-scaffold.md` 中的 Prompt 到任意 AI 工具。

产出：一个**双击即可打开**的单文件 HTML，左侧控制台 + 右侧 iPhone 手机原型。

### 第 6 步 · 生成综合评审页（0.5h）

用 Claude Code 直接说"帮我生成综合评审页"，或手动复制 `references/module-1-2-review-page-scaffold.md` 中的 Prompt。

产出：三栏桌面页 —— 左切换台 | 中 iPhone 原型 | 右 PRD 正文 + 目录。

---

## 使用示例

### 在 Claude Code 中自然语言触发

```
你：帮我审查这份模块 2 文档是否完整

Claude Code：(读取 module-2-doc-template.md)
→ 检查章节 1-8 是否齐全
→ 检查第 8 章下游转译输入是否填写
→ 检查是否有不该出现的技术规范
→ 输出缺失项和阻断项清单
```

```
你：把这份模块 2 文档转成 AI 构建包

Claude Code：(读取 module-3-conversion-prompt.md + module-3-output-spec.md)
→ 生成 [[产品名]]-AI构建包.md
→ 自动运行可执行性验证
→ 输出是否可交 Cursor/Claude Code/v0 开工
```

```
你：帮我做全链一致性校验

Claude Code：(读取全部模块产物)
→ 检查模块 2 第 8 章 → 模块 1 和模块 3 是否对齐
→ 检查各文档间的 scope/角色/状态/合规/技术选型是否矛盾
→ 输出阻断项 → 风险项 → 建议下一步
```

---

## 交付清单

| 交付物 | 文件名 | 给谁 |
|--------|--------|------|
| 模块 2 文档 | `[[产品名]]-模块 2-文档.md` | 所有人类 |
| 模块 3 构建包 | `[[产品名]]-AI构建包.md` | AI 构建工具 |
| 模块 1 原型 | `[[产品名]]-原型.html` | 算法/前端/后端 |
| 综合评审页 | `[[产品名]]-综合评审页.html` | 评审会/汇报/跨团队 |

---

## 什么时候该停下找人

| 卡住的节点 | 该找谁 |
|-----------|--------|
| 产品定位没想清楚 | 产品负责人或业务方 |
| 模块 2 评审不通过 | 回第 2 步修改，别忽略反馈 |
| 模块 3 一直有 `[待产品方补充]` | 模块 2 第 8 章有缺口，回填 |
| 模块 1 原型 AI 怎么都做不对 | 模块 2 核心流程没讲清楚，回填 |
| 法规不确定（模块 2 第 7 章） | **找法务，不要自己拍脑袋** |

---

## 设计原则

1. **关注点严格分离** —— 模块 1/2/3 物理独立，各司其职
2. **不发明产品决策** —— 缺失项标记 `[待产品方补充]`，不编造
3. **AI 可执行即合格** —— 模块 3 的唯一验收标准：能否直接交给 Cursor/Claude Code/v0 开工
4. **可追溯** —— 模块 3 中的每项技术决策都能映射回模块 2 的对应章节

---

## License

MIT

---

**一句话：走完 6 步 → 三模块交付齐，再多一份综合评审页用于展示。**
