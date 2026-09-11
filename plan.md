# Personal Knowledge Base Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use `superpowers:executing-plans` to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 在当前 Obsidian Vault 中建立可直接使用的 Raw -> Source -> Wiki 个人知识库纯净骨架。

**Architecture:** 使用普通 Markdown、目录和 Obsidian Wikilink，不增加第三方依赖。Raw 保存不可变证据，Source 建立来源索引，Wiki 维护归一化的长期知识，Inbox 作为临时入口；空目录用 `.gitkeep` 保留。

**Tech Stack:** Obsidian、Markdown、YAML frontmatter、Wikilink、PowerShell、Git

**Spec:** 用户于 2026-09-11 在当前会话中确认的设计；完整规则将在执行时写入 `AGENTS.md`。

## Global Constraints

- 不创建示例知识、示例 Raw 或演示数据。
- 不修改、移动或删除已有 Raw、个人笔记及 Obsidian 插件文件。
- 目标文件如在执行前出现，必须先读取并停止请示，禁止覆盖。
- 不修改 `.obsidian/workspace.json` 和 `.obsidian/plugins/git-vault-sync/data.json`。
- 不新增插件或依赖；只配置已启用的 Templates 核心插件。
- Wiki 默认最多采用“主题/概念/页面”三级结构，复杂关系使用 Wikilink。
- 不确定内容不得标记为 `verified`。
- Git 使用单次参数 `-c safe.directory=D:/nll_vault/nll`，不修改全局配置。

## File Map

- Create: `AGENTS.md`、`inbox/README.md`。
- Create: `raw/{AI,旅游,育儿,电影,读书,笔记,面试,文档}/.gitkeep`。
- Create: `wiki/index.md`、`wiki/log.md`、`wiki/sources/README.md`。
- Create: `wiki/{AI,旅游,育儿,电影,读书}/index.md`。
- Create: `wiki/AI/{LLM,Agent,RAG,FPGA}/.gitkeep`。
- Create: `templates/Wiki.md`、`templates/Source.md`。
- Create: `.obsidian/templates.json`，设置模板目录为 `templates`。
- Create/Update: `process.txt`，仅在提交时记录本次变更。
- Preserve: `.obsidian/workspace.json`、`.obsidian/plugins/git-vault-sync/data.json`。

## Impact Assessment

- 新增内容限定在上述计划路径。
- 主题入口页只用于导航，不代表已有知识结论。
- `.gitkeep` 只让 Git 保留空目录，不承载知识内容。
- 不改变社区插件、插件数据或 Obsidian 工作区布局。

---

### Task 1: 执行前保护与分支准备

**Files:** Inspect repository; create branch `feat/initialize-knowledge-base`

- [ ] **Step 1: 检查 Git 状态**

```powershell
git -c safe.directory=D:/nll_vault/nll status --short --branch
```

Expected: 当前为 `main`；保留执行前已有的两个 `.obsidian` 未跟踪文件。

- [ ] **Step 2: 检查目标冲突**

```powershell
$targets = @('AGENTS.md','inbox/README.md','wiki/index.md','wiki/log.md','wiki/sources/README.md','templates/Wiki.md','templates/Source.md','.obsidian/templates.json','process.txt'); $targets | Where-Object { Test-Path -LiteralPath $_ }
```

Expected: 除已确认的 `plan.md` 外无目标文件。若有输出，读取文件并停止执行，禁止覆盖。

- [ ] **Step 3: 创建功能分支**

```powershell
git -c safe.directory=D:/nll_vault/nll switch -c feat/initialize-knowledge-base
```

Expected: 切换到 `feat/initialize-knowledge-base`。

### Task 2: 创建 Raw、Inbox 与治理规则

**Files:** Create `AGENTS.md`, `inbox/README.md`, Raw directories and `.gitkeep`

- [ ] **Step 1: 创建目录**

使用 `New-Item -ItemType Directory` 创建 File Map 中声明的目录；只创建缺失目录，不移动现有内容。

