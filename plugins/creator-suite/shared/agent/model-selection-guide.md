# Sub-Agent Model Selection Guide

A document listing available models for sub-agents and their characteristics, pros/cons, and recommended use cases.

---

## 🤖 Available Models

| Model | Performance | Speed | Cost | Recommended Type |
|-------|-------------|-------|------|------------------|
| **inherit** | Variable | Variable | Variable | All types (default) |
| **haiku** | Basic | ⚡ Fast | 💰 Cheap | Specialist |
| **sonnet** | Excellent | 🔄 Normal | 💰💰 Normal | Analyst, Orchestrator |
| **opus** | Best | 🐌 Slow | 💰💰💰 Expensive | Critical Orchestrator |

---

## 📚 Model Detailed Descriptions

### inherit (default, recommended)

**Description**: Uses the same model as the main conversation.

**Pros**:
- ✅ Consistency: Maintains same quality as main conversation
- ✅ Easy setup: No need for separate model selection
- ✅ Easy maintenance: Automatically reflects model changes

**Cons**:
- ⚠️ Model change impact: Sub-agents affected when main model changes
- ⚠️ No fine control: Difficult to optimize per agent

**Use Cases**:
- Most sub-agents
- When there are no special performance requirements
- Fast prototyping

**Recommended Agent Types**:
- ✅ Specialist (most)
- ✅ Analyst (most)
- ✅ Orchestrator (general cases)

**Example**:
```yaml
---
name: code-formatter
description: "Code formatting tool"
tools: Read,Write,Bash
model: inherit  # Use same model as main conversation
---
```

---

### haiku (fast response)

**Description**: Claude Haiku - Fastest and most economical model

**Pros**:
- ⚡ Fast response time
- 💰 Low cost
- ✅ Suitable for simple tasks

**Cons**:
- ⚠️ Lower reasoning quality
- ⚠️ Difficult to handle complex tasks
- ⚠️ May miss subtle context

**Use Cases**:
- Simple and repetitive tasks
- Clear rule-based tasks
- When quick feedback is important

**Recommended Agent Types**:
- ✅ Specialist (formatter, linter runner)
- ⚠️ Analyst (simple validation only)
- ❌ Orchestrator (too complex, unsuitable)

**Suitable Agent Examples**:
- `prettier-formatter`: Simple formatting
- `link-checker`: Link validation
- `test-runner`: Test execution and reporting

**Unsuitable Agent Examples**:
- `code-reviewer`: Complex analysis needed
- `security-auditor`: Subtle vulnerability detection needed
- `release-manager`: Multi-phase workflow orchestration

**Example**:
```yaml
---
name: prettier-formatter
description: "Automatically format code using Prettier"
tools: Read,Write,Bash
model: haiku  # haiku sufficient for simple tasks
---
```

---

### sonnet (balanced performance)

**Description**: Claude Sonnet 4.5 - Balance of performance and speed

**Pros**:
- ✅ Excellent reasoning ability
- ✅ Can handle complex tasks
- ✅ Reasonable response time
- ✅ Suitable for most tasks

**Cons**:
- ⚠️ Slower than haiku
- ⚠️ More expensive than haiku

**Use Cases**:
- Complex analysis tasks
- Multi-phase workflows
- Tasks requiring high quality
- When context understanding is important

**Recommended Agent Types**:
- ✅ Analyst (all analysis tasks)
- ✅ Orchestrator (complex workflows)
- ⚠️ Specialist (complex cases only)

**Suitable Agent Examples**:
- `code-reviewer`: Comprehensive code review
- `security-auditor`: Security vulnerability analysis
- `performance-analyzer`: Performance bottleneck analysis
- `deployment-coordinator`: Deployment workflow orchestration

**Example**:
```yaml
---
name: code-reviewer
description: "Comprehensive validation of TypeScript and Python code quality, security, and performance"
tools: Read,Grep,Bash
model: sonnet  # sonnet recommended for complex analysis
---
```

---

### opus (highest quality)

**Description**: Claude Opus - Highest performance model

**Pros**:
- 🏆 Highest level reasoning ability
- ✅ Handles very complex tasks
- ✅ Perfect understanding of subtle context
- ✅ Highest quality output

**Cons**:
- 🐌 Slowest response time
- 💰💰💰 High cost
- ⚠️ Excessive for most tasks

**Use Cases**:
- Very important and complex workflows
- Tasks with significant impact on failure
- When highest quality is essential

**Recommended Agent Types**:
- ⚠️ Analyst (generally unnecessary)
- ✅ Orchestrator (very important cases only)
- ❌ Specialist (excessive)

**Suitable Agent Examples**:
- `production-release-manager`: Production release management
- `critical-security-auditor`: Critical system security audit
- `architecture-reviewer`: System architecture review

**Unsuitable Agent Examples**:
- Most general tasks (low cost-effectiveness)

