# Automation Hooks

## Kiro Hook System

Kiro supports event-driven automation through hooks that trigger on specific events:

### Hook Event Types

- **fileEdited**: When files are saved/edited
- **fileCreated**: When new files are created
- **fileDeleted**: When files are deleted
- **userTriggered**: Manual hook execution
- **promptSubmit**: When user sends a message
- **agentStop**: When agent completes a response

### Hook Actions

1. **askAgent**: Send a prompt to the AI agent
2. **runCommand**: Execute a shell command

## Common Hook Patterns

### Auto-Format on Save
Trigger: fileEdited on `**/*.{ts,tsx,js,jsx}`
Action: Run Prettier to format code

### Test on Save
Trigger: fileEdited on `**/*.{ts,tsx,js,jsx}`
Action: Run relevant tests for the edited file

### Security Scan
Trigger: agentStop
Action: Check modified files for console.log, hardcoded secrets

### Documentation Sync
Trigger: fileEdited on `src/**/*.{ts,tsx}`
Action: Update corresponding documentation

## Best Practices

- Use specific file patterns to avoid excessive hook triggers
- Keep hook actions fast and focused
- Use `askAgent` for intelligent context-aware actions
- Use `runCommand` for simple shell operations
- Test hooks thoroughly before enabling in production

## Hook Configuration

Hooks can be configured through:
1. Kiro IDE UI (Agent Hooks panel)
2. Markdown files in `.kiro/hooks/` directory
3. Agent configuration files (for CLI hooks)

Note: Kiro IDE hooks are different from CLI hooks. IDE hooks are event-driven and UI-managed, while CLI hooks are defined in agent JSON configurations.
