# Kiro Hooks

This directory contains Kiro IDE hooks that automate tasks based on file events.

## Available Hooks

### 1. Auto-Format on Save (`auto-format.json`)
**Trigger:** When `.ts`, `.tsx`, `.js`, `.jsx` files are edited
**Action:** Runs Prettier to format the file
**Type:** `runCommand`

### 2. Check Console.log (`check-console-log.json`)
**Trigger:** When agent completes a response
**Action:** Asks agent to check for console.log statements
**Type:** `askAgent`

### 3. Security Scan (`security-scan.json`)
**Trigger:** When code files are edited
**Action:** Asks agent to scan for security issues
**Type:** `askAgent`

### 4. TypeScript Type Check (`type-check.json`)
**Trigger:** When `.ts`, `.tsx` files are edited
**Action:** Runs TypeScript compiler to check types
**Type:** `runCommand`

### 5. Test on Save (`test-on-save.json`)
**Trigger:** When code files are edited
**Action:** Asks agent to run relevant tests
**Type:** `askAgent`

## Hook Structure

Each hook follows this schema:

```json
{
  "name": "Hook Name",
  "version": "1.0.0",
  "description": "What this hook does",
  "when": {
    "type": "fileEdited|fileCreated|fileDeleted|userTriggered|promptSubmit|agentStop",
    "patterns": ["**/*.{ts,tsx}"]  // Only for file events
  },
  "then": {
    "type": "askAgent|runCommand",
    "prompt": "Prompt for agent",  // For askAgent
    "command": "shell command"     // For runCommand
  }
}
```

## Event Types

- **fileEdited**: When files are saved/edited
- **fileCreated**: When new files are created
- **fileDeleted**: When files are deleted
- **userTriggered**: Manual hook execution
- **promptSubmit**: When user sends a message
- **agentStop**: When agent completes a response

## Action Types

- **askAgent**: Send a prompt to the AI agent (intelligent, context-aware)
- **runCommand**: Execute a shell command (fast, simple operations)

## Enabling/Disabling Hooks

Hooks can be managed through:
1. **Kiro IDE UI**: Agent Hooks panel in the sidebar
2. **File System**: Add/remove JSON files in this directory
3. **Hook Files**: Edit the JSON files directly

## Best Practices

1. **Use Specific Patterns**: Avoid `**/*` to prevent excessive triggers
2. **Keep Actions Fast**: Long-running commands can slow down workflow
3. **Use askAgent for Intelligence**: Let the agent make decisions
4. **Use runCommand for Speed**: Simple operations like formatting
5. **Test Thoroughly**: Verify hooks work as expected before enabling

## Differences from Claude Code Hooks

Claude Code hooks were tool-based (PreToolUse, PostToolUse, etc.). Kiro hooks are event-based (fileEdited, agentStop, etc.).

**Mapping:**
- Claude Code `PostToolUse` (Edit) → Kiro `fileEdited`
- Claude Code `Stop` → Kiro `agentStop`
- Claude Code `SessionStart` → Kiro `promptSubmit` (first message)
- Claude Code `PreToolUse` (Bash) → Not directly mappable (use askAgent instead)

## Variables

In hook commands, you can use:
- `${file}` - The file that triggered the hook
- `${workspace}` - The workspace root directory

## Examples

### Example 1: Lint on Save
```json
{
  "name": "Lint on Save",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["**/*.{ts,tsx}"]
  },
  "then": {
    "type": "runCommand",
    "command": "npx eslint \"${file}\" --fix"
  }
}
```

### Example 2: Security Review After Response
```json
{
  "name": "Security Review",
  "version": "1.0.0",
  "when": {
    "type": "agentStop"
  },
  "then": {
    "type": "askAgent",
    "prompt": "Review all modified files for security vulnerabilities: hardcoded secrets, SQL injection, XSS. Report any issues found."
  }
}
```

### Example 3: Create Test File
```json
{
  "name": "Create Test File",
  "version": "1.0.0",
  "when": {
    "type": "fileCreated",
    "patterns": ["src/**/*.{ts,tsx}"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "A new source file was created. Check if a corresponding test file exists. If not, suggest creating one following TDD principles."
  }
}
```

## Troubleshooting

**Hook not triggering:**
- Check file patterns match the edited files
- Verify hook is enabled in Kiro UI
- Check JSON syntax is valid

**Command fails:**
- Verify command works in terminal
- Check file paths are correct
- Ensure required tools are installed (prettier, eslint, etc.)

**Agent not responding:**
- Check prompt is clear and actionable
- Verify agent has necessary permissions
- Try simplifying the prompt

## Related Documentation

- See `.kiro/steering/automation-hooks.md` for hook patterns
- See Kiro IDE documentation for advanced hook features
- See `hooks/hooks.json` for original Claude Code hooks (reference only)
