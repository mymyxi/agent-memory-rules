# AI Agent Memory System

> A battle-tested methodology for persistent memory management in AI Agents

[中文文档](README_CN.md)

## 🎯 What is this?

A complete configuration ruleset for AI Agent memory systems that enables persistent memory capabilities across conversations. Through structured memory classification, standardized storage formats, and clear usage principles, your AI assistant can remember important information and continuously optimize collaboration.

**Supported Platforms:** Claude Code / Codex / Kimi Work / Workbuddy / Any file-system enabled Agent

## ✨ Key Features

- **🗂️ Five-layer Classification**: Core / Projects / Ideas / Diary / Weekly
- **📝 Four Memory Types**: user / feedback / project / reference
- **🎨 Emotion Tagging** *(optional)*: Inside Out-inspired emotion tags for memory prioritization — Joy, Sadness, Anger, Disgust, Fear, Anxiety + Joy·Brave transformation
- **🔍 Smart Retrieval**: Quick access via MEMORY.md index + proactive emotion-triggered retrieval
- **⏰ Staleness Management**: Timestamps with automatic verification prompts
- **🧹 Forgetting Mechanism**: Tiered retention (S+/S/A/B) with automatic cleanup
- **🛡️ Privacy Protection**: Clear guidelines on what to save and what to avoid

## 🚀 Quick Start

### 1. Create Memory Directory Structure

Choose the path based on your Agent platform:

**Claude Code:**
```bash
mkdir -p ~/.claude/projects/-Users-yourname/memory/{core,projects,ideas,diary,weekly}
cd ~/.claude/projects/-Users-yourname/memory
```

**Workbuddy / Kimi Work / Others:**
```bash
mkdir -p ~/agent-memory/{core,projects,ideas,diary,weekly}
cd ~/agent-memory
```

### 2. Copy Core Configuration

Add the content from `skill/memory-system.md` to your Agent system configuration (system prompt/instructions/config file).

### 3. (Optional) Add Emotion Layer

For smarter memory prioritization, also add `skill/emotion-system.md`. This gives your agent the ability to tag memories with emotions, proactively retrieve relevant past experiences, and auto-clean stale memories. See [Emotion-Memory System](docs/emotion-system.md) for details.

### 3. Create Memory Index

```bash
touch MEMORY.md
```

Organize your memory index by category in `MEMORY.md`:

```markdown
# Memory Index

## Core
- [Collaboration preferences](core/feedback_behavior.md) — Communication style, detail level, tool choices

## Projects
- [Project A status](projects/project_a_status.md) — Current stage, tech stack, next steps

## Ideas
- [Product ideas](ideas/product_ideas.md) — Product directions to explore

## Diary
- [2026-07-27 Daily log](diary/diary-2026-07-27.md) — Today's work and decisions

## Weekly
- [2026-W30 Weekly](weekly/week-2026-30.md) — Week summary
```

### 4. Use Templates

Copy templates from `templates/` directory and fill in your content.

## 📖 Usage Guide

### When to Create Memories?

**user type**: User role, skills, preferences, background  
**feedback type**: Corrections or confirmations of collaboration approaches  
**project type**: Project status, decisions, progress tracking  
**reference type**: External system locations and resources

### What NOT to Save?

❌ Code implementation details (read from current codebase)  
❌ Git history (use `git log`)  
❌ Temporary task status (expires with session)  
❌ Information already in documentation

## 📁 Project Structure

```
agent-memory-rules/
├── README.md              # English documentation
├── README_CN.md           # Chinese documentation
├── skill/
│   ├── memory-system.md   # Core memory configuration (add to system prompt)
│   └── emotion-system.md  # Optional emotion-tagging layer
├── templates/             # Memory templates
│   ├── user_template.md
│   ├── feedback_template.md
│   ├── project_template.md
│   └── reference_template.md
└── examples/              # Example memories (fictional)
    ├── MEMORY.md
    └── memories/
```

## 🤝 Contributing

Issues and Pull Requests welcome!

Share your improvements, report issues, or contribute usage experiences.

## 📄 License

MIT License

## 🙏 Acknowledgments

This memory system has been refined over 6+ months in real projects. Thanks to all users who provided feedback.

---

**Get Started:** Copy `skill/memory-system.md` to your Agent system configuration  
**Level Up:** Add `skill/emotion-system.md` for emotion-based memory prioritization  
**Need Help:** Check `examples/` for complete examples  
**Learn More:** Read detailed documentation in `skill/memory-system.md`
