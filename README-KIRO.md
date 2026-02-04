# Kiro IDE Configuration

This repository has been converted to work with **Kiro IDE** while maintaining compatibility with Claude Code.

## What's Been Converted

### ✅ Fully Functional (59% Complete)

- **12 Agents** - All converted and ready to use
- **19 Steering Files** - Core guidance and patterns
- **7 Commands** - Available as slash commands
- **Project Structure** - Organized in `.kiro/` directory

### 🔄 Remaining Work

- 16 additional commands to convert
- MCP server configuration
- Hooks redesign (requires manual work)

## Quick Start

### 1. Install Kiro IDE
Download and install from official source

### 2. Open Repository
```
File → Open Folder → Select this directory
```

### 3. Test It Works
```
In Kiro chat:
"Use the planner agent to create a plan for adding a feature"
```

**See:** `KIRO-QUICK-START.md` for 5-minute setup guide

## Directory Structure

```
.kiro/
├── agents/           # 12 agent configurations (JSON)
│   ├── planner.json
│   ├── tdd-guide.json
│   ├── code-reviewer.json
│   └── ... (9 more)
├── steering/         # 19+ guidance files (Markdown)
│   ├── coding-standards.md
│   ├── tdd-workflow.md
│   ├── cmd-plan.md (slash command)
│   └── ... (16 more)
└── settings/         # MCP and other configs (to be added)
```

## Available Agents

| Agent | Purpose | Model |
|-------|---------|-------|
| **planner** | Implementation planning | Opus 4 |
| **architect** | System design | Opus 4 |
| **tdd-guide** | Test-driven development | Opus 4 |
| **code-reviewer** | Code review | Sonnet 4 |
| **security-reviewer** | Security analysis | Opus 4 |
| **build-error-resolver** | Fix build errors | Opus 4 |
| **e2e-runner** | E2E testing | Opus 4 |
| **refactor-cleaner** | Dead code cleanup | Opus 4 |
| **doc-updater** | Documentation | Opus 4 |
| **database-reviewer** | Database optimization | Opus 4 |
| **go-reviewer** | Go code review | Opus 4 |
| **go-build-resolver** | Go build fixes | Opus 4 |

## Available Commands (Slash Commands)

| Command | Description |
|---------|-------------|
| `/plan` | Create implementation plan |
| `/code-review` | Review uncommitted changes |
| `/verify` | Run comprehensive checks |
| `/build-fix` | Fix build errors incrementally |
| `/test-coverage` | Analyze and improve coverage |
| `/update-docs` | Sync documentation |

**Note:** Slash command syntax may vary in Kiro. Try mentioning commands naturally if `/command` doesn't work.

## Steering Files (Always Loaded)

These provide automatic guidance:

- **Coding Standards** - Immutability, naming, patterns
- **TDD Workflow** - Test-driven development process
- **Security Guidelines** - Security checks and best practices
- **Git Workflow** - Commit messages, PR process
- **Backend Patterns** - API design, caching, error handling
- **Frontend Patterns** - React, hooks, performance
- **Go Patterns** - Idiomatic Go, concurrency
- **Postgres Patterns** - Database optimization, RLS
- **Testing Requirements** - 80% coverage, test types
- **Performance Optimization** - Model selection, context management

## How to Use

### Using Agents

```
In Kiro chat:
"Use the [agent-name] agent to [task]"

Example:
"Use the planner agent to plan adding authentication"
```

### Using Commands

```
In Kiro chat:
"/[command-name]" or "Run the [command-name] command"

Example:
"/plan" or "Run the plan command"
```

### Automatic Guidance

Steering files are loaded automatically. Just ask questions:

```
"What are the coding standards?"
"How should I structure tests?"
"What's the security checklist?"
```

## Testing Your Setup

### Quick Test (2 minutes)
```bash
# 1. Check files exist
dir .kiro\agents\*.json    # Should show 12 files
dir .kiro\steering\*.md    # Should show 19+ files

# 2. Test in Kiro
# Open Kiro, ask: "Use the planner agent to create a simple plan"
```

### Full Test (30 minutes)
See: `KIRO-TESTING-GUIDE.md`

## Conversion Details

### Key Differences from Claude Code

| Aspect | Claude Code | Kiro IDE |
|--------|-------------|----------|
| Agent Format | Markdown with YAML | JSON + Markdown |
| Tool Names | Read, Write, Edit | fs_read, fs_write, fs_edit |
| Model Names | opus, sonnet | claude-opus-4, claude-sonnet-4 |
| Skills/Rules | Separate directories | Combined in steering/ |
| Commands | commands/*.md | steering/cmd-*.md (manual) |

### Tool Mapping

| Claude Code | Kiro IDE |
|-------------|----------|
| Read | fs_read |
| Write | fs_write |
| Edit | fs_edit |
| Bash | execute_bash |
| Grep | grep |
| Glob | glob |

### Model Mapping

| Claude Code | Kiro IDE |
|-------------|----------|
| opus | claude-opus-4 |
| sonnet | claude-sonnet-4 |
| haiku | claude-haiku-4 |

## Troubleshooting

### Agents Not Loading
```bash
# Check JSON syntax
node -e "JSON.parse(require('fs').readFileSync('.kiro/agents/planner.json'))"

# Verify files exist
dir agents\*.md
```

### Steering Files Not Referenced
```bash
# Check files exist
dir .kiro\steering\*.md

# Restart Kiro IDE
```

### File Reference Errors
```bash
# Verify relative paths are correct
type .kiro\agents\planner.json | findstr "file://"

# Should show: "file://../agents/planner.md"
```

## Documentation

- **Quick Start:** `KIRO-QUICK-START.md` - 5-minute setup
- **Testing Guide:** `KIRO-TESTING-GUIDE.md` - Comprehensive testing
- **Conversion Status:** `KIRO-CONVERSION-STATUS.md` - Progress tracking

## Compatibility

This repository works with:
- ✅ **Kiro IDE** - Using `.kiro/` configuration
- ✅ **Claude Code** - Using original `agents/`, `skills/`, `rules/` directories

Both can coexist in the same repository!

## Contributing

When adding new agents or steering files:

1. **For Agents:**
   - Add markdown to `agents/[name].md`
   - Add JSON config to `.kiro/agents/[name].json`
   - Reference markdown via `file://../agents/[name].md`

2. **For Steering Files:**
   - Add markdown to `.kiro/steering/[name].md`
   - Use `inclusion: manual` front-matter for commands
   - No front-matter for always-included guidance

3. **Test:**
   - Validate JSON syntax
   - Test in Kiro IDE
   - Update documentation

## Support

- **Issues:** Check `KIRO-TESTING-GUIDE.md` troubleshooting section
- **Status:** See `KIRO-CONVERSION-STATUS.md` for known issues
- **Original Config:** Compare with `agents/`, `skills/`, `rules/` directories

## License

Same as original repository

---

**Ready to use with Kiro IDE!** 🚀

Start with: `KIRO-QUICK-START.md`
