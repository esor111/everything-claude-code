# Project Structure

## Directory Organization

```
everything-claude-code/
├── .kiro/                      # Kiro IDE configurations
│   ├── steering/              # Context and guidance files
│   ├── agents/                # Custom agent configurations (JSON)
│   └── settings/              # MCP and other settings
├── agents/                     # Claude Code agents (Markdown)
├── skills/                     # Reusable workflow definitions
├── commands/                   # Slash command definitions
├── rules/                      # Always-follow guidelines
├── hooks/                      # Event-driven automation
├── contexts/                   # Dynamic system prompt injection
├── scripts/                    # Utility scripts
│   ├── hooks/                 # Hook implementation scripts
│   └── lib/                   # Shared utilities
├── mcp-configs/               # MCP server configurations
├── examples/                  # Example configurations
└── tests/                     # Test suite
```

## File Naming Conventions

### Steering Files
- **kebab-case.md** - All steering files use kebab-case
- **Descriptive names** - `security-guidelines.md`, `api-standards.md`
- **Front-matter** - YAML configuration at file start

### Agent Files
- **kebab-case.json** - Agent configurations in JSON format
- **Match purpose** - `planner.json`, `code-reviewer.json`

### Scripts
- **kebab-case.js** - JavaScript utility scripts
- **Descriptive names** - `setup-package-manager.js`

### Tests
- **name.test.js** - Test files with `.test.js` suffix
- **Co-located** - Tests near the code they test

## Import Patterns

### Absolute Imports (Preferred)
```typescript
import { utility } from '@/lib/utils'
import { Component } from '@/components/Component'
```

### Relative Imports (When Necessary)
```typescript
import { helper } from './helper'
import { config } from '../config'
```

## Code Organization

### Component Structure
```
ComponentName/
├── index.ts                   # Public exports
├── ComponentName.tsx          # Implementation
├── ComponentName.test.tsx     # Tests
├── types.ts                   # Type definitions
└── hooks.ts                   # Custom hooks
```

### API Structure
```
api/
├── route.ts                   # Route handler
├── route.test.ts              # Integration tests
├── schema.ts                  # Validation schemas
└── types.ts                   # Type definitions
```

## Configuration Files

### Root Level
- `package.json` - Dependencies and scripts
- `tsconfig.json` - TypeScript configuration
- `eslint.config.js` - Linting rules
- `.gitignore` - Git exclusions
- `README.md` - Project documentation

### Kiro Configuration
- `.kiro/steering/*.md` - Steering files (always loaded by default)
- `.kiro/agents/*.json` - Custom agent definitions
- `.kiro/settings/mcp.json` - MCP server configurations

## Architectural Decisions

### Modular Design
- Keep files under 500 lines
- Single responsibility per file
- Clear separation of concerns

### Type Safety
- Use TypeScript for all new code
- Explicit types over `any`
- Interface definitions for public APIs

### Testing Strategy
- Tests co-located with code
- TDD approach (tests first)
- 80%+ coverage requirement

### Documentation
- README.md for project overview
- Inline comments for complex logic
- JSDoc for public APIs
- Steering files for AI guidance

## Best Practices

1. **File Size** - Keep files focused and under 500 lines
2. **Naming** - Use descriptive, searchable names
3. **Exports** - Use named exports over default exports
4. **Imports** - Group and order imports logically
5. **Comments** - Explain why, not what
6. **Tests** - Write tests before implementation
7. **Types** - Define types explicitly
8. **Errors** - Handle errors gracefully
