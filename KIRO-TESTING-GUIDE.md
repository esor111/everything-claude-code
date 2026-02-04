# Kiro IDE Testing Guide

Step-by-step guide to test the converted Kiro configuration.

## Prerequisites

1. **Install Kiro IDE** (if not already installed)
   - Download from: https://kiro.ai or official source
   - Install and launch the application

2. **Open This Repository in Kiro**
   - File → Open Folder
   - Select the `everything-claude-code` directory
   - Kiro should detect the `.kiro/` configuration automatically

## Testing Steps

### Step 1: Verify Directory Structure ✅

Check that all directories were created correctly:

```bash
# Run this command to verify structure
dir .kiro /s /b

# Expected output:
# .kiro\agents
# .kiro\settings
# .kiro\steering
# .kiro\agents\architect.json
# .kiro\agents\build-error-resolver.json
# ... (12 agent files)
# .kiro\steering\agent-orchestration.md
# .kiro\steering\automation-hooks.md
# ... (19+ steering files)
```

**✅ Pass Criteria:** All directories exist, 12 agent JSON files, 19+ steering files

---

### Step 2: Test Agent Loading 🤖

**Test each agent loads without errors:**

1. Open Kiro IDE
2. Open the Command Palette (Ctrl+Shift+P or Cmd+Shift+P)
3. Type "Kiro: List Agents" or look for agent selection
4. Verify all 12 agents appear:
   - ✅ planner
   - ✅ tdd-guide
   - ✅ architect
   - ✅ security-reviewer
   - ✅ build-error-resolver
   - ✅ code-reviewer
   - ✅ e2e-runner
   - ✅ refactor-cleaner
   - ✅ doc-updater
   - ✅ database-reviewer
   - ✅ go-reviewer
   - ✅ go-build-resolver

**Manual Test:**

```
In Kiro chat, type:
"Use the planner agent to create a plan for adding a new feature"

Expected: Kiro should invoke the planner agent and create a structured plan
```

**✅ Pass Criteria:** All agents visible, no loading errors, planner agent responds

---

### Step 3: Test Steering Files (Always-Included) 📋

**Verify steering files are loaded automatically:**

1. In Kiro chat, ask a coding question:
   ```
   "What are the coding standards for this project?"
   ```

2. Kiro should reference the steering files automatically:
   - Should mention immutability patterns
   - Should reference 80% test coverage requirement
   - Should mention TDD workflow

**Test specific steering files:**

```
Test 1: "What's the git workflow for this project?"
Expected: References git-workflow.md (conventional commits, PR process)

Test 2: "What security checks should I perform?"
Expected: References security-guidelines.md (no hardcoded secrets, SQL injection, etc.)

Test 3: "What's the project structure?"
Expected: References structure.md (directory organization)
```

**✅ Pass Criteria:** Kiro references steering files in responses without being explicitly told

---

### Step 4: Test Manual Steering Files (Slash Commands) ⚡

**Test converted commands work as slash commands:**

In Kiro chat, try these commands:

```
Test 1: Type "/plan" or mention "plan command"
Expected: Invokes planner agent, creates implementation plan

Test 2: Type "/code-review" or mention "code review command"
Expected: Reviews git diff, checks for security issues

Test 3: Type "/verify" or mention "verify command"
Expected: Runs build, types, lint, tests checks

Test 4: Type "/build-fix" or mention "build fix command"
Expected: Attempts to fix build errors incrementally

Test 5: Type "/test-coverage" or mention "test coverage command"
Expected: Analyzes coverage, suggests missing tests

Test 6: Type "/update-docs" or mention "update docs command"
Expected: Syncs documentation from package.json
```

**Note:** Slash command syntax may vary in Kiro. If `/command` doesn't work, try:
- Mentioning the command naturally: "Run the plan command"
- Using Kiro's command palette
- Checking Kiro's documentation for slash command syntax

**✅ Pass Criteria:** At least 3 commands work correctly

---

### Step 5: Test Agent Tool Permissions 🔧

**Verify agents have correct tool access:**

1. **Test file reading:**
   ```
   Ask planner agent: "Read the package.json file and tell me what scripts are available"
   Expected: Agent can read and parse package.json
   ```