**Example**:
```yaml
---
name: production-release-manager
description: "Manage entire production environment release process"
tools: Read,Write,Bash
model: opus  # opus used for very important tasks
---
```

---

## 🎯 Recommended Models by Type

### Specialist (single task specialist)

**General Recommendation**: `inherit` or `haiku`

| Task Complexity | Recommended Model | Examples |
|----------------|-------------------|----------|
| Simple | haiku | prettier-formatter, link-checker |
| Normal | inherit | eslint-enforcer, test-runner |
| Complex | sonnet | advanced-refactorer |

```yaml
# Simple formatter
model: haiku

# General tool runner
model: inherit

# Complex refactoring tool
model: sonnet
```

---

### Analyst (analysis and review)

**General Recommendation**: `inherit` or `sonnet`

| Analysis Depth | Recommended Model | Examples |
|---------------|-------------------|----------|
| Surface | inherit | simple-validator |
| Medium | sonnet | code-reviewer |
| Deep | sonnet | security-auditor, architecture-reviewer |

```yaml
# General code review
model: inherit

# Comprehensive analysis
model: sonnet

# Security audit (high quality needed)
model: sonnet
```

**Note**: Not recommended to use haiku for Analyst (analysis quality degradation)

---

### Orchestrator (complex workflow)

**General Recommendation**: `sonnet` or `opus`

| Workflow Importance | Recommended Model | Examples |
|--------------------|-------------------|----------|
| General | sonnet | ci-cd-coordinator |
| Important | sonnet | release-manager |
| Very Important | opus | production-deployment-manager |

```yaml
# General CI/CD
model: sonnet

# Release management
model: sonnet

# Production deployment (big impact on failure)
model: opus
```

**Note**: Not recommended to use haiku for Orchestrator (difficult to handle complexity)

---

## 🔀 Model Selection Decision Tree

```
┌─────────────────────────┐
│ What is the agent type? │
└───────┬─────────────────┘
        │
   ┌────┴────┐
   │         │
Specialist  │
   │         │
   ├─ Simple task? → haiku
   ├─ Normal task? → inherit
   └─ Complex task? → sonnet
             │
        Analyst
             │
             ├─ Surface analysis? → inherit
             ├─ Medium analysis? → sonnet
             └─ Deep analysis? → sonnet
             │
        Orchestrator
             │
             ├─ General workflow? → sonnet
             ├─ Important workflow? → sonnet
             └─ Very important? → opus
```

---

## 💡 Model Selection Tips

### ✅ Do's

1. **Use Default**: Use `inherit` unless there's a special reason
2. **Performance Test**: Test with multiple models and choose optimally
3. **Consider Cost**: Consider in order haiku → inherit → sonnet → opus
4. **Evaluate Task Complexity**: Choose model appropriate for task

### ❌ Don'ts

1. **Excessive Model**: Don't use opus for simple tasks
2. **Insufficient Model**: Don't use haiku for complex analysis
3. **Indiscriminate opus**: Don't choose opus without considering cost-effectiveness
4. **Unvalidated Choice**: Don't select model without testing

---

## 📊 Model Comparison Matrix

| Requirement | haiku | inherit | sonnet | opus |
|-------------|-------|---------|--------|------|
| Fast response | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| High quality | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Low cost | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| Complex reasoning | ⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Simple tasks | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Complex tasks | ⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 🔧 Practical Examples

### Example 1: ESLint Formatter (Specialist)

```yaml
---
name: eslint-enforcer
description: "Automatically apply ESLint rules"
tools: Read,Write,Bash
model: haiku  # ← haiku sufficient for simple and repetitive tasks
---
```

**Selection Reason**:
- Task is clear and simple (run eslint --fix)
- Quick feedback important
- No complex reasoning needed

---

### Example 2: Code Reviewer (Analyst)

```yaml
---
name: code-reviewer
description: "Comprehensive code quality validation"
tools: Read,Grep,Bash
model: sonnet  # ← sonnet for complex analysis
---
```

**Selection Reason**:
- Multi-angle analysis needed (security, performance, quality)
- Context understanding important
- Subtle problem detection needed

---

### Example 3: Release Manager (Orchestrator)

```yaml
---
name: release-manager
description: "Manage entire release process"
tools: Read,Write,Bash
model: sonnet  # ← sonnet for complex workflow
---
```

**Selection Reason**:
- Multi-phase workflow orchestration
- Error handling and rollback needed
- High reliability needed

---

### Example 4: Production Deployment (Orchestrator)

```yaml
---
name: production-deployment-manager
description: "Manage entire production environment deployment process"
tools: Read,Write,Bash
model: opus  # ← opus for very important tasks with high failure impact
---
```

**Selection Reason**:
- Importance of production environment
- Big impact on failure
- Highest quality judgment needed

---

## 📖 References

- **Type System**: `type-system.md`
- **Tool Guide**: `available-tools.md`
- **Validation Criteria**: `validation-criteria.md`
- **Templates**: `templates/` directory
