# AI PRD Suite

一套把产品 PRD 拆成三个独立模块的工作流模板，可作为 Claude Code Skill 安装，也可以把 Prompt 复制到其他 AI 工具里用。

---

## 三模块说明

| 模块 | 受众 | 形态 | 说明 |
|------|------|------|------|
| 模块 1 · 原型 | 工程师（前端/后端/算法） | 单文件 HTML | 左控制台 + 右 iPhone 原型，双击浏览器打开 |
| 模块 2 · 文档 | 所有人类（PM/运营/法务/客户） | Markdown | 8 章叙事 PRD，第 8 章供模块 1 和模块 3 抽取 |
| 模块 3 · AI 构建包 | AI 工具（Cursor/Claude Code/v0） | Markdown | 9 章结构化构建指令，校验后可喂给 AI 开工 |

额外提供 **模块 1+2 综合评审页**：把原型和 PRD 拼成一个三栏 HTML，评审会用。

模块之间的关系：

```
模块 2（PRD 文档）
  ├── 供给 → 模块 1（HTML 原型）
  ├── 供给 → 模块 3（AI 构建包）
  └── 1+2 合并 → 综合评审页
```

---

## 文件结构

```
├── SKILL.md                            # Claude Code Skill 定义
├── agents/openai.yaml                  # Agent 配置
├── references/
│   ├── readme-quickstart.md            # 上手说明
│   ├── module-2-doc-template.md        # 模块 2 文档模板
│   ├── module-3-output-spec.md         # 模块 3 产出物规范
│   ├── module-3-conversion-prompt.md   # 模块 2→3 转换 Prompt
│   ├── module-1-prototype-scaffold.md  # 模块 1 原型生成 Prompt
│   └── module-1-2-review-page-scaffold.md  # 综合评审页生成 Prompt
└── README.md
```

---

## 安装为 Claude Code Skill

```bash
git clone https://github.com/EPetHome/PRD_TOOL.git ~/.claude/skills/ai-prd-suite
```

或者 symlink：

```bash
git clone https://github.com/EPetHome/PRD_TOOL.git /your/path/PRD_TOOL
ln -s /your/path/PRD_TOOL ~/.claude/skills/ai-prd-suite
```

安装后在 Claude Code 中说需求即可触发，比如"审查这份模块 2 文档""把模块 2 转成 AI 构建包""验证模块 3 能不能直接开工"。

不用 Claude Code 的话，直接复制 `references/` 下的 Prompt 贴到 ChatGPT / Cursor / v0 也能用。

---

## 使用步骤

### 1. 想清楚产品

动笔前先确认三个问题：给谁用、核心功能 3-5 个是什么、商业形态（MVP/内测/规模化/成熟期）。

### 2. 填写模块 2 文档

复制 `references/module-2-doc-template.md` 到项目目录，按模板填空。第 8 章"下游转译输入"必须填。

### 3. 人工评审模块 2

找一位非技术同事通读，确认能完整复述产品做什么。通过后模块 2 进入交付状态。

### 4. 生成模块 3 AI 构建包

把模块 2 文档 + `references/module-3-conversion-prompt.md` 喂给 AI。产出 `[[产品名]]-AI构建包.md`。生成后按同文件中的"可执行性验证 Prompt"检查一遍。

### 5. 生成模块 1 原型

把模块 2 文档 + `references/module-1-prototype-scaffold.md` 喂给 AI。产出 `[[产品名]]-原型.html`，双击浏览器打开验收。

### 6. 生成综合评审页

把模块 2 文档 + 模块 1 HTML 全文 + `references/module-1-2-review-page-scaffold.md` 喂给 AI。产出 `[[产品名]]-综合评审页.html`。

---

## 交付物

| 交付物 | 文件名 | 给谁 |
|--------|--------|------|
| 模块 2 文档 | `[[产品名]]-模块 2-文档.md` | 所有人类 |
| 模块 3 构建包 | `[[产品名]]-AI构建包.md` | AI 构建工具 |
| 模块 1 原型 | `[[产品名]]-原型.html` | 算法/前端/后端 |
| 综合评审页 | `[[产品名]]-综合评审页.html` | 评审会/汇报 |

---

## Skill 支持的任务

SKILL.md 定义了 6 个路由：

| 用户意图 | 读取的参考文件 | 产出 |
|---------|-------------|------|
| 填写/审查模块 2 | module-2-doc-template.md | 审查报告或填好的 PRD |
| 生成模块 3 | module-3-conversion-prompt.md + module-3-output-spec.md | AI 构建包 + 可执行性验证 |
| 验证模块 3 可执行性 | module-3-conversion-prompt.md（验证段） | 阻断项清单 + 可执行性矩阵 |
| 生成模块 1 原型 | module-1-prototype-scaffold.md | 单文件 HTML 原型 |
| 生成综合评审页 | module-1-2-review-page-scaffold.md | 三栏评审 HTML |
| 全链一致性校验 | 全部产物 | 阻断项 → 风险项 → 建议 |

---

## 什么时候该停下找人

| 卡住的地方 | 该找谁 |
|-----------|--------|
| 产品定位没想清楚 | 产品负责人或业务方，对齐再动笔 |
| 模块 2 评审不通过 | 按反馈改，不要强推 |
| 模块 3 有 `[待产品方补充]` 消不掉 | 模块 2 第 8 章有缺口，回填 |
| 模块 1 原型 AI 一直做不对 | 模块 2 核心流程描述不清楚，回第 2 步补 |
| 法规不确定 | 找法务，不要自己判断 |

---

## License

MIT
