# Setup Guide

This guide will help you set up the Cursor Rules System in your project for enhanced AI-assisted development.

## Prerequisites

- [Cursor IDE](https://cursor.sh/) installed
- Basic understanding of your project's architecture
- Familiarity with markdown files

## Installation

### 1. Copy the Rule System

Copy the entire `.cursor/` directory from this repository to your project root:

```
your-project/
├── .cursor/
│   ├── global.mdc
│   ├── cursor-memory-bank.mdc
│   ├── memory-bank/
│   ├── feature-rules/
│   ├── tech-stack/
│   └── mcp.json
├── src/
└── ...
```

### 2. Configure Memory Bank Core Files

The memory bank requires six core files. Start with these templates:

#### `memory-bank/projectbrief.mdc`
```markdown
---
description: Project foundation and core requirements
globs: 
alwaysApply: true
---

# Project Brief

## Project Name
[Your Project Name]

## Core Purpose
[What problem does this project solve?]

## Key Requirements
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

## Success Criteria
- [Criterion 1]
- [Criterion 2]
```

#### `memory-bank/productContext.mdc`
```markdown
---
description: Product vision and user experience goals
globs: 
alwaysApply: true
---

# Product Context

## Problem Statement
[What specific problem are you solving?]

## Target Users
[Who will use this product?]

## Core User Flows
1. [Primary user flow]
2. [Secondary user flow]

## Product Goals
- [Goal 1]
- [Goal 2]
```

#### `memory-bank/activeContext.mdc`
```markdown
---
description: Current work focus and session goals
globs: 
alwaysApply: true
---

# Active Context

## Current Sprint Goal
[What are you working on right now?]

## Recent Changes
- [Change 1]
- [Change 2]

## Next Steps
1. [Next action]
2. [Following action]

## Active Decisions
- [Decision or question currently being considered]
```

#### `memory-bank/systemPatterns.mdc`
```markdown
---
description: System architecture and design patterns
globs: 
alwaysApply: true
---

# System Patterns

## Architecture Overview
[High-level system architecture]

## Key Design Patterns
- [Pattern 1]: [Description]
- [Pattern 2]: [Description]

## Component Relationships
[How major components interact]

## Data Flow
[How data moves through the system]
```

#### `memory-bank/techContext.mdc`
```markdown
---
description: Technology stack and development setup
globs: 
alwaysApply: true
---

# Tech Context

## Technology Stack
- **Frontend**: [e.g., Next.js, React]
- **Backend**: [e.g., Node.js, Supabase]
- **Database**: [e.g., PostgreSQL]
- **Deployment**: [e.g., Vercel, AWS]

## Development Setup
[How to run the project locally]

## Key Dependencies
- [Dependency 1]: [Purpose]
- [Dependency 2]: [Purpose]

## Technical Constraints
- [Constraint 1]
- [Constraint 2]
```

#### `memory-bank/progress.mdc`
```markdown
---
description: Current status and progress tracking
globs: 
alwaysApply: true
---

# Progress

## [COMPLETE] Features
- [Completed feature 1]
- [Completed feature 2]

## [IN_PROGRESS] Current Work
- [Active work item 1]
- [Active work item 2]

## [PENDING] Next Steps
- [Planned work 1]
- [Planned work 2]

## Known Issues
- [Issue 1]: [Description]
- [Issue 2]: [Description]

## What Works Well
- [Working feature 1]
- [Working feature 2]
```

### 3. Configure MCP Integration (Optional)

If you use MCP servers, update `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "your-service": {
      "command": "your-command",
      "args": ["your-args"],
      "env": {
        "YOUR_ENV_VAR": "your-value"
      }
    }
  }
}
```

**⚠️ Important**: Remove any sensitive tokens or keys before sharing.

### 4. Customize Feature Rules

Review the `feature-rules/` directory and:
- Remove irrelevant feature files
- Customize existing rules for your project
- Add new feature rules as needed

### 5. Update Tech Stack Rules

Customize `tech-stack/` files for your specific technology choices:
- Add framework-specific patterns
- Document coding standards
- Include tool configurations

## First Use

### 1. Test the System

Open Cursor and try these commands:
- Type `/plan [your task]` to see the planning workflow
- Use `update memory bank` to trigger a system review
- Create a small feature to test the rule application

### 2. Verify Context Loading

Check that Cursor:
- Reads memory bank files at session start
- References appropriate feature rules
- Applies tech stack patterns consistently

### 3. Update Documentation

After your first successful session:
- Update `activeContext.mdc` with current focus
- Document any new patterns discovered
- Add project-specific insights to relevant files

## Common Setup Issues

### Rules Not Being Applied
- Ensure `.cursor/` is in your project root
- Check that `global.mdc` has `alwaysApply: true`
- Verify file extensions are `.mdc` for rule files

### Memory Bank Not Loading
- Confirm all six core memory bank files exist
- Check that files use proper markdown headers
- Ensure `cursor-memory-bank.mdc` is present

### Feature Rules Conflicts
- Review feature rule files for overlapping logic
- Consolidate duplicate patterns
- Use clear, specific rule descriptions

## Next Steps

Once setup is complete:
1. Read the [Workflow Guide](workflow-guide.md) to learn effective usage patterns
2. Explore the [Architecture Guide](architecture-guide.md) for deeper understanding
3. Check out [Examples](../examples/) for inspiration and templates

## Getting Help

- Review the [FAQ](faq.md) for common questions
- Check existing [Issues](https://github.com/your-repo/issues) for known problems
- Create a new issue with your setup question

---

**You're ready to start!** The system will learn and improve as you use it. Focus on keeping the memory bank updated and documenting patterns as you discover them.