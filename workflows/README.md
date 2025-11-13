# ACP Workflows

Out-of-the-box workflows for the Ambient Code Platform (ACP).

## 🎓 Learning Path

New to workflows? Follow this progression:

```
1. hello-world    → Your first 5 minutes
2. template       → Learn all the features
3. bugfix         → See a real automation workflow
4. debugging      → See a complex multi-step workflow
5. Build your own! 🚀
```

---

## 📚 Available Workflows

### [hello-world](hello-world/) - Start Here! ⭐

**The absolute simplest workflow** - perfect for your first 5 minutes.

- **What it does**: Says hello and creates a greeting file
- **Commands**: `/hello`
- **Size**: ~40 lines total
- **Learn**: Basic workflow structure

**Try it first!** Once you understand this, everything else makes sense.

---

### [template](template/) - Reference Guide

**Complete reference** showing all workflow features and patterns.

- **What it does**: Demonstrates every available field and pattern
- **Commands**: `/template.start`, `/template.next`
- **Size**: Comprehensive documentation
- **Learn**: All configuration options, design patterns, best practices

**Use when:** Creating your own workflow and need to know what's possible.

---

### [bugfix](bugfix/) - Automation Example

**Real-world automated workflow** - bug to PR with minimal human interaction.

- **What it does**: Analyzes bugs, implements fixes, creates PRs automatically
- **Commands**: `/bugfix.start`, `/bugfix.fix`, `/bugfix.verify`, `/bugfix.submit`
- **Size**: Production-ready automation
- **Learn**: Agent usage, TDD workflow, Git automation

**Use when:** You want to see how workflows handle real engineering tasks.

---

### [debugging](debugging/) - Complex Workflow

**Comprehensive incident debugging** - the engineering equivalent of "plan a feature".

- **What it does**: Systematic incident investigation, RCA, fix implementation, runbook generation
- **Commands**: `/debug.start`, `/debug.analyze`, `/debug.trace`, `/debug.diagnose`, `/debug.fix`, `/debug.document`
- **Size**: Multi-agent, observability-integrated
- **Learn**: Complex workflows, SRE practices, multi-step processes

**Use when:** You want to see how workflows orchestrate multiple agents for complex tasks.

---

## 🚀 Quick Start

### Never created a workflow? Try this:

```bash
# 1. Try hello-world (5 minutes)
cd workflows/hello-world
cat README.md
# Activate in ACP and run: /hello

# 2. Read template tutorial (15 minutes)
cd workflows/template
cat README.md
# Follow the "Coffee Order" tutorial

# 3. Copy template and build your own!
cp -r workflows/template workflows/my-awesome-workflow
# Edit and customize
```

---

## 📖 Understanding Workflows

### What is a workflow?

A workflow is a **specialized assistant** for Claude Code that:
- Defines a role (debugger, code reviewer, feature planner)
- Provides slash commands (`/start`, `/fix`, `/analyze`)
- Organizes outputs in predictable locations
- Guides users through multi-step processes

### Anatomy of a workflow

**Minimum viable workflow (2 files):**
```
workflows/my-workflow/
├── .ambient/ambient.json     # Config: name, description, prompts
└── .claude/commands/
    └── start.md              # At least one command
```

**That's it!** Just configuration + commands.

### Visual Guide

```
User activates workflow
        ↓
Claude loads configuration
        ↓
User runs /command
        ↓
Claude follows command instructions
        ↓
Outputs appear in artifacts/
```

**Key insight**: Workflow logic (in `workflows/`) and outputs (in `artifacts/`) are separate!

---

## 🛠️ Creating Your Own Workflow

### Option 1: Start from hello-world (Easiest)

```bash
cp -r workflows/hello-world workflows/my-workflow
cd workflows/my-workflow
# Edit ambient.json - change name and description
# Edit .claude/commands/hello.md - change what it does
# Rename: mv .claude/commands/hello.md .claude/commands/mycommand.md
```

### Option 2: Start from template (Feature-rich)

```bash
cp -r workflows/template workflows/my-workflow
cd workflows/my-workflow
# Follow template README for customization
```

