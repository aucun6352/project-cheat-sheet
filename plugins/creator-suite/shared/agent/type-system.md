# Sub-Agent Type System

Detailed description and selection guide for the three sub-agent types (Specialist, Analyst, Orchestrator).

---

## 📊 Type Comparison Summary

| Type | Complexity | Word Count | Use Case | Recommended Model |
|------|-----------|-----------|----------|-------------------|
| **Specialist** | Low | 100-300 | Single task expert | inherit, haiku |
| **Analyst** | Medium | 300-800 | Analysis/Review | inherit, sonnet |
| **Orchestrator** | High | 800-2000+ | Complex workflows | sonnet, opus |

---

## 🔧 Specialist

### Overview

**Purpose**: Expert agent specialized in a single task

**Characteristics**:
- ✅ Performs one task quickly and accurately
- ✅ Clear input → process → output flow
- ✅ Simple prompt structure
- ✅ Fast execution time

**Recommended Word Count**: 100-300 words

**Recommended Model**: `inherit` or `haiku`

---

### Suitable Use Cases

- ✅ Code formatting (ESLint, Prettier, Black)
- ✅ Link validation
- ✅ Test execution
- ✅ File cleanup
- ✅ Simple transformation tasks
- ✅ Quick validation tasks

---

### Unsuitable Use Cases

- ❌ Code review (Use Analyst)
- ❌ Deployment process (Use Orchestrator)
- ❌ Multi-step analysis
- ❌ Complex decision making

---

### Prompt Structure

```markdown
---
name: {agent-name}
description: "{brief description}"
tools: {minimum_required_tools}
model: inherit
---

# {Agent Name}

{1-2 sentence role description}

## Role
{specific role definition}

## Triggers
{3-5 trigger conditions}

## Behavioral Guidelines
1. **{Step1}**: {description}
2. **{Step2}**: {description}
3. **{Step3}**: {description}

## Output Format
```
{output example}
```

## Boundaries

**Will:**
- {tasks to perform}

**Will Not:**
- {prohibited tasks}
```

---

### Real Example: ESLint Enforcer

```yaml
---
name: eslint-enforcer
description: "Automatically apply ESLint rules to JavaScript/TypeScript files"
tools: Read,Write,Bash
model: haiku
---
```

**Complexity**: Low
**Word Count**: Approximately 200 words
**Execution Time**: < 30 seconds

---

## 📊 Analyst

### Overview

**Purpose**: Expert agent that performs code analysis, review, and evaluation

**Characteristics**:
- ✅ Comprehensive analysis from multiple perspectives
- ✅ Provides specific feedback and examples
- ✅ Classification by severity
- ✅ Constructive improvement suggestions

**Recommended Word Count**: 300-800 words

**Recommended Model**: `inherit` or `sonnet`

---

### Suitable Use Cases

- ✅ Code review
- ✅ Security audit
- ✅ Performance analysis
- ✅ Architecture evaluation
- ✅ Test coverage review
- ✅ Documentation quality assessment

---

### Unsuitable Use Cases

- ❌ Automatic code fixes (Use Specialist)
- ❌ Complex deployment (Use Orchestrator)
- ❌ Simple formatting (Use Specialist)

---

### Prompt Structure

```markdown
---
name: {agent-name}
description: "{analysis role description}"
tools: Read,Grep,Bash
model: sonnet
---

# {Agent Name}

{2-3 sentence expertise description}

## Role
{detailed role and responsibilities}

## Expertise Areas
- {expertise_area_1}
- {expertise_area_2}
- {expertise_area_3}
- {expertise_area_4}

## Triggers
{automatic execution conditions}

## Analysis Process
1. **Phase 1**: {description}
2. **Phase 2**: {description}
3. **Phase 3**: {description}
4. **Phase 4**: {description}

## Output Format
```
{structured analysis result}
```

## Analysis Standards
- **Critical**: {criteria}
- **High**: {criteria}
- **Medium**: {criteria}
- **Low**: {criteria}

## Boundaries

**Will:**
- {analysis to perform}

**Will Not:**
- {prohibited tasks (no modifications)}
```

---

### Real Example: Code Reviewer

```yaml
---
name: code-reviewer
description: "Comprehensive verification of TypeScript and Python code quality, security, and performance"
tools: Read,Grep,Bash
model: sonnet
---
```

**Complexity**: Medium
**Word Count**: Approximately 600 words
**Analysis Depth**: 4-phase process

**Key Features**:
- Multi-phase analysis process
- Issue classification by severity
- Provides specific code examples
- Read-only (does not modify)

---

## 🎯 Orchestrator

### Overview

**Purpose**: Advanced agent that orchestrates complex multi-step workflows

**Characteristics**:
- ✅ Workflow composed of multiple phases
- ✅ Orchestrates various tools
- ✅ Error handling and rollback
- ✅ Safety checks and validation
- ✅ Detailed progress reporting

**Recommended Word Count**: 800-2000+ words

**Recommended Model**: `sonnet` or `opus`

---

### Suitable Use Cases

- ✅ CI/CD pipeline
- ✅ Release management
- ✅ Deployment process
- ✅ Data migration
- ✅ System initialization
- ✅ Complex test scenarios

---

### Unsuitable Use Cases

- ❌ Simple formatting (Use Specialist)
- ❌ Single analysis (Use Analyst)

---

### Prompt Structure

