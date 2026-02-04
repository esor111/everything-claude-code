# Steering System: Complete Architecture & Philosophy

## Table of Contents

1. [Core Philosophy](#core-philosophy)
2. [System Architecture](#system-architecture)
3. [The Three-Tier Model](#the-three-tier-model)
4. [Context Injection Mechanism](#context-injection-mechanism)
5. [Conflict Resolution (MCB)](#conflict-resolution-mcb)
6. [File Reference System](#file-reference-system)
7. [Event-Driven Architecture](#event-driven-architecture)
8. [Implementation Guide](#implementation-guide)
9. [Design Patterns](#design-patterns)
10. [Real-World Examples](#real-world-examples)

---

## Core Philosophy

### The Problem This Solves

**Without Steering:**
- AI has no project context
- Inconsistent code quality
- Security vulnerabilities slip through
- No enforced testing standards
- Every interaction starts from zero knowledge

**With Steering:**
- AI understands project standards automatically
- Consistent patterns across all code
- Security checks are automatic
- TDD is enforced by default
- Context accumulates and compounds

### The Constraint-Based Creativity Principle

```
Freedom WITHOUT Structure = Chaos
Freedom WITH Structure = Mastery
```

Like jazz improvisation, steering provides:
- **Guardrails** (what NOT to do)
- **Patterns** (proven solutions)
- **Standards** (consistency)
- **Workflows** (processes)

This paradoxically INCREASES productivity by:
1. Reducing decision paralysis
2. Ensuring consistency
3. Catching errors early
4. Building on proven patterns

---

## System Architecture

### The Five Layers

```
┌─────────────────────────────────────────┐
│  Layer 5: User Interaction              │
│  (Chat, Commands, File Edits)           │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│  Layer 4: Event System                  │
│  (Hooks, Triggers, Lifecycle)           │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│  Layer 3: Context Injection             │
│  (Steering Rules, Priority Resolution)  │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│  Layer 2: Agent Orchestration           │
│  (Specialized Agents, Parallel Exec)    │
└─────────────────────────────────────────┘
                  ↓
┌─────────────────────────────────────────┐
│  Layer 1: AI Model                      │
│  (Claude, GPT, etc.)                    │
└─────────────────────────────────────────┘
```

### Directory Structure

```
.kiro/
├── steering/              # Context injection rules
│   ├── always-active/    # Loaded every interaction
│   │   ├── security-guidelines.md
│   │   ├── coding-standards.md
│   │   └── tdd-workflow.md
│   ├── conditional/      # Loaded based on patterns
│   │   ├── golang-patterns.md
│   │   ├── frontend-patterns.md
│   │   └── postgres-patterns.md
│   └── manual/           # Loaded on explicit reference
│       └── advanced-optimization.md
├── agents/               # Specialized AI workers
│   ├── planner.json
│   ├── code-reviewer.json
│   └── security-reviewer.json
├── hooks/                # Event-driven automation
│   └── hooks.json
└── settings/             # Configuration
    └── mcp.json
```

---

## The Three-Tier Model

### Context, Command, Agent (CCA)

This is the fundamental architecture pattern:

```
┌──────────────────────────────────────────────────┐
│                   CONTEXT                        │
│  (Steering Rules - Passive Guidance)             │
│  "How to think about problems"                   │
│                                                  │
│  Examples:                                       │
│  - Always use TDD                                │
│  - Security checks mandatory                     │
│  - Immutability required                         │
└──────────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────────┐
│                   COMMAND                        │
│  (Slash Commands - Explicit Actions)             │
│  "What to do right now"                          │
│                                                  │
│  Examples:                                       │
│  - /plan - Create implementation plan            │
│  - /review - Review current code                 │
│  - /verify - Run verification loop               │
└──────────────────────────────────────────────────┘
                      ↓
┌──────────────────────────────────────────────────┐
│                    AGENT                         │
│  (Custom Agents - Autonomous Workers)            │
│  "Who does the work"                             │
│                                                  │
│  Examples:                                       │
│  - planner: Creates detailed plans               │
│  - code-reviewer: Analyzes code quality          │
│  - tdd-guide: Enforces test-first development    │
└──────────────────────────────────────────────────┘
```

### Why This Separation Matters

**Context** provides the "operating system" - always present, shapes all behavior
**Command** provides the "application" - specific task to execute
**Agent** provides the "specialist" - focused expertise for complex tasks

This separation enables:
- **Composability**: Mix and match contexts, commands, agents
- **Reusability**: Same context works across different commands
- **Specialization**: Agents can be highly focused
- **Maintainability**: Change one layer without affecting others

---

## Context Injection Mechanism

### How Steering Rules Get Into Prompts

```typescript
// Conceptual implementation
class SteeringEngine {
  async buildContext(interaction: Interaction): Promise<string> {
    const rules: SteeringRule[] = []
    
    // 1. Load always-active rules
    rules.push(...this.loadAlwaysActive())
    
    // 2. Load conditional rules based on file patterns
    if (interaction.activeFile) {
      rules.push(...this.loadConditional(interaction.activeFile))
    }
    
    // 3. Load manually referenced rules
    if (interaction.hasContextReferences) {
      rules.push(...this.loadManual(interaction.contextReferences))
    }
    
    // 4. Resolve conflicts (priority system)
    const resolved = this.resolveConflicts(rules)
    
    // 5. Expand file references
    const expanded = await this.expandFileReferences(resolved)
    
    // 6. Build final context string
    return this.formatContext(expanded)
  }
}
```

### Front-Matter Configuration

```yaml
---
# Inclusion strategy
inclusion: always | fileMatch | manual

# Pattern for conditional loading (glob syntax)
fileMatchPattern: '**/*.go'

# Priority for conflict resolution (higher = wins)
priority: 100

# Tags for organization
tags: [backend, database, performance]

# Dependencies (load these rules too)
requires: [coding-standards, security-guidelines]
---

# Rule content starts here
```

### The Three Inclusion Modes

#### 1. Always Active (Default)

```markdown
---
inclusion: always
---

# Security Guidelines

ALWAYS check for:
- Hardcoded secrets
- SQL injection
- XSS vulnerabilities
```

**When loaded**: Every single interaction
**Use case**: Foundational rules that apply universally

#### 2. File Match (Conditional)

```markdown
---
inclusion: fileMatch
fileMatchPattern: '**/*.go'
---

# Go Patterns

When writing Go code:
- Use interfaces for dependencies
- Return errors, don't panic
- Make zero value useful
```

**When loaded**: Only when editing Go files
**Use case**: Language/framework-specific guidance

#### 3. Manual (On-Demand)

```markdown
---
inclusion: manual
---

# Advanced Performance Optimization

Deep optimization techniques:
- Memory pooling strategies
- Lock-free algorithms
- SIMD vectorization
```

**When loaded**: Only when user types `#advanced-optimization`
**Use case**: Specialized knowledge not needed by default

---

## Conflict Resolution (MCB)

### The Priority Hierarchy

```
Workspace Rules (Priority: 100)
       ↓ (overrides)
Global Rules (Priority: 50)
       ↓ (overrides)
System Rules (Priority: 10)
```

### MCB = Model, Context, Base

- **M**odel: System-level defaults (lowest priority)
- **C**ontext: Workspace-specific rules (highest priority)
- **B**ase: Global user preferences (middle priority)

### Conflict Resolution Algorithm

```typescript
function resolveConflicts(rules: SteeringRule[]): SteeringRule[] {
  // 1. Group by topic/category
  const grouped = groupByTopic(rules)
  
  // 2. Within each group, sort by priority
  for (const [topic, topicRules] of grouped) {
    topicRules.sort((a, b) => b.priority - a.priority)
  }
  
  // 3. Take highest priority rule per topic
  const resolved = []
  for (const [topic, topicRules] of grouped) {
    resolved.push(topicRules[0]) // Highest priority
  }
  
  return resolved
}
```

### Example Conflict

```markdown
# System Rule (Priority: 10)
Test coverage: 70% minimum

# Global Rule (Priority: 50)
Test coverage: 80% minimum

# Workspace Rule (Priority: 100)
Test coverage: 90% minimum for this critical project

# RESULT: 90% minimum (workspace wins)
```

---

## File Reference System

### The Transclusion Pattern

Instead of copying content, reference it:

```markdown
# API Standards

Our API follows this specification:

#[[file:specs/openapi.yaml]]

All endpoints must conform to this schema.
```

### Why This Matters

**Without transclusion:**
- Duplicate content everywhere
- Updates require changing multiple files
- Version drift between copies
- Wastes tokens

**With transclusion:**
- Single source of truth
- Update once, applies everywhere
- Always in sync
- Efficient token usage

### Implementation

```typescript
async function expandFileReferences(content: string): Promise<string> {
  const pattern = /#\[\[file:([^\]]+)\]\]/g
  let expanded = content
  
  for (const match of content.matchAll(pattern)) {
    const filePath = match[1]
    const fileContent = await readFile(filePath)
    expanded = expanded.replace(match[0], fileContent)
  }
  
  return expanded
}
```

### Advanced Usage

```markdown
# Database Schema

Current schema:
#[[file:schema.sql]]

Migration history:
#[[file:migrations/001_initial.sql]]
#[[file:migrations/002_add_indexes.sql]]

GraphQL API:
#[[file:schema.graphql]]
```

---

## Event-Driven Architecture

### Hook System Overview

```
Event Occurs → Pattern Match → Action Triggered → Result
```

### Event Types & Use Cases

| Event | When | Common Actions |
|-------|------|----------------|
| `fileEdited` | File saved | Format, lint, test |
| `fileCreated` | New file | Add boilerplate, update imports |
| `fileDeleted` | File removed | Clean references, update docs |
| `promptSubmit` | User sends message | Log, analyze intent |
| `agentStop` | AI finishes response | Verify, test, commit |
| `userTriggered` | Manual button click | Custom workflows |

### Hook Configuration Schema

```json
{
  "name": "Auto-format on save",
  "version": "1.0.0",
  "when": {
    "type": "fileEdited",
    "patterns": ["**/*.{ts,tsx,js,jsx}"]
  },
  "then": {
    "type": "runCommand",
    "command": "prettier --write {{file}}"
  }
}
```

### Hook Action Types

#### 1. askAgent (Intelligent)

```json
{
  "type": "askAgent",
  "prompt": "Review the changes in {{file}} for security issues"
}
```

**When to use**: Need context-aware analysis
**Examples**: Code review, security scan, documentation update

#### 2. runCommand (Fast)

```json
{
  "type": "runCommand",
  "command": "npm test {{file}}"
}
```

**When to use**: Simple, deterministic operations
**Examples**: Format, lint, compile, test

### Hook Lifecycle

```
1. Event occurs (e.g., file saved)
2. Pattern matching (does file match hook pattern?)
3. Action preparation (substitute variables)
4. Execution (run command or ask agent)
5. Result handling (success/failure)
6. Logging (record for debugging)
```

---

## Implementation Guide

### Building Your Own Steering System

#### Step 1: Context Loader

```typescript
interface SteeringRule {
  id: string
  content: string
  inclusion: 'always' | 'fileMatch' | 'manual'
  fileMatchPattern?: string
  priority: number
}

class SteeringLoader {
  private rulesDir: string
  
  async loadAll(): Promise<SteeringRule[]> {
    const files = await this.findMarkdownFiles(this.rulesDir)
    const rules: SteeringRule[] = []
    
    for (const file of files) {
      const content = await readFile(file)
      const rule = this.parseRule(content, file)
      rules.push(rule)
    }
    
    return rules
  }
  
  private parseRule(content: string, filePath: string): SteeringRule {
    const { frontMatter, body } = this.parseFrontMatter(content)
    
    return {
      id: path.basename(filePath, '.md'),
      content: body,
      inclusion: frontMatter.inclusion || 'always',
      fileMatchPattern: frontMatter.fileMatchPattern,
      priority: frontMatter.priority || 50
    }
  }
}
```

#### Step 2: Pattern Matcher

```typescript
import minimatch from 'minimatch'

class PatternMatcher {
  matches(filePath: string, pattern: string): boolean {
    return minimatch(filePath, pattern, {
      dot: true,
      matchBase: true
    })
  }
  
  findMatchingRules(
    filePath: string,
    rules: SteeringRule[]
  ): SteeringRule[] {
    return rules.filter(rule => {
      if (rule.inclusion !== 'fileMatch') return false
      if (!rule.fileMatchPattern) return false
      return this.matches(filePath, rule.fileMatchPattern)
    })
  }
}
```

#### Step 3: Context Builder

```typescript
class ContextBuilder {
  async build(interaction: Interaction): Promise<string> {
    const loader = new SteeringLoader('.kiro/steering')
    const matcher = new PatternMatcher()
    
    // Load all rules
    const allRules = await loader.loadAll()
    
    // Filter applicable rules
    const applicable: SteeringRule[] = []
    
    // Always-active rules
    applicable.push(
      ...allRules.filter(r => r.inclusion === 'always')
    )
    
    // Conditional rules
    if (interaction.activeFile) {
      applicable.push(
        ...matcher.findMatchingRules(interaction.activeFile, allRules)
      )
    }
    
    // Manual rules
    if (interaction.contextReferences) {
      applicable.push(
        ...allRules.filter(r => 
          r.inclusion === 'manual' &&
          interaction.contextReferences.includes(r.id)
        )
      )
    }
    
    // Resolve conflicts
    const resolved = this.resolveConflicts(applicable)
    
    // Expand file references
    const expanded = await this.expandReferences(resolved)
    
    // Format for prompt
    return this.format(expanded)
  }
  
  private resolveConflicts(rules: SteeringRule[]): SteeringRule[] {
    // Group by ID, take highest priority
    const byId = new Map<string, SteeringRule>()
    
    for (const rule of rules) {
      const existing = byId.get(rule.id)
      if (!existing || rule.priority > existing.priority) {
        byId.set(rule.id, rule)
      }
    }
    
    return Array.from(byId.values())
  }
  
  private format(rules: SteeringRule[]): string {
    return rules.map(rule => `
## Included Rules (${rule.id}) [Workspace]

<user-rule id=${rule.id}>
\`\`\`
${rule.content}
\`\`\`
</user-rule>
    `).join('\n\n')
  }
}
```

#### Step 4: Hook Engine

```typescript
interface Hook {
  name: string
  when: {
    type: EventType
    patterns?: string[]
  }
  then: {
    type: 'askAgent' | 'runCommand'
    prompt?: string
    command?: string
  }
}

class HookEngine {
  private hooks: Hook[] = []
  
  async trigger(event: Event): Promise<void> {
    const matching = this.findMatchingHooks(event)
    
    for (const hook of matching) {
      await this.execute(hook, event)
    }
  }
  
  private findMatchingHooks(event: Event): Hook[] {
    return this.hooks.filter(hook => {
      if (hook.when.type !== event.type) return false
      
      if (hook.when.patterns && event.filePath) {
        return hook.when.patterns.some(pattern =>
          minimatch(event.filePath!, pattern)
        )
      }
      
      return true
    })
  }
  
  private async execute(hook: Hook, event: Event): Promise<void> {
    if (hook.then.type === 'runCommand') {
      const command = this.substituteVariables(
        hook.then.command!,
        event
      )
      await this.runCommand(command)
    } else {
      const prompt = this.substituteVariables(
        hook.then.prompt!,
        event
      )
      await this.askAgent(prompt)
    }
  }
}
```

---

## Design Patterns

### Pattern 1: Layered Guidance

```
Generic (Always) → Specific (Conditional) → Expert (Manual)
```

Example:
1. **Always**: "Use TypeScript for type safety"
2. **Conditional**: "For React components, use functional components"
3. **Manual**: "For performance-critical code, use React.memo and useMemo"

### Pattern 2: Progressive Disclosure

Don't overwhelm with all information at once:

```markdown
# Basic (Always Active)
Use TDD: Write tests first

# Intermediate (File Match: **/*.test.ts)
Use Jest matchers: expect().toBe(), toEqual(), toThrow()

# Advanced (Manual: #testing-advanced)
Property-based testing with fast-check
Mutation testing with Stryker
```

### Pattern 3: Cross-Reference Network

Rules reference each other:

```markdown
# security-guidelines.md
For database security, see #postgres-patterns

# postgres-patterns.md
For general security principles, see #security-guidelines
```

This creates a knowledge graph.

### Pattern 4: Workflow Enforcement

```markdown
# tdd-workflow.md
1. Write failing test (RED)
2. Write minimal code (GREEN)
3. Refactor (IMPROVE)
4. Verify coverage >80%

# verification-loop.md
After completing work:
1. Run tests
2. Check coverage
3. Lint code
4. Security scan
```

Workflows guide the AI through processes.

---

## Real-World Examples

### Example 1: Language-Specific Guidance

```markdown
---
inclusion: fileMatch
fileMatchPattern: '**/*.go'
priority: 75
---

# Go Development Patterns

## Error Handling
ALWAYS wrap errors with context:
```go
if err != nil {
  return fmt.Errorf("operation failed: %w", err)
}
```

## Interfaces
Accept interfaces, return structs:
```go
func Process(r io.Reader) (*Result, error)
```
```

**Result**: When editing Go files, AI automatically follows Go idioms.

### Example 2: Security Enforcement

```markdown
---
inclusion: always
priority: 100
---

# Security Guidelines

BEFORE ANY COMMIT:
- [ ] No hardcoded secrets
- [ ] All inputs validated
- [ ] SQL injection prevented
- [ ] XSS sanitization applied

Use environment variables:
```typescript
const apiKey = process.env.API_KEY
if (!apiKey) throw new Error('API_KEY required')
```
```

**Result**: Security checks happen automatically, every time.

### Example 3: Project-Specific Standards

```markdown
---
inclusion: always
priority: 90
---

# Project: E-Commerce Platform

## Architecture
- Microservices with gRPC
- PostgreSQL for transactional data
- Redis for caching
- Kafka for events

## File References
API Spec: #[[file:specs/api.proto]]
Database Schema: #[[file:schema.sql]]

## Testing
- 90% coverage minimum (critical project)
- E2E tests for checkout flow
- Load testing for 10k concurrent users
```

**Result**: AI understands project architecture and requirements.

### Example 4: Hook-Driven Workflow

```json
{
  "name": "TDD Enforcer",
  "when": {
    "type": "fileCreated",
    "patterns": ["src/**/*.ts"]
  },
  "then": {
    "type": "askAgent",
    "prompt": "A new file {{file}} was created. Following TDD, create a test file first at {{testFile}}, then implement the functionality."
  }
}
```

**Result**: Creating a new file automatically triggers test-first workflow.

---

## Advanced Concepts

### Dynamic Rule Generation

Rules can be generated programmatically:

```typescript
// Generate language-specific rules from templates
async function generateLanguageRules() {
  const languages = ['go', 'typescript', 'python', 'rust']
  
  for (const lang of languages) {
    const template = await loadTemplate('language-patterns')
    const rules = template.render({ language: lang })
    await writeFile(`.kiro/steering/${lang}-patterns.md`, rules)
  }
}
```

### Rule Composition

Combine multiple rules:

```markdown
---
inclusion: always
requires: [coding-standards, security-guidelines, tdd-workflow]
---

# Full Stack Development

This rule combines:
- Coding standards for consistency
- Security guidelines for safety
- TDD workflow for quality

Additional full-stack specific guidance:
- API design patterns
- Database optimization
- Frontend performance
```

### Conditional Logic in Rules

```markdown
---
inclusion: fileMatch
fileMatchPattern: '**/*.{ts,tsx}'
---

# TypeScript Patterns

{{#if isReactFile}}
## React-Specific
Use functional components with hooks
{{/if}}

{{#if isAPIFile}}
## API-Specific
Use Zod for validation
{{/if}}
```

### Metrics & Observability

Track steering effectiveness:

```typescript
class SteeringMetrics {
  trackRuleUsage(ruleId: string) {
    // How often is each rule loaded?
  }
  
  trackConflicts(rules: SteeringRule[]) {
    // Which rules conflict most often?
  }
  
  trackOutcomes(ruleId: string, success: boolean) {
    // Do certain rules lead to better outcomes?
  }
}
```

---

## Summary: The Complete Picture

### What You've Learned

1. **Philosophy**: Constraint-based creativity enables better AI output
2. **Architecture**: Five-layer system from user to model
3. **CCA Pattern**: Context, Command, Agent separation
4. **Injection**: How rules get into prompts
5. **Conflicts**: MCB priority resolution
6. **References**: File transclusion for DRY
7. **Events**: Hook-driven automation
8. **Implementation**: How to build it yourself

### Key Insights

- **Steering is a compiler**: Transforms project knowledge into AI context
- **Rules are composable**: Mix and match for different scenarios
- **Priority matters**: Workspace > Global > System
- **Events enable automation**: React to changes automatically
- **Transclusion prevents drift**: Single source of truth

### Building Your Own System

You now understand:
- **WHY**: Consistent, high-quality AI output
- **HOW**: Context injection, pattern matching, priority resolution
- **WHERE**: File organization and architecture
- **WHEN**: Event-driven triggers and lifecycle
- **WHO**: Separation of concerns (context, command, agent)

You can recreate this system from scratch with:
1. A markdown parser (front-matter + content)
2. A pattern matcher (glob patterns)
3. A priority resolver (conflict resolution)
4. A file reference expander (transclusion)
5. An event system (hooks)
6. A context builder (assembles final prompt)

---

## Next Steps

1. **Experiment**: Create your own steering rules
2. **Measure**: Track which rules improve outcomes
3. **Iterate**: Refine based on real usage
4. **Share**: Contribute patterns back to community
5. **Extend**: Add new event types, action types, inclusion modes

The steering system is a living framework that grows with your needs.
