# Frontmatter Examples Collection

A document organizing sub-agent Frontmatter writing examples by type.

---

## 📝 Frontmatter Structure

```yaml
---
name: agent-name              # Required: kebab-case format
description: "Brief description"    # Required: 10-500 characters
tools: Read,Grep,Bash         # Optional: comma-separated or omit (all tools allowed)
model: inherit                # Optional: inherit/sonnet/opus/haiku (default: inherit)
---
```

---

## 🔧 Specialist Type Examples

### Example 1: ESLint Enforcer (formatter)

```yaml
---
name: eslint-enforcer
description: "A professional formatter that automatically applies ESLint rules to JavaScript/TypeScript files and maintains consistent code style"
tools: Read,Write,Bash
model: haiku
---
```

**Explanation**:
- `name`: Clear and descriptive name
- `description`: Clearly expresses role and purpose
- `tools`: File read + write + tool execution (ESLint)
- `model`: haiku for simple tasks

---

### Example 2: Prettier Formatter

```yaml
---
name: prettier-formatter
description: "A formatter that automatically organizes code format using Prettier"
tools: Read,Write,Bash
model: haiku
---
```

**Explanation**:
- Simple and repetitive task
- haiku model for fast execution

---

### Example 3: Link Checker

```yaml
---
name: link-checker
description: "A tool that validates all links in Markdown files and reports broken links"
tools: Read,Bash,Grep
model: inherit
---
```

**Explanation**:
- `tools`: File read + search + external tool execution
- `model`: inherit (use default model)

---

### Example 4: Test Runner

```yaml
---
name: test-runner
description: "A test runner that executes project tests and reports results concisely"
tools: Read,Bash
model: haiku
---
```

**Explanation**:
- Simple test execution and reporting
- Read for checking configuration files

---

## 📊 Analyst Type Examples

### Example 1: Code Reviewer (comprehensive review)

```yaml
---
name: code-reviewer
description: "A professional code reviewer that comprehensively validates TypeScript and Python code quality, security, performance, and maintainability"
tools: Read,Grep,Bash
model: sonnet
---
```

**Explanation**:
- `description`: Clearly specifies analysis scope (quality, security, performance, maintainability)
- `tools`: Read + search + tool execution (analysis only, no Write!)
- `model`: sonnet for complex analysis

---

### Example 2: Security Auditor (security audit)

```yaml
---
name: security-auditor
description: "A professional security analyst that identifies application security vulnerabilities and evaluates them based on OWASP standards"
tools: Read,Grep,Bash
model: sonnet
---
```

**Explanation**:
- Security-focused analysis
- OWASP standards specified
- Precise analysis with sonnet model

---

### Example 3: Performance Analyzer

```yaml
---
name: performance-analyzer
description: "A performance analysis specialist that identifies code performance bottlenecks and suggests optimization approaches"
tools: Read,Grep,Bash
model: sonnet
---
```

**Explanation**:
- Performance analysis specialized
- Includes optimization suggestions

---

### Example 4: TypeScript Reviewer (language-specific)

```yaml
---
name: typescript-reviewer
description: "A TypeScript specialist reviewer that validates TypeScript code type safety, pattern usage, and best practices"
tools: Read,Grep,Bash
model: inherit
---
```

**Explanation**:
- Language-specific reviewer
- inherit model (general case)

---

## 🎯 Orchestrator Type Examples

### Example 1: Release Manager (release management)

```yaml
---
name: release-manager
description: "A release manager that manages the entire npm package release process and automates from version updates to deployment"
tools: Read,Write,Bash
model: sonnet
---
```

**Explanation**:
- `description`: Clearly specifies management scope (version update → deployment)
- `tools`: File read/write + command execution
- `model`: sonnet for complex workflow

---

### Example 2: CI/CD Manager

```yaml
---
name: ci-cd-manager
description: "An automation manager that orchestrates CI/CD pipeline and manages the entire build, test, and deployment process"
tools: Read,Write,Bash,Grep
model: sonnet
---
```

**Explanation**:
- Entire pipeline management
- Grep added (for log search, etc.)

---

### Example 3: Production Deployment Manager (production deployment)

```yaml
---
name: production-deployment-manager
description: "A deployment manager that manages the entire production environment deployment process and performs safety checks and rollback strategies"
tools: Read,Write,Bash
model: opus
---
```

