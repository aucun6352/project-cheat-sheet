# Sub-Agent Best Practices

Best practices and writing tips for creating effective and safe sub-agents.

---

## ✅ Do's (Recommendations)

### 1. Clear Role Definition

**Principle**: You should be able to describe what the agent does in one sentence.

**Good Example**:
```markdown
"A professional reviewer that comprehensively validates the quality, security, and performance of TypeScript and Python code"
```

**Bad Example**:
```markdown
"Improves code"  # Too vague
"Performs various tasks"  # Unclear role
```

**Checklist**:
- [ ] Is the role clearly described in one sentence?
- [ ] Is the area of expertise specifically stated?
- [ ] Are inputs and outputs clear?

---

### 2. Specific Trigger Conditions

**Principle**: Clearly and specifically define when the agent will be executed.

**Good Example**:
```markdown
## Triggers
- PR creation or update
- User requests "review this code" or "code review"
- Files with .ts, .tsx, .py extensions are modified
- Commits to main/develop branches
- Explicit: "Use the code-reviewer subagent"
```

**Bad Example**:
```markdown
## Triggers
- Code-related tasks
- When needed
- When user requests
```

**Checklist**:
- [ ] Are there 3-5 specific scenarios?
- [ ] Are file extensions or branch names specified?
- [ ] Are user command patterns included?

---

### 3. Principle of Least Privilege

**Principle**: Allow only the minimum tools necessary to perform the role.

**Good Example**:
```yaml
# Analysis agent (read-only)
tools: Read,Grep

# Analysis + validation tool execution
tools: Read,Grep,Bash

# Formatting (file modification needed)
tools: Read,Write,Bash
```

**Bad Example**:
```yaml
# Write permission for analysis agent
name: code-reviewer
tools: Read,Write,Bash  # Write unnecessary!

# All tools allowed
# tools field omitted (all tools allowed)
```

**Checklist**:
- [ ] Does analysis work use only Read, Grep?
- [ ] Is Write/Edit allowed only when truly necessary?
- [ ] Is it clear which commands Bash will execute?

---

### 4. Clear Boundary Setting

**Principle**: Clearly define Will/Will Not to limit agent behavior.

**Good Example**:
```markdown
## Boundaries

**Will:**
- Analyze code thoroughly across all quality dimensions
- Provide specific, actionable feedback with code examples
- Reference official documentation and best practices
- Suggest improvements with technical rationale
- Identify security vulnerabilities and performance issues

**Will Not:**
- Modify code directly (analysis only, no auto-fix)
- Execute potentially dangerous code
- Access external APIs without permission
- Review binary files or compiled output
- Make changes without explicit user approval
```

**Bad Example**:
```markdown
## Boundaries

**Will:**
- Analyze code

**Will Not:**
- Bad things
```

**Checklist**:
- [ ] Are there 3-5 Will items?
- [ ] Are there 2-4 Will Not items?
- [ ] Are security considerations included?

---

### 5. Structured Output Format

**Principle**: Provide consistent output format to improve user experience.

**Good Example**:
```markdown
## Output Format
\`\`\`
📋 Code Review Report

## Summary
- Files Reviewed: {count}
- Total Issues: {count}
- Critical: {count} | High: {count} | Medium: {count} | Low: {count}

## Critical Issues 🚨

### [Line 45-52] SQL Injection Risk
**Issue**: User input directly concatenated
**Severity**: Critical
**Fix**: Use parameterized queries

## Recommendations
1. Address critical security issues immediately
2. Consider performance optimizations
3. Add comprehensive error handling
\`\`\`
```

**Bad Example**:
```markdown
## Output Format
Output results
```

**Checklist**:
- [ ] Are specific output examples provided?
- [ ] Is the structure clear and easy to read?
- [ ] Are emojis or markdown appropriately used?

---

### 6. Type-Specific Structure

**Principle**: Use structure appropriate for Specialist/Analyst/Orchestrator.

**Specialist**:
```markdown
- 100-300 words
- 3-4 step Behavioral Guidelines
- Concise Output Format
- Fast execution (<30 seconds)
```

**Analyst**:
```markdown
- 300-800 words
- 4-5 Phase Analysis Process
- Severity classification system
- Structured reports
```

**Orchestrator**:
```markdown
- 800-2000+ words
- 4-6 Phase Workflow
- Detailed Error Handling
- Rollback mechanisms
```

---

### 7. Error Handling and Validation

**Principle**: Detailed error handling is essential especially for Orchestrators.

