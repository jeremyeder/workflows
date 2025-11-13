# Hello World Workflow

The **absolute simplest** ACP workflow. Start here if you're new!

## What It Does

Greets you and creates a file. That's it!

## Try It

```bash
/hello
/hello Alice
/hello Bob
```

## The Entire Workflow

This workflow has just **3 files**:

```
workflows/hello-world/
├── .ambient/
│   └── ambient.json          # 7 lines: name, description, prompts, results
└── .claude/
    └── commands/
        └── hello.md          # 30 lines: one simple command
```

**Total: ~40 lines of code** for a complete workflow!

## Understanding the Files

### 1. ambient.json (Required)

This file tells ACP about your workflow:

```json
{
  "name": "Hello World",                    // What shows in UI
  "description": "The simplest...",         // Brief explanation
  "systemPrompt": "You are a...",          // Instructions for Claude
  "startupPrompt": "👋 Hi! Run /hello",    // Greeting message
  "results": {                              // Where outputs go
    "Greeting": "artifacts/greetings/*.md"
  }
}
```

### 2. hello.md (Your Command)

This file defines what `/hello` does:

- **Frontmatter**: Describes the command
- **Steps**: What the command should do
- **Implementation**: The actual logic

That's it! Just these two pieces.

## Build Your Own

Want to create your own workflow? Here's how:

### Option 1: Copy This (Easiest)

```bash
cp -r workflows/hello-world workflows/my-workflow
cd workflows/my-workflow

# Edit .ambient/ambient.json - change the name
# Edit .claude/commands/hello.md - change what it does
# Rename: mv .claude/commands/hello.md .claude/commands/mycommand.md
```

### Option 2: From Scratch

```bash
# Create structure
mkdir -p workflows/my-workflow/.ambient
mkdir -p workflows/my-workflow/.claude/commands

# Create ambient.json (use hello-world as template)
# Create your-command.md (use hello.md as template)
```

## Next Steps

Once you've tried Hello World:

1. **hello-world** (you are here) - Simplest possible
2. **template** - Shows all available features
3. **bugfix** - Real-world automated workflow
4. **debugging** - Complex multi-step workflow

Start simple, build up! 🚀