**Explanation**:
- Importance of production deployment
- opus model (highest quality)
- Mention of safety checks and rollback

---

### Example 4: Database Migration Coordinator

```yaml
---
name: db-migration-coordinator
description: "A migration coordinator that orchestrates database migrations and manages schema changes, data migration, and rollback"
tools: Read,Write,Bash
model: sonnet
---
```

**Explanation**:
- Database-specific workflow
- Schema changes + data migration + rollback

---

## 🔍 Model Selection Patterns

### haiku usage examples

```yaml
# Simple tasks
model: haiku

# Suitable for:
# - Formatting (prettier, eslint)
# - Link validation
# - Simple test execution
# - When quick feedback is important
```

---

### inherit usage examples (default)

```yaml
# General tasks
model: inherit

# Suitable for:
# - Most agents
# - No special requirements
# - Maintain consistency with main conversation
```

---

### sonnet usage examples

```yaml
# Complex analysis/workflow
model: sonnet

# Suitable for:
# - Code review
# - Security audit
# - Complex workflow
# - When high quality is needed
```

---

### opus usage examples

```yaml
# Very important tasks
model: opus

# Suitable for:
# - Production deployment
# - Critical system audit
# - Big impact on failure
# - Highest quality essential
```

---

## 🔒 Tools Selection Patterns

### Read-only (analysis tasks)

```yaml
tools: Read,Grep

# Suitable for:
# - Pure analysis tasks
# - Code review
# - Security audit (read-only)
```

---

### Read + tool execution

```yaml
tools: Read,Grep,Bash

# Suitable for:
# - Analysis + validation tool execution
# - Code review + test execution
# - Security audit + vulnerability scanning
```

---

### File modification included

```yaml
tools: Read,Write,Bash

# Suitable for:
# - Formatting
# - Automatic code fixing
# - File creation/update
```

---

### Comprehensive tools (workflow)

```yaml
tools: Read,Write,Bash,Grep

# Suitable for:
# - Complex workflow
# - Release management
# - CI/CD pipeline
```

---

### All tools allowed (not recommended)

```yaml
# All tools allowed when tools field omitted
---
name: example
description: "Description"
# No tools field = all tools allowed
---

# Reasons:
# - Security risk
# - Excessive permissions
# - Violates principle of least privilege
```

---

## ✅ Frontmatter Writing Checklist

Pre-creation checklist:

- [ ] `name`: Is it in kebab-case format? (e.g., `code-reviewer`)
- [ ] `name`: Is it in the 3-50 character range?
- [ ] `description`: Is it in the 10-500 character range?
- [ ] `description`: Are role and purpose clear?
- [ ] `tools`: Includes only minimum tools necessary for role?
- [ ] `tools`: Doesn't include Write/Edit for analysis tasks?
- [ ] `model`: Is it an appropriate model for type and complexity?
- [ ] `model`: Considered cost-effectiveness?

---

## ❌ Common Mistakes

### Mistake 1: Vague description

```yaml
# ❌ Bad Example
description: "Improves code"

# ✅ Good Example
description: "An analyst that analyzes TypeScript code performance and security and suggests specific improvements"
```

---

### Mistake 2: Excessive tool permissions

```yaml
# ❌ Bad Example (Write permission for analysis task)
name: code-reviewer
tools: Read,Write,Bash

# ✅ Good Example (read-only)
name: code-reviewer
tools: Read,Grep,Bash
```

---

### Mistake 3: Inappropriate model selection

```yaml
# ❌ Bad Example (opus for simple task)
name: prettier-formatter
model: opus

# ✅ Good Example (haiku for simple task)
name: prettier-formatter
model: haiku
```

---

### Mistake 4: Allowing all tools

```yaml
# ❌ Bad Example (tools field omitted)
---
name: code-reviewer
description: "Code reviewer"
# No tools field = all tools allowed
---

# ✅ Good Example (specify only necessary tools)
---
name: code-reviewer
description: "Code reviewer"
tools: Read,Grep,Bash
---
```

---

## 📖 References

- **Type System**: `../type-system.md`
- **Model Selection Guide**: `../model-selection-guide.md`
- **Tool Guide**: `../available-tools.md`
- **Validation Criteria**: `../validation-criteria.md`
- **Templates**: `../templates/` directory