2. **Test file writing:**
   ```
   Ask tdd-guide agent: "Create a test file for a simple add function"
   Expected: Agent creates a .test.ts file
   ```

3. **Test bash execution:**
   ```
   Ask build-error-resolver: "Run npm run build and show me any errors"
   Expected: Agent executes build command
   ```

4. **Test grep/search:**
   ```
   Ask code-reviewer: "Search for console.log statements in src/"
   Expected: Agent uses grep to find console.log
   ```

**✅ Pass Criteria:** Agents can use their allowed tools without permission errors

---

### Step 6: Test Agent-Specific Functionality 🎯

**Test each agent's core functionality:**

#### 6.1 Planner Agent
```
Request: "Plan a feature to add user authentication"
Expected:
- Restates requirements
- Breaks into phases
- Identifies risks
- Waits for confirmation before proceeding
```

#### 6.2 TDD Guide Agent
```
Request: "Use TDD to create a function that validates email addresses"
Expected:
- Defines interface first
- Writes failing test
- Implements minimal code
- Verifies tests pass
- Checks coverage
```

#### 6.3 Code Reviewer Agent
```
Request: "Review the changes in my last commit"
Expected:
- Runs git diff
- Checks for security issues
- Checks code quality
- Provides severity ratings
```

#### 6.4 Security Reviewer Agent
```
Request: "Scan the codebase for security vulnerabilities"
Expected:
- Checks for hardcoded secrets
- Checks for SQL injection risks
- Checks for XSS vulnerabilities
- Provides security report
```

**✅ Pass Criteria:** Each agent performs its specialized function correctly

---

### Step 7: Test Model Selection 🧠

**Verify correct models are used:**

Check `.kiro/agents/*.json` files have correct model mappings:

```bash
# Check model assignments
findstr /s "model" .kiro\agents\*.json

Expected output should show:
- planner.json: "claude-opus-4"
- tdd-guide.json: "claude-opus-4"
- code-reviewer.json: "claude-sonnet-4"
- etc.
```

**In Kiro:**
- Opus agents (planner, architect, security-reviewer) should provide deeper reasoning
- Sonnet agents should be faster for routine tasks

**✅ Pass Criteria:** Agents use appropriate models, no model loading errors

---

### Step 8: Test File References 📁

**Verify agent prompt files are found:**

Each agent JSON references its markdown file via `file://../agents/[name].md`

Test that references work:

```
In Kiro, invoke any agent and check:
1. Agent loads without "file not found" errors
2. Agent behavior matches the markdown description
3. Agent follows instructions from the markdown file
```

**Manual verification:**

```bash
# Check all referenced files exist
for %f in (.kiro\agents\*.json) do (
    echo Checking %f
    type %f | findstr "file://"
)

# Verify corresponding .md files exist in agents/
dir agents\*.md
```

**✅ Pass Criteria:** All file references resolve, no 404 errors

---

### Step 9: Test Steering File Inclusion 📚

**Test always-included vs manual-included steering:**

1. **Always-included test:**
   ```
   Ask: "What are the coding standards?"
   Expected: Kiro references coding-standards.md automatically
   ```

2. **Manual-included test:**
   ```
   Ask: "Run the plan command"
   Expected: Kiro loads cmd-plan.md only when requested
   ```

3. **Check front-matter:**
   ```bash
   # Verify manual inclusion front-matter
   findstr /s "inclusion: manual" .kiro\steering\cmd-*.md
   
   Expected: All cmd-*.md files have "inclusion: manual"
   ```

**✅ Pass Criteria:** Always-included files load automatically, manual files load on demand

---

### Step 10: Integration Test 🔗

**End-to-end workflow test:**

Simulate a complete development workflow:

```
Step 1: "Use the planner agent to plan adding a login feature"
Expected: Planner creates detailed plan

Step 2: "Confirm the plan and use TDD to implement"
Expected: TDD guide creates tests first, then implementation

Step 3: "Run code review on the changes"
Expected: Code reviewer checks for issues

Step 4: "Run verification checks"
Expected: Verify command runs build, tests, lint

Step 5: "Update documentation for the new feature"
Expected: Doc updater syncs documentation
```

**✅ Pass Criteria:** Complete workflow executes without errors