- [ ] **Step 2: 写入完整 `AGENTS.md`**

原样保存用户提供的 41 节知识库规则，确保包含 Raw 不可变、Canonical Wiki、查询顺序、冲突处理、知识状态、健康检查和标准工作流。

- [ ] **Step 3: 写入 Inbox 说明**

`inbox/README.md` 明确 Inbox 是临时入口，并记录：

```text
新资料 -> 判断类型和主题 -> 搜索 Wiki -> 归档 Raw -> 创建 Source
       -> 更新或创建 Canonical Wiki -> 必要时更新 index -> 追加 log
```

- [ ] **Step 4: 创建 Raw 占位文件**

为八个 Raw 主题目录创建零字节 `.gitkeep`。不得写入摘要或示例资料。

- [ ] **Step 5: 验证规则与 Raw**

```powershell
rg -n "Raw 是不可变的证据层|新资料处理前必须搜索 Wiki|Canonical Wiki" AGENTS.md
Get-ChildItem -LiteralPath raw -Recurse -File -Force | Where-Object { $_.Name -ne '.gitkeep' }
```

Expected: 三类核心规则均可定位；第二条命令无输出。

### Task 3: 创建 Wiki 导航、Source 说明与日志

**Files:** Create Wiki indexes, `wiki/sources/README.md`, `wiki/log.md`

- [ ] **Step 1: 创建总入口**

`wiki/index.md` 使用 `# Knowledge Wiki`，只加入已存在的主题入口：

```markdown
- [[AI/index|AI]]
- [[旅游/index|旅游]]
- [[育儿/index|育儿]]
- [[电影/index|电影]]
- [[读书/index|读书]]
```

- [ ] **Step 2: 创建主题入口**

五个主题 `index.md` 只包含主题标题、用途和“重要知识入口”。AI 入口列出 LLM、Agent、RAG、FPGA 的目录名称，但不创建指向不存在页面的链接。

- [ ] **Step 3: 创建 Source 说明**

`wiki/sources/README.md` 明确：每份正式 Raw 建立对应 Source；不得编造作者、日期、URL 或内容；Source 回链 Raw，Wiki 在“来源”区引用 Source。

- [ ] **Step 4: 初始化追加式日志**

`wiki/log.md` 创建 `2026-09-11` 条目，记录骨架创建、`Conflicts: 无`、`Need human confirmation: 无`；以后只追加，不修改历史。

- [ ] **Step 5: 验证导航**

确认 `wiki/index.md` 的五个 Wikilink 分别对应现有的 `wiki/<主题>/index.md`，初始化不得产生 Broken link。

### Task 4: 创建模板并配置 Obsidian

**Files:** Create `templates/Wiki.md`, `templates/Source.md`, `.obsidian/templates.json`

- [ ] **Step 1: 创建 Wiki 模板**

模板 frontmatter 使用 `title`、`topics`、`aliases`、`status: supported`；正文使用“是什么、核心概念、工作原理、实践方法、相关知识、来源”章节。

- [ ] **Step 2: 创建 Source 模板**

模板 frontmatter 使用 `title`、`source_type`、`author`、`date`、`url`、`topics`；正文使用“摘要、关键内容、关联 Wiki、Raw”章节。无法确认的元数据保持为空。

- [ ] **Step 3: 配置 Templates 核心插件**

创建 `.obsidian/templates.json`：

```json
{
  "folder": "templates"
}
```

不修改 `.obsidian/core-plugins.json`，执行前已确认 Templates 核心插件启用。

- [ ] **Step 4: 验证模板配置**

```powershell
Get-Content -LiteralPath '.obsidian/templates.json' -Raw | ConvertFrom-Json | Select-Object folder
rg -n "^status: supported$|^## 来源$" templates/Wiki.md
rg -n "^source_type:$|^## Raw$" templates/Source.md
```

Expected: `folder` 为 `templates`，模板字段和章节均存在。
