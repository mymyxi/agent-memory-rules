# AI Agent 记忆系统配置规则

> 一套经过实战验证的 AI Agent 持久化记忆管理方法论

## 🎯 这个项目是什么？

这是一套完整的 AI Agent 记忆系统配置规则，帮助你的 AI 助手建立持久化记忆能力。通过结构化的记忆分类、标准化的存储格式和清晰的使用原则，让 AI 能够跨会话记住重要信息，持续优化协作体验。

**适用平台：** Claude Code / Codex / Kimi Work / Workbuddy / 其他支持文件系统的 Agent

## ✨ 核心特性

- **🗂️ 五层分类体系**：Core（不变基础信息）/ Projects（项目状态）/ Ideas（点子库）/ Diary（每日流水）/ Weekly（周报归档）
- **📝 四种记忆类型**：user（用户画像）/ feedback（协作反馈）/ project（项目状态）/ reference（外部资源引用）
- **🎨 情绪标签**（可选）：灵感来自《头脑特工队》，用情绪给记忆打优先级标签——乐乐/忧忧/怒怒/厌厌/怕怕/焦焦 + 乐乐·勇转化态
- **🔍 智能检索机制**：通过 MEMORY.md 索引快速定位 + 情绪触发的主动调取
- **⏰ 时效性管理**：记忆带时间戳，过期信息自动提示验证
- **🧹 遗忘清理机制**：四级强度分级（S+/S/A/B），自动清理过期记忆
- **🛡️ 隐私保护**：明确什么该记、什么不该记，避免敏感信息泄露

## 🚀 快速开始

### 1. 创建记忆目录结构

根据你使用的 Agent 平台选择对应路径：

**Claude Code:**
```bash
mkdir -p ~/.claude/projects/-Users-yourname/memory/{core,projects,ideas,diary,weekly}
cd ~/.claude/projects/-Users-yourname/memory
```

**Workbuddy / Kimi Work / 其他:**
```bash
mkdir -p ~/agent-memory/{core,projects,ideas,diary,weekly}
cd ~/agent-memory
```

### 2. 复制核心配置文件

将 `skill/memory-system.md` 的内容添加到你的 Agent 系统配置中（系统提示词/instructions/配置文件）。

### 3.（可选）添加情绪标签层

如果需要更智能的记忆优先级管理，将 `skill/emotion-system.md` 也添加到系统配置中。这会让你的 Agent 具备情绪标记、主动调取相关记忆、自动清理过期记忆的能力。

### 3. 创建记忆索引文件

```bash
touch MEMORY.md
```

在 `MEMORY.md` 中按分类组织记忆索引：

```markdown
# Memory Index

## Core（不变的基础信息）
- [协作风格偏好](core/feedback_behavior.md) — 沟通方式、响应详略、工具选择原则

## Projects（项目状态）
- [项目A进展](projects/project_a_status.md) — 当前阶段、技术选型、下一步计划

## Ideas（点子库）
- [产品想法池](ideas/product_ideas.md) — 待验证的产品方向

## Diary（每日流水）
- [2026-07-27 今日记忆](diary/diary-2026-07-27.md) — 今日工作内容和关键决策

## Weekly（周报归档）
- [2026-W30 周报](weekly/week-2026-30.md) — 本周完成任务汇总
```

### 4. 使用模板创建记忆

从 `templates/` 目录复制对应类型的模板，填写内容。

## 📋 一键接入 — 把这段话发给你的 Agent

直接复制粘贴给你的 Agent（Codex / Claude Code / WorkBuddy / Kimi Work）：

```
使用 agent-memory-rules 框架管理你的持久化记忆。

项目地址：https://github.com/mymyxi/agent-memory-rules

核心文件：
- skill/memory-system.md — 基础记忆规则（五层分类、四种记忆类型）
- skill/emotion-system.md — 情绪记忆系统 v1.1（情绪标签 + 自动清理）

你需要做的事：
1. 读取 skill/memory-system.md，按五层分类创建 .workbuddy/memory/ 目录
2. 读取 skill/emotion-system.md，用情绪标签给每次会话的内容分级存储
3. 存记忆铁律：
   - 发生即存储（默认 C 级，30天自动清理）
   - 有情绪标签 → 按 S+/S/A/B 级决定留存时间
   - 教训/反模式 → 写进 MEMORY.md
   - 可复用方法 → 存为 skill
   - 纯闲聊/打招呼 → 不存
4. 怕怕亮 → 暂停操作先确认
5. 同类情绪累积 3 次 → 回流更新身份文件
```

就这一句话。你的 Agent 会自己从仓库读取配置。

## 📖 使用指南

### 什么时候应该创建记忆？

#### user 类型
- 了解到用户的角色、技能、偏好时
- 用户明确表达协作方式期望时
- 需要长期记住的个人背景信息

#### feedback 类型
- 用户纠正你的做法时（"不要这样做"）
- 用户确认某种做法有效时（"就这样继续"）
- 发现可复用的协作模式时

#### project 类型
- 启动新项目时
- 项目状态发生重要变化时
- 需要跨会话跟踪的工作进展

#### reference 类型
- 用户提到外部系统的重要信息时
- 需要记录文档/API/资源位置时

### 什么不应该存入记忆？

❌ **代码实现细节**（应该通过读取当前代码获取）  
❌ **Git历史信息**（应该通过 `git log` 获取）  
❌ **临时任务状态**（当前会话结束即失效）  
❌ **已有文档中的信息**（避免与 Agent 配置文件/说明文档等重复）

## 🔧 高级用法

### 记忆时效性验证

记忆文件会标注创建/更新时间。当 AI 引用超过一定时间的记忆时，会提示验证：

```
<system-reminder>This memory is 30 days old. Verify against current state before asserting as fact.</system-reminder>
```

### 记忆更新策略

- 发现记忆过时 → 更新记忆文件的 frontmatter 和内容
- 发现记忆错误 → 直接修正，不保留错误版本
- 发现记忆重复 → 合并到更完整的那个

### 记忆检索优化

MEMORY.md 前200行会自动加载到上下文，因此：
- 高频使用的记忆放前面
- 每条索引保持在150字符内
- 用清晰的描述帮助 AI 判断相关性

## 📁 项目结构

```
agent-memory-rules/
├── README.md              # 英文说明
├── README_CN.md           # 中文说明（本文件）
├── skill/
│   ├── memory-system.md   # 核心记忆配置规则（添加到系统提示词）
│   └── emotion-system.md  # 可选：情绪标签层
├── templates/             # 记忆模板
│   ├── user_template.md
│   ├── feedback_template.md
│   ├── project_template.md
│   └── reference_template.md
└── examples/              # 虚构示例
    ├── MEMORY.md
    └── memories/
        ├── core/
        ├── projects/
        └── ideas/
```

## 🤝 贡献指南

欢迎提交 Issue 和 Pull Request！

如果你有更好的记忆分类方法、发现规则中的问题、或者想分享你的使用经验，请随时参与贡献。

## 📄 开源协议

MIT License

## 🙏 致谢

这套记忆系统在实际项目中迭代超过6个月，感谢所有参与测试和反馈的用户。

---

**开始使用：** 从 `skill/memory-system.md` 复制配置到你的 Agent 系统配置  
**进阶增强：** 添加 `skill/emotion-system.md` 获得情绪标签能力  
**遇到问题：** 查看 `examples/` 目录的完整示例  
**深入了解：** 阅读 `skill/memory-system.md` 中的详细说明