---

## Common Issues & Troubleshooting

### Issue 1: Agents Not Loading

**Symptoms:** Agents don't appear in Kiro's agent list

**Solutions:**
1. Check `.kiro/agents/` directory exists
2. Verify JSON syntax is valid:
   ```bash
   # Validate JSON files (requires Node.js)
   for %f in (.kiro\agents\*.json) do node -e "JSON.parse(require('fs').readFileSync('%f'))"
   ```
3. Check file permissions
4. Restart Kiro IDE

### Issue 2: File References Not Found

**Symptoms:** "File not found" errors when loading agents

**Solutions:**
1. Verify relative path is correct: `file://../agents/[name].md`
2. Check that `agents/[name].md` files exist
3. Ensure no typos in filenames
4. Check file path separators (use `/` not `\`)

### Issue 3: Steering Files Not Loading

**Symptoms:** Kiro doesn't reference steering files

**Solutions:**
1. Check `.kiro/steering/` directory exists
2. Verify files are `.md` format
3. Check front-matter syntax for manual inclusion
4. Restart Kiro to reload steering files

### Issue 4: Tool Permission Errors

**Symptoms:** "Tool not allowed" errors

**Solutions:**
1. Check `allowedTools` array in agent JSON
2. Verify tool names are correct (fs_read, not Read)
3. Add missing tools to allowedTools array
4. Restart Kiro

### Issue 5: Model Not Found

**Symptoms:** "Model not available" errors

**Solutions:**
1. Check model name is correct (claude-opus-4, not opus)
2. Verify Kiro has access to Claude models
3. Check API keys are configured
4. Try alternative model (claude-sonnet-4)

---

## Validation Checklist

Use this checklist to confirm everything works:

### Configuration Files
- [ ] `.kiro/` directory exists
- [ ] `.kiro/agents/` contains 12 JSON files
- [ ] `.kiro/steering/` contains 19+ MD files
- [ ] All JSON files have valid syntax
- [ ] All file references resolve correctly

### Agents
- [ ] All 12 agents appear in Kiro
- [ ] Planner agent works
- [ ] TDD guide agent works
- [ ] Code reviewer agent works
- [ ] Security reviewer agent works
- [ ] Build error resolver agent works
- [ ] No agent loading errors

### Steering Files
- [ ] Always-included files load automatically
- [ ] Manual-included files load on demand
- [ ] Coding standards are referenced
- [ ] Security guidelines are referenced
- [ ] TDD workflow is referenced

### Commands (Manual Steering)
- [ ] Plan command works
- [ ] Code review command works
- [ ] Verify command works
- [ ] Build fix command works
- [ ] Test coverage command works
- [ ] Update docs command works

### Tools & Permissions
- [ ] Agents can read files
- [ ] Agents can write files
- [ ] Agents can execute bash commands
- [ ] Agents can use grep/search
- [ ] No permission errors

### Integration
- [ ] Complete workflow executes successfully
- [ ] Agents work together correctly
- [ ] No file reference errors
- [ ] No model loading errors

---

## Next Steps After Testing

Once testing is complete:

1. **Document Issues:** Note any problems in `KIRO-CONVERSION-STATUS.md`
2. **Fix Issues:** Address any failing tests
3. **Complete Conversion:** Convert remaining commands and MCP config
4. **Create User Guide:** Document how to use Kiro with this repository
5. **Update README:** Add Kiro-specific instructions

---

## Getting Help

If you encounter issues:

1. **Check Kiro Documentation:** https://kiro.ai/docs (or official docs)
2. **Review Conversion Status:** See `KIRO-CONVERSION-STATUS.md`
3. **Check Original Files:** Compare with `agents/*.md` originals
4. **Test in Claude Code:** Verify original configuration works
5. **Report Issues:** Document problems for troubleshooting

---

## Success Criteria

The conversion is successful when:

✅ All 12 agents load without errors
✅ Steering files are referenced automatically
✅ Manual commands work as slash commands
✅ Agents can use their allowed tools
✅ Complete workflow executes successfully
✅ No file reference errors
✅ No model loading errors

**Estimated Testing Time:** 30-45 minutes for complete validation

Good luck with testing! 🚀