```markdown
---
name: {agent-name}
description: "{workflow description}"
tools: Read,Write,Bash
model: sonnet
---

# {Agent Name}

{2-3 sentence orchestration role description}

## Role
{detailed role and management scope}

## Responsibilities
{5-8 responsibility items}

## Triggers
{workflow initiation conditions}

## Workflow Phases

### Phase 1: {Phase name}
{detailed description}

**Steps:**
1. {step}
2. {step}

**Validation:**
- {validation condition}

**Error Handling:**
- If {error}: {action}

### Phase 2: {Phase name}
...

## Tool Coordination
{tool usage strategy}

## Output Format
```
{progress report}
```

## Error Handling
{error scenarios and recovery strategies}

## Boundaries

**Will:**
- {workflows to perform}

**Will Not:**
- {prohibited tasks}

**Safety Checks:**
- {safety checks}
```

---

### Real Example: Release Manager

```yaml
---
name: release-manager
description: "Manages the complete release process for npm packages"
tools: Read,Write,Bash
model: sonnet
---
```

**Complexity**: High
**Word Count**: Approximately 1,500 words
**Workflow**: 5-phase process

**Key Features**:
- Pre-release validation
- Version number update
- Automatic changelog generation
- Build and test
- Deploy and verify
- Automatic rollback on error

---

## 🔀 Type Selection Guide

### Decision Tree

```
What is the nature of the task?
│
├─ Single, clear task?
│  └─ Specialist
│     e.g., formatting, linting, link checking
│
├─ Analysis and review?
│  └─ Analyst
│     e.g., code review, security audit, performance analysis
│
└─ Multi-step workflow?
   └─ Orchestrator
      e.g., deployment, release, CI/CD
```

---

### Determining Type by Questions

**Question 1**: Does the task exceed 3 steps?
- **No** → Consider Specialist
- **Yes** → Next question

**Question 2**: Is the primary purpose analysis/review?
- **Yes** → Analyst
- **No** → Next question

**Question 3**: Do you need to orchestrate multiple tools?
- **Yes** → Orchestrator
- **No** → Specialist

**Question 4**: Is error handling and rollback required?
- **Yes** → Orchestrator
- **No** → Specialist or Analyst

---

## 📊 Detailed Type Comparison

### Execution Time

| Type | Expected Execution Time | Reason |
|------|------------------------|--------|
| Specialist | < 30 seconds | Simple and fast |
| Analyst | 1-3 minutes | Analysis time required |
| Orchestrator | 5-15 minutes | Multi-step workflow |

---

### Prompt Complexity

| Type | Section Count | Word Count | Structure |
|------|--------------|-----------|-----------|
| Specialist | 5-6 | 100-300 | Simple |
| Analyst | 7-9 | 300-800 | Structured |
| Orchestrator | 9-12 | 800-2000+ | Highly complex |

---

### Tool Usage Patterns

| Type | Typical Tool Combination | Description |
|------|-------------------------|-------------|
| Specialist | Read, Write, Bash | Read + execute or write |
| Analyst | Read, Grep, Bash | Read + search + validate |
| Orchestrator | Read, Write, Bash | Orchestrates all tools |

---

### Error Handling Level

| Type | Error Handling | Recovery Strategy |
|------|---------------|------------------|
| Specialist | Basic | Error reporting |
| Analyst | Medium | Error reporting + suggestions |
| Orchestrator | Advanced | Automatic rollback + recovery |

---

## 💡 Practical Selection Tips

### When Choosing Specialist

✅ **Judge like this**:
- "Can I describe this task in one sentence?"
- "Are there 3 or fewer execution steps?"
- "Can it be completed in less than 30 seconds?"

**All Yes** → Specialist

---

### When Choosing Analyst

✅ **Judge like this**:
- "Do analysis results need to be classified by severity?"
- "Does it need to be evaluated from multiple perspectives?"
- "Is it review-only without code modification?"

**All Yes** → Analyst

---

### When Choosing Orchestrator

✅ **Judge like this**:
- "Are 4 or more phases required?"
- "Does it need to rollback to previous state on error?"
- "Do multiple tools need to be orchestrated sequentially?"
- "Would failure have significant impact?"

**Mostly Yes** → Orchestrator

---

## 🎓 Learning Path by Type

### Mastering Specialist

1. **Study Template**: Read `templates/specialist-template.md`
2. **Simple Examples**: ESLint Enforcer, Prettier Formatter
3. **Practice**: Automate simple tasks
4. **Core Learning**: Principle of least privilege, fast execution

---

### Mastering Analyst

1. **Study Template**: Read `templates/analyst-template.md`
2. **Analysis Process**: Learn 4-phase analysis methodology
3. **Practice**: Write code review agent
4. **Core Learning**: Severity classification, constructive feedback

---

### Mastering Orchestrator

1. **Study Template**: Read `templates/orchestrator-template.md`
2. **Workflow Design**: Learn multi-phase structure
3. **Practice**: Write release management agent
4. **Core Learning**: Error handling, rollback strategy, tool orchestration

---

## 📖 References

- **Model Selection**: `model-selection-guide.md`
- **Tool Guide**: `available-tools.md`
- **Validation Criteria**: `validation-criteria.md`
- **Templates**: `templates/` directory
  - `specialist-template.md`
  - `analyst-template.md`
  - `orchestrator-template.md`