**Good Example** (Orchestrator):
```markdown
## Error Handling

### Build Failures
- **Action**: ABORT release immediately
- **Recovery**: Report errors with stack trace
- **Notification**: Alert team via configured channel

### Deployment Failures
- **Action**: Automatic rollback to previous version
- **Recovery**: Restore from backup
- **Notification**: Critical alert to on-call team

### Test Failures
- **Action**: ABORT, fix tests before retry
- **Recovery**: Provide detailed test report
- **Notification**: Standard notification
```

**Checklist**:
- [ ] Are 3-5 major error scenarios defined?
- [ ] Is there a recovery strategy for each error?
- [ ] Is a rollback mechanism included? (Orchestrator)

---

### 8. Type and Model Alignment

**Principle**: Choose a model appropriate for task complexity.

| Type | Recommended Model | Reason |
|------|-------------------|--------|
| Specialist (simple) | haiku | Fast and cheap |
| Specialist (complex) | inherit | Appropriate performance |
| Analyst | sonnet | Precise analysis needed |
| Orchestrator | sonnet/opus | Complex orchestration needed |

**Checklist**:
- [ ] Not using opus for simple tasks?
- [ ] Not using haiku for complex analysis?

---

## ❌ Don'ts (Things to Avoid)

### 1. Vague Roles

**❌ Bad Example**:
```markdown
description: "Improves code"
```

**✅ Good Example**:
```markdown
description: "An analyst that analyzes TypeScript code performance and security and suggests specific improvements"
```

---

### 2. Excessive Permissions

**❌ Bad Example**:
```yaml
# Write permission for analysis agent
name: security-auditor
tools: Read,Write,Bash
```

**✅ Good Example**:
```yaml
# Analysis is read-only
name: security-auditor
tools: Read,Grep,Bash
```

---

### 3. Unclear Triggers

**❌ Bad Example**:
```markdown
## Triggers
- Code-related tasks
- When needed
```

**✅ Good Example**:
```markdown
## Triggers
- PR creation or update
- Files with .ts, .py extensions modified
- User requests "review this code"
```

---

### 4. Missing Error Handling

**❌ Bad Example** (Orchestrator):
```markdown
# No error handling section
```

**✅ Good Example**:
```markdown
## Error Handling
- Build Failures: ABORT release
- Deployment Failures: Auto rollback
- Test Failures: ABORT, fix tests
```

---

### 5. Deployment Without Testing

**❌ Bad Example**:
- Production use immediately after creation
- No trigger condition validation

**✅ Good Example**:
- Test after creation
- Verify trigger conditions work correctly
- Validate in multiple scenarios

---

### 6. Allowing All Tools

**❌ Bad Example**:
```yaml
# tools field omitted (all tools allowed)
---
name: example
description: "Description"
---
```

**✅ Good Example**:
```yaml
# Specify only necessary tools
---
name: example
description: "Description"
tools: Read,Grep,Bash
---
```

---

## 💡 Type-Specific Writing Tips

### Specialist Writing Tips

**Core Principles**: Simplicity, speed, clarity

✅ **Do's**:
- Focus on one task only
- 3-4 step simple process
- Fast execution (< 30 seconds)
- Use minimum tools only

❌ **Don'ts**:
- Complex workflows (→ Orchestrator)
- Multi-task handling
- Excessive analysis (→ Analyst)

**Example Checklist**:
- [ ] Is it within 100-300 words?
- [ ] Does it have a single clear purpose?
- [ ] Is there a simple 3-4 step process?

---

### Analyst Writing Tips

**Core Principles**: Comprehensive analysis, constructive feedback, read-only

✅ **Do's**:
- Comprehensive analysis from multiple perspectives
- Specific feedback with examples
- Use severity classification system
- Provide reference documentation

❌ **Don'ts**:
- Direct code modification (read-only!)
- Abstract or vague feedback
- Execute or run tests

**Example Checklist**:
- [ ] Is it within 300-800 words?
- [ ] Is there a 4-5 phase analysis process?
- [ ] Is there a severity classification system?
- [ ] Not using Write/Edit tools?

---

### Orchestrator Writing Tips

**Core Principles**: Orchestration, error handling, safety

✅ **Do's**:
- Clearly define multi-phase workflow
- Detailed error handling and rollback
- Validation conditions for each phase
- Real-time progress reporting

❌ **Don'ts**:
- Missing error handling
- No rollback mechanism
- Proceeding to next step without validation

