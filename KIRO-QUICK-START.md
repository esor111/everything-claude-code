# Kiro IDE Quick Start Guide

Get started testing the Kiro conversion in 5 minutes.

## Quick Setup (5 minutes)

### 1. Install Kiro IDE
```bash
# Download and install Kiro IDE from official source
# https://kiro.ai or your distribution method
```

### 2. Open This Repository
```
1. Launch Kiro IDE
2. File → Open Folder
3. Select: everything-claude-code directory
4. Kiro will auto-detect .kiro/ configuration
```

### 3. Verify Configuration Loaded
```
In Kiro IDE:
- Look for ".kiro" indicator in status bar or sidebar
- Check that agents are available
- Steering files should load automatically
```

## Quick Tests (10 minutes)

### Test 1: Agent Works ✅
```
In Kiro chat, type:
"Use the planner agent to create a simple plan for adding a README file"

Expected: Planner agent responds with structured plan
```

### Test 2: Steering Files Work ✅
```
In Kiro chat, ask:
"What are the coding standards for this project?"

Expected: Kiro references coding-standards.md automatically
```

### Test 3: Command Works ✅
```
In Kiro chat, type:
"Run the code review command" or "/code-review"

Expected: Code reviewer checks git diff for issues
```

### Test 4: File Operations Work ✅
```
In Kiro chat, ask:
"Read the package.json file and list the available scripts"

Expected: Kiro reads and displays package.json scripts
```

## If Something Doesn't Work

### Quick Fixes

**Agents not loading?**
```bash
# Check agents exist
dir .kiro\agents\*.json

# Should show 12 files
```

**Steering files not referenced?**
```bash
# Check steering files exist
dir .kiro\steering\*.md

# Should show 19+ files
```

**File reference errors?**
```bash
# Verify original agent files exist
dir agents\*.md

# Should show 12 files
```

**JSON syntax errors?**
```bash
# Validate JSON (requires Node.js)
node -e "JSON.parse(require('fs').readFileSync('.kiro/agents/planner.json'))"

# Should output the JSON or show syntax error
```

## What to Test

Priority order:

1. ✅ **Agents load** (most important)
2. ✅ **Planner agent works** (test one agent)
3. ✅ **Steering files referenced** (test guidance)
4. ✅ **Commands work** (test slash commands)
5. ✅ **File operations work** (test tools)

## Full Testing

For comprehensive testing, see: `KIRO-TESTING-GUIDE.md`

## Conversion Status

Current completion: **59%**

See: `KIRO-CONVERSION-STATUS.md` for details

## Need Help?

1. Check `KIRO-TESTING-GUIDE.md` for detailed troubleshooting
2. Review `KIRO-CONVERSION-STATUS.md` for known issues
3. Compare with original `agents/*.md` files
4. Check Kiro IDE documentation

## Next Steps

After quick testing:

1. Run full test suite (see KIRO-TESTING-GUIDE.md)
2. Complete remaining conversions (16 commands, MCP config)
3. Test in real development workflow
4. Report any issues found

---

**Quick Start Complete!** 🎉

You should now have:
- ✅ Kiro IDE installed
- ✅ Repository opened in Kiro
- ✅ Basic functionality tested
- ✅ Ready for full testing

Next: See `KIRO-TESTING-GUIDE.md` for comprehensive testing