### Option 3: From scratch (Learning mode)

```bash
mkdir -p workflows/my-workflow/.ambient
mkdir -p workflows/my-workflow/.claude/commands

# Create ambient.json (see template for structure)
# Create your-command.md (see hello-world for simplicity)
```

---

## 📊 Workflow Comparison

| Workflow | Complexity | Use Case | Commands | Agents | Best For |
|----------|-----------|----------|----------|---------|----------|
| **hello-world** | ⭐ Minimal | Learning | 1 | 0 | First workflow |
| **template** | ⭐⭐ Reference | Documentation | 2 | 1 | Creating new workflows |
| **bugfix** | ⭐⭐⭐ Real | Bug fixing | 4 | 3 | Automated bug fixes |
| **debugging** | ⭐⭐⭐⭐ Complex | Incident response | 6 | 4 | Production incidents |

---

## 🎯 Choose Your Adventure

**I want to...**

### Learn how workflows work
→ Start with **[hello-world](hello-world/)**

### See all available features
→ Read **[template](template/)**

### Build automated bug fixes
→ Study **[bugfix](bugfix/)**

### Handle production incidents
→ Explore **[debugging](debugging/)**

### Create my own workflow
→ Copy **[hello-world](hello-world/)** or **[template](template/)**

---

## 📝 Workflow Best Practices

### Do's ✅

- **Start simple** - Begin with 1-2 commands, add more as needed
- **Clear names** - Use descriptive command names (`/analyze`, not `/do`)
- **Guide users** - Tell them what to do next at each step
- **Validate input** - Check user input before doing work
- **Create docs** - Add a README explaining your workflow

### Don'ts ❌

- **Don't overcomplicate** - If one command works, don't force multiple
- **Don't skip validation** - Always check inputs
- **Don't mix concerns** - Each command should do one thing well
- **Don't forget outputs** - Map results in ambient.json
- **Don't skip testing** - Try all commands with various inputs

---

## 🤔 Common Questions

### Q: How many commands should my workflow have?

**A:** As few as possible! Start with one, add more only if needed.
- **hello-world**: 1 command (perfect for simple tasks)
- **bugfix**: 4 commands (logical workflow stages)
- **debugging**: 6 commands (complex investigation process)

### Q: Do I need agents?

**A:** Usually no! Agents are optional.
- **Use agents**: For complex tasks (code exploration, RCA, TDD)
- **Skip agents**: For simple workflows (hello-world doesn't use any)

### Q: Where do outputs go?

**A:** Always in `artifacts/`, never in `workflows/`.
- `workflows/` = Workflow logic (versioned in git)
- `artifacts/` = Outputs (gitignored, generated fresh each time)

### Q: Can I have multiple workflows?

**A:** Yes! Users can switch between workflows.
- Each workflow is independent
- Activate the one you need for your task
- Switch workflows as needed

---

## 🔧 Debugging Your Workflow

**Command not found?**
- Check file name: `.claude/commands/mycommand.md`
- Ensure workflow is activated
- Verify directory is in `workflows/`

**Outputs not appearing?**
- Check `results` paths in ambient.json
- Ensure paths are relative to `artifacts/`
- Verify glob patterns are correct

**Claude not following instructions?**
- Check `systemPrompt` is clear
- Ensure command markdown is well-structured
- Test with simple inputs first

---

## 📚 Additional Resources

- **Main ACP docs**: [Link to main documentation]
- **Workflow spec**: See template/README.md for complete reference
- **Example commands**: Browse `.claude/commands/` in each workflow

---

## 🤝 Contributing

Created a useful workflow? Share it!

1. Add it to `workflows/your-workflow/`
2. Include a clear README
3. Add examples in the command files
4. Update this index

---

## 🎉 You're Ready!

Pick a workflow above and dive in. Remember:

1. **Start with hello-world** (5 minutes)
2. **Read template tutorial** (15 minutes)
3. **Build your own** (fun!)

Questions? Check the workflow READMEs or ask for help!

Happy workflow building! 🚀