**Example Checklist**:
- [ ] Is it within 800-2000+ words?
- [ ] Is there a 4-6 phase workflow?
- [ ] Are error handling strategies defined?
- [ ] Is a rollback mechanism included?

---

## 🔒 Security Best Practices

### 1. Principle of Least Privilege

```yaml
# ✅ Good Example
tools: Read,Grep  # Only analysis needed

# ❌ Bad Example
# tools field omitted (all tools allowed)
```

---

### 2. Prohibit Dangerous Commands

```markdown
## Boundaries

**Will Not:**
- Execute `rm -rf` or destructive commands
- Run commands with `sudo` or elevated privileges
- Access environment variables containing secrets
- Modify critical system files
```

---

### 3. Input Validation

```markdown
## Behavioral Guidelines
1. **Validate**: Check file paths before operations
2. **Sanitize**: Escape special characters in user input
3. **Verify**: Confirm critical operations before execution
```

---

### 4. Safety Checks (Orchestrator)

```markdown
## Safety Checks
- Verify git status before release
- Check for uncommitted changes
- Validate environment before deployment
- Confirm backup exists before migration
```

---

## 📊 Quality Improvement Tips

### 1. Target 90+ Validation Score

**Tips for high scores**:
- Include all required sections
- Specific and detailed descriptions
- Word count appropriate for type
- 3+ trigger conditions
- Structured output format

---

### 2. Practical Testing

**Test Scenarios**:
1. Trigger condition validation
2. Test with various inputs
3. Verify error scenarios
4. Validate output format

---

### 3. Iterative Improvement

**Improvement Process**:
1. Check validation report after creation
2. Apply "Improvable areas"
3. Collect feedback from actual use
4. Continuously update

---

## 🎯 Practical Examples

### Example 1: Good Specialist

```yaml
---
name: eslint-enforcer
description: "A professional formatter that automatically applies ESLint rules to JavaScript/TypeScript files and maintains consistent code style"
tools: Read,Write,Bash
model: haiku
---

# ESLint Enforcer

Focuses on simple and clear tasks.

## Role
Execute ESLint to automatically fix code style.

## Triggers
- User mentions "lint", "eslint", "format"
- .ts, .tsx, .js, .jsx files modified
- Explicit: "run eslint"

## Behavioral Guidelines
1. **Read**: Read file to understand current state
2. **Execute**: Run `npx eslint --fix {file}`
3. **Report**: Report fixes and remaining issues

## Output Format
✅ ESLint Results
- Auto-Fixed: 12 issues
- Remaining: 1 warning

## Boundaries
**Will:** Run eslint --fix, Report results
**Will Not:** Modify .eslintrc, Ignore errors
```

**Why it's good**:
- Single clear purpose
- 3-step process
- Minimum necessary tools
- Fast model (haiku)

---

### Example 2: Good Analyst

```yaml
---
name: security-auditor
description: "A professional security analyst that identifies application security vulnerabilities and evaluates them based on OWASP standards"
tools: Read,Grep,Bash
model: sonnet
---

# Security Auditor

Comprehensively analyzes security vulnerabilities.

## Role
Perform security audit based on OWASP Top 10.

## Expertise Areas
- OWASP Top 10 vulnerabilities
- Authentication and authorization
- Data encryption and protection
- Input validation and sanitization

## Triggers
- Security review requests
- Pre-deployment checks
- Code changes in auth/security modules

## Analysis Process
1. **Scan**: Search for SQL injection, XSS, CSRF
2. **Analyze**: Review authentication/authorization logic
3. **Review**: Verify data handling and encryption
4. **Report**: Classify by risk level

## Output Format
🔒 Security Audit Report
## Risk Summary
Critical: {count} | High: {count}

## Critical Vulnerabilities
[Details]

## Recommendations
[Improvements by priority]

## Analysis Standards
- **Critical**: Immediate action required
- **High**: Fix recommended before deployment
- **Medium**: Planned improvement
- **Low**: For reference

## Boundaries
**Will:** Identify vulnerabilities, assess risks, suggest improvements
**Will Not:** Modify code, access production data
```

**Why it's good**:
- Comprehensive analysis
- Severity classification
- Read-only (no Write)
- Appropriate model (sonnet)

---

## 📖 References

- **Type System**: `type-system.md`
- **Model Selection Guide**: `model-selection-guide.md`
- **Tool Guide**: `available-tools.md`
- **Validation Criteria**: `validation-criteria.md`
- **Frontmatter Examples**: `examples/frontmatter-examples.md`
