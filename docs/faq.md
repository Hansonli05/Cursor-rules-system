# Frequently Asked Questions

Common questions and answers about the Cursor Rules System.

## General Questions

### What is the Cursor Rules System?

The Cursor Rules System is a hierarchical rule and context management system designed to enhance AI-assisted development with Cursor IDE. It provides structured context management, reusable patterns, and consistent AI behavior across development sessions.

### Why do I need this system?

- **Context Preservation**: AI memory resets between sessions - this system ensures consistency
- **Pattern Reuse**: Capture and reuse successful development patterns
- **Team Collaboration**: Consistent AI behavior across team members
- **Quality Assurance**: Enforce coding standards and architectural decisions
- **Knowledge Management**: Preserve institutional knowledge in readable format

### How is this different from just using .cursorrules?

The traditional `.cursorrules` file is a single file with mixed concerns. This system provides:
- **Hierarchical Organization**: Three-tier system prevents conflicts
- **Comprehensive Context**: Six core memory bank files ensure complete project understanding
- **Dynamic Updates**: Files evolve with your project
- **Cross-Domain Integration**: Rules reference and build upon each other
- **Status Tracking**: Built-in progress and status management

## Setup and Configuration

### Do I need to use all the files?

**Required files**:
- `global.mdc` and `cursor-memory-bank.mdc` (system operation)
- All six memory bank core files (project context)

**Optional files**:
- Feature rules (create as needed for your features)
- Tech stack rules (customize for your technology choices)
- Additional memory bank files (for complex projects)

### Can I use this with existing .cursorrules files?

Yes, but we recommend migrating to the new system for better organization. You can:
1. Start with the new system alongside existing .cursorrules
2. Gradually migrate patterns to appropriate rule domains
3. Remove old .cursorrules once migration is complete

### How do I customize it for my tech stack?

1. **Update `techContext.mdc`** with your specific technologies
2. **Modify tech-stack/ files** for your framework patterns
3. **Create new tech-stack files** for technologies not included
4. **Update MCP configuration** for your specific tools and services

## Usage and Workflow

### When should I use `/plan` mode?

Use `/plan` for:
- **New features** that affect multiple files
- **Complex refactoring** tasks
- **Architectural changes**
- **When you're unsure** about the approach
- **Cross-domain changes** affecting multiple systems

Don't use `/plan` for:
- Simple, single-file changes
- Well-understood, routine tasks
- Quick bug fixes
- Minor text or styling updates

### How often should I update the memory bank?

**Update frequency**:
- `activeContext.mdc`: Every development session
- `progress.mdc`: Weekly or after major milestones
- `systemPatterns.mdc`: When architectural decisions are made
- Other files: As project evolves

**Trigger for updates**:
- Completing major features
- Making architectural decisions
- Learning new patterns
- Before complex development phases

### What's the difference between feature rules and tech stack rules?

**Feature Rules** (`feature-rules/`):
- **What**: User-facing feature logic and patterns
- **Examples**: Authentication systems, payment processing, user management
- **Focus**: Business logic and user experience patterns
- **When to create**: When building reusable feature patterns

**Tech Stack Rules** (`tech-stack/`):
- **What**: Technology-specific implementation patterns
- **Examples**: React patterns, API design, database schemas
- **Focus**: How to implement with specific tools
- **When to create**: When adopting new technologies or discovering tech-specific patterns

## Common Issues

### AI isn't following my rules

**Troubleshooting steps**:
1. **Check file locations**: Ensure `.cursor/` is in project root
2. **Verify file extensions**: Use `.mdc` for rule files
3. **Check `alwaysApply: true`**: In `global.mdc` and `cursor-memory-bank.mdc`
4. **Review for conflicts**: Ensure no contradicting rules across domains
5. **Test with simple commands**: Start with basic requests to verify system is working

### Rules are conflicting with each other

**Resolution approach**:
1. **Identify conflicts**: Review rules across all three domains
2. **Establish hierarchy**: Memory bank > feature rules > tech stack
3. **Consolidate duplicates**: Merge similar patterns into single rules
4. **Be more specific**: Make rules more focused and less overlapping
5. **Document relationships**: Link related rules explicitly

### Memory bank files are getting too large

**Organization strategies**:
1. **Break into sub-files**: Create additional files in `memory-bank/` folder
2. **Focus on current**: Move historical information to archive files
3. **Use linking**: Reference other files instead of duplicating information
4. **Regular cleanup**: Remove outdated information periodically

### AI asks too many questions

This usually means:
1. **Insufficient context**: Add more detail to memory bank files
2. **Unclear requirements**: Be more specific in your requests
3. **Missing patterns**: Create feature rules for common tasks
4. **Complex requests**: Break large requests into smaller, focused tasks

## Advanced Usage

### Can I use this with teams?

**Team collaboration strategies**:
- **Shared memory bank**: Keep core memory bank files synchronized
- **Individual contexts**: Each developer can have personal `activeContext.mdc`
- **Collaborative rules**: Team contributes to feature and tech stack rules
- **Review process**: Establish process for rule updates and changes

### How do I migrate from other systems?

**Migration approach**:
1. **Start fresh**: Set up new system alongside existing
2. **Extract patterns**: Identify reusable patterns from existing code
3. **Document incrementally**: Add rules as you discover patterns
4. **Validate effectiveness**: Test new system on small features first
5. **Full transition**: Move to new system once confident

### Can I automate rule updates?

**Automation possibilities**:
- **CI/CD integration**: Update progress files on deployments
- **Git hooks**: Update active context on commits
- **Monitoring integration**: Update status based on system health
- **Template generation**: Auto-generate boilerplate rule files

## Troubleshooting

### System not loading context at session start

**Check these items**:
- `.cursor/` folder in project root
- `global.mdc` has correct headers and `alwaysApply: true`
- All six core memory bank files exist
- Files use proper markdown formatting
- No syntax errors in file headers

### Rules not being applied consistently

**Common causes**:
- Rules too generic or vague
- Conflicting rules across domains
- Missing context in memory bank
- AI overriding rules due to conflicting instructions

### Performance issues with large rule sets

**Optimization strategies**:
- Keep rule files focused and concise
- Archive outdated rules with `#ARCHIVED` tag
- Use specific file naming for quick identification
- Regular cleanup of unused patterns

## Getting Help

### Where can I get more help?

1. **Documentation**: Check all guide documents first
2. **GitHub Issues**: Report bugs or request features
3. **GitHub Discussions**: Ask questions and share experiences
4. **Examples**: Review example configurations for similar setups
5. **Community**: Connect with other users using the system

### How do I report bugs or request features?

**For bugs**:
- Include your configuration files (sanitized of sensitive data)
- Describe expected vs actual behavior
- Include steps to reproduce
- Mention your Cursor version and OS

**For features**:
- Describe the use case and problem
- Suggest potential solutions
- Include examples if relevant
- Explain how it would improve the system

### How can I contribute?

See our [Contributing Guide](../CONTRIBUTING.md) for:
- Types of contributions welcome
- Development setup process
- Pull request guidelines
- Code of conduct

---

**Still have questions?** Check our [GitHub Discussions](https://github.com/your-repo/discussions) or create a new issue.